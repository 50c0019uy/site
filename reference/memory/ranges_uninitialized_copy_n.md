# uninitialized_copy_n
* memory[meta header]
* std::ranges[meta namespace]
* function template[meta id-type]
* cpp20[meta cpp]

```cpp
namespace std::ranges {
  template <class I, class O>
  using uninitialized_copy_n_result = in_out_result<I, O>;

  template <input_iterator I,
            no-throw-forward-iterator O,
            no-throw-sentinel<O> S>
  requires constructible_from<iter_value_t<O>, iter_reference_t<I>>
  uninitialized_copy_n_result<I, O>
    uninitialized_copy_n(
      I ifirst,
      iter_difference_t<I> n,
      O ofirst,
      S olast
    );                               // (1) C++20
}
```
* in_out_result[link /reference/algorithm/ranges_in_out_result.md]
* input_iterator[link /reference/iterator/input_iterator.md]
* no-throw-forward-iterator[link no-throw-forward-iterator.md.nolink]
* no-throw-sentinel[link no-throw-sentinel.md.nolink]
* constructible_from[link /reference/concepts/constructible_from.md]
* iter_reference_t[link /reference/iterator/iter_reference_t.md]
* iter_difference_t[link /reference/iterator/iter_difference_t.md]

## 概要
未初期化領域の範囲`[ofirst, ofirst + n)`を配置`new`で`[ifirst, ifirst + n)`の対応する要素から初期化してコピー出力する。

- (1): イテレータペアで範囲を指定する


## テンプレートパラメータ制約
- (1):
    - `I`が[`input_iterator`](/reference/iterator/input_iterator.md)である
    - `O`が[`no-throw-forward-iterator`](no-throw-forward-iterator.md.nolink)である
    - `S`が[`O`に対する例外を投げない番兵](no-throw-sentinel.md.nolink)である
    - `O`の要素型が、`I`の要素型を引数として[構築可能](/reference/concepts/constructible_from.md)である


## 事前条件

- 範囲`[ofirst, olast)`が範囲`ifirst + [0, n)`と重ならないこと


## 効果
以下と等価である：

```cpp
auto t = uninitialized_copy(counted_iterator(ifirst, n),
                            default_sentinel, ofirst, olast);
return {std::move(t.in).base(), t.out};
```
* uninitialized_copy[link ranges_uninitialized_copy.md]
* counted_iterator[link /reference/iterator/counted_iterator.md]
* base()[link /reference/iterator/counted_iterator/base.md]
* default_sentinel[link /reference/iterator/default_sentinel_t.md]
* std::move[link /reference/utility/move.md]


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

  // 未初期化領域pを初期化しつつ範囲vから要素をコピー
  std::ranges::uninitialized_copy_n(v.begin(), size, p, p + size);

  // pの領域が初期化され、かつvからpに要素がコピーされているか確認
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
* std::ranges::uninitialized_copy_n[color ff0000]
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
- [`uninitialized_copy_n`](uninitialized_copy_n.md)

## 参照
- [P9896R4 The One Ranges Proposal](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2018/p0896r4.pdf)
