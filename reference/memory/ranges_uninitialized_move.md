# uninitialized_move
* memory[meta header]
* std::ranges[meta namespace]
* function template[meta id-type]
* cpp20[meta cpp]

```cpp
namespace std::ranges {
  template <class I, class O>
  using uninitialized_move_result = in_out_result<I, O>;

  template <input_iterator I, sentinel_for<I> S1,
            no-throw-forward-iterator O, no-throw-sentinel<O> S2>
  requires constructible_from<iter_value_t<O>, iter_rvalue_reference_t<I>>
  uninitialized_move_result<I, O>
    uninitialized_move(I ifirst, S1 ilast, O ofirst, S2 olast); // (1) C++20

  template <input_range IR, no-throw-forward-range OR>
  requires constructible_from<range_value_t<OR>, range_rvalue_reference_t<IR>>
  uninitialized_move_result<borrowed_iterator_t<IR>, borrowed_iterator_t<OR>>
    uninitialized_move(IR&& in_range, OR&& out_range);          // (2) C++20
}
```
* in_out_result[link /reference/algorithm/ranges_in_out_result.md]
* input_iterator[link /reference/iterator/input_iterator.md]
* sentinel_for[link /reference/iterator/sentinel_for.md]
* no-throw-forward-iterator[link no-throw-forward-iterator.md.nolink]
* no-throw-sentinel[link no-throw-sentinel.md.nolink]
* constructible_from[link /reference/concepts/constructible_from.md]
* iter_rvalue_reference_t[link /reference/iterator/iter_rvalue_reference_t.md]
* input_range[link /reference/ranges/input_range.md]
* no-throw-forward-range[link no-throw-forward-range.md.nolink]
* range_value_t[link /reference/ranges/range_value_t.md]
* range_rvalue_reference_t[link /reference/ranges/range_rvalue_reference_t.md]
* borrowed_iterator_t[link /reference/ranges/borrowed_iterator_t.md]

## 概要
未初期化領域の範囲（`out_range`、`[ofirst, olast)`）を配置`new`で入力範囲（`in_range`、`[ifirst, ilast)`）の対応する要素から初期化してムーブ出力する。

- (1): イテレータペアで範囲を指定する
- (2): 範囲を直接指定する


## テンプレートパラメータ制約
- (1):
    - `I`が[`input_iterator`](/reference/iterator/input_iterator.md)である
    - `S1`が[`I`に対する番兵](/reference/iterator/sentinel_for.md)である
    - `O`が[`no-throw-forward-iterator`](no-throw-forward-iterator.md.nolink)である
    - `S2`が[`O`に対する例外を投げない番兵](no-throw-sentinel.md.nolink)である
    - `O`の要素型が、`I`の要素型の右辺値を引数として[構築可能](/reference/concepts/constructible_from.md)である
- (2):
    - `IR`が[`input_range`](/reference/ranges/input_range.md)である
    - `OR`が[`no-throw-forward-range`](no-throw-forward-range.md.nolink)である
    - `OR`の要素型が、`IR`の要素型の右辺値を引数として[構築可能](/reference/concepts/constructible_from.md)である


## 事前条件

- 範囲`[ofirst, olast)`が範囲`[ifirst, ilast)`と重ならないこと


## 効果
説明用の関数`voidify`があるとして、

```cpp
template<class T>
constexpr void* voidify(T& obj) noexcept {
  return const_cast<void*>(static_cast<const volatile void*>(addressof(obj)));
}
```
* addressof[link addressof.md]


以下と等価である：

```cpp
for (; ifirst != ilast && ofirst != olast; ++ofirst, (void)++ifirst) {
  ::new (voidify(*ofirst)) remove_reference_t<iter_reference_t<O>>(ranges::iter_move(*ifirst));
}
return {std::move(ifirst), ofirst};
```
* remove_reference_t[link /reference/type_traits/remove_reference.md]
* iter_reference_t[link /reference/iterator/iter_reference_t.md]
* std::move[link /reference/utility/move.md]
* ranges::iter_move[link /reference/iterator/iter_move.md]


## 例
```cpp example
#include <iostream>
#include <memory>

#include <vector>
#include <algorithm>

int main()
{
  const std::vector<int> v = {1, 2, 3};

  std::allocator<int> alloc;

  // メモリ確保。
  // この段階では、[p, p + size)の領域は未初期化
  const std::size_t size = 3;
  int* p = alloc.allocate(size);

  // 未初期化領域pを初期化しつつ範囲vから要素をムーブ
  std::ranges::uninitialized_move(v, std::ranges::subrange{p, p + size});

  // pの領域が初期化され、かつvからpに要素がムーブされているか確認
  std::for_each(p, p + size, [](int x) {
    std::cout << x << std::endl;
  });

  // 要素を破棄
  for (std::size_t i = 0; i < size; ++i) {
    std::destroy_at(p + i);
  }

  // メモリ解放
  alloc.deallocate(p, size);
}
```
* std::ranges::uninitialized_move[color ff0000]
* std::ranges::subrange[link /reference/ranges/subrange.md]
* std::allocator[link allocator.md]
* alloc.allocate[link allocator/allocate.md]
* std::destroy_at[link destroy_at.md]
* alloc.deallocate[link allocator/deallocate.md]

### 出力
```
1
2
3
```


## バージョン
### 言語
- C++20

### 処理系
- [Clang](/implementation.md#clang): 16.0
- [GCC](/implementation.md#gcc): 10.2.0
- [Visual C++](/implementation.md#visual_cpp): 2019 Update 10


## 関連項目
- [`uninitialized_move`](uninitialized_move.md)

## 参照
- [P9896R4 The One Ranges Proposal](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2018/p0896r4.pdf)
