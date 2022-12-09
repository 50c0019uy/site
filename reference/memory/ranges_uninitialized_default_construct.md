# uninitialized_default_construct
* memory[meta header]
* std::ranges[meta namespace]
* function template[meta id-type]
* cpp20[meta cpp]

```cpp
namespace std::ranges {
  template <no-throw-forward-iterator I, no-throw-sentinel<I> S>
    requires default_initializable<iter_value_t<I>>
  I uninitialized_default_construct(I first, S last);            // (1) C++20

  template <no-throw-forward-range R>
    requires default_initializable<range_value_t<R>>
  borrowed_iterator_t<R> uninitialized_default_construct(R&& r); // (2) C++20
}
```
* no-throw-forward-iterator[link no-throw-forward-iterator.md]
* no-throw-sentinel[link no-throw-sentinel.md]
* default_initializable[link /reference/concepts/default_initializable.md]
* iter_value_t[link /reference/iterator/iter_value_t.md]
* no-throw-forward-range[link no-throw-forward-range.md.nolink]
* range_value_t[link /reference/ranges/range_value_t.md]
* borrowed_iterator_t[link /reference/ranges/borrowed_iterator_t.md]

## 概要
未初期化領域の範囲 (`r`、`[first, last)`) の各要素をデフォルト構築する。

- (1): イテレータペアで範囲を指定する
- (2): 範囲を直接指定する


## テンプレートパラメータ制約
- (1):
    - `I`が[`no-throw-forward-iterator`](no-throw-forward-iterator.md)である
    - `S`が[`I`に対する例外を投げない番兵](no-throw-sentinel.md)である
    - `I`の要素型が、[デフォルト構築可能](/reference/concepts/default_initializable.md)である
- (2):
    - `R`が[`no-throw-forward-range`](no-throw-forward-range.md.nolink)である
    - `R`の要素型が、[デフォルト構築可能](/reference/concepts/default_initializable.md)である


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
for (; first != last; ++first)
  ::new (voidify(*first)) remove_reference_t<iter_reference_t<I>>;
return first;
```
* remove_reference_t[link /reference/type_traits/remove_reference.md]
* iter_reference_t[link /reference/iterator/iter_reference_t.md]


## 備考
- [`std::vector`](/reference/vector/vector.md)クラスの要素数を変更する操作は、要素を値構築するためゼロ初期化が行われる。その値初期化のコストが気になるような場合に、デフォルト構築することでプログラマの責任で必要な分だけ任意に初期化でき、パフォーマンス向上が期待できるようになる。
     - 例としてBoost Container Libraryの`vector`クラスには、要素数を変更するメンバ関数にデフォルト構築のオプションとして[`default_init`](https://www.boost.org/doc/libs/release/doc/html/container/extended_functionality.html#container.extended_functionality.default_initialialization)がある


## 例
```cpp example
#include <iostream>
#include <memory>
#include <algorithm>

struct Vector {
  int x, y;
};

int main()
{
  std::allocator<Vector> alloc;

  // メモリ確保。
  // この段階では、[p, p + size)の領域は未初期化
  const std::size_t size = 3;
  Vector* p = alloc.allocate(size);

  // 未初期化領域[p, p + size)の各要素をデフォルト構築
  std::ranges::uninitialized_default_construct(std::ranges::subrange{p, p + size});

  // pの領域が初期化され、かつ範囲pの全ての要素が2で埋められているか確認
  std::for_each(p, p + size, [](const Vector& v) {
    std::cout << v.x << ',' << v.y << std::endl;
  });

  // 要素を破棄
  for (std::size_t i = 0; i < size; ++i) {
    std::destroy_at(p + i);
  }

  // メモリ解放
  alloc.deallocate(p, size);
}
```
* std::ranges::uninitialized_default_construct[color ff0000]
* std::ranges::subrange[link /reference/ranges/subrange.md]
* std::allocator[link allocator.md]
* alloc.allocate[link allocator/allocate.md]
* std::destroy_at[link destroy_at.md]
* alloc.deallocate[link allocator/deallocate.md]

### 出力例
```
1445540552,1445540279
0,1445540279
0,1445540279
```


## バージョン
### 言語
- C++20

### 処理系
- [Clang](/implementation.md#clang): 16.0
- [GCC](/implementation.md#gcc): 10.2.0
- [Visual C++](/implementation.md#visual_cpp): 2019 Update 10


## 関連項目
- [`uninitialized_default_construct`](uninitialized_default_construct.md)

## 参照
- [P9896R4 The One Ranges Proposal](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2018/p0896r4.pdf)
