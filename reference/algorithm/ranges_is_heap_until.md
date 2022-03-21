# is_heap_until
* algorithm[meta header]
* std::ranges[meta namespace]
* function template[meta id-type]
* cpp20[meta cpp]

```cpp
namespace std::ranges {
  // (1)
  template<random_access_iterator I, sentinel_for<I> S, class Proj = identity, indirect_strict_weak_order<projected<I, Proj>> Comp = ranges::less>
  constexpr I is_heap_until(I first, S last, Comp comp = {}, Proj proj = {});

  // (2)
  template<random_access_range R, class Proj = identity, indirect_strict_weak_order<projected<iterator_t<R>, Proj>> Comp = ranges::less>
  constexpr borrowed_iterator_t<R> is_heap_until(R&& r, Comp comp = {}, Proj proj = {});
}
```
* random_access_iterator[link /reference/iterator/random_access_iterator.md]
* sentinel_for[link /reference/iterator/sentinel_for.md]
* ranges::less[link /reference/functional/ranges_less.md]
* identity[link /reference/functional/identity.md]
* indirect_strict_weak_order[link /reference/iterator/indirect_strict_weak_order.md]
* random_access_range[link /reference/ranges/random_access_range.md]
* iterator_t[link /reference/ranges/iterator_t.md]

## 概要
範囲がヒープ化されているか判定し、ヒープ化されていない最初の要素を指すイテレータを取得する。

* (1): イテレーターペアで範囲を指定する
* (2): 範囲を直接指定する


## 戻り値
[`distance`](/reference/iterator/distance.md)`(first, last) < 2` の場合は `last` を返す。そうでない場合、`[first,last]` 内のイテレータ `i` について、`[first,i)` が `heap` であるような最後の `i` を返す。


## 計算量
線形時間


## 例
```cpp example
#include <iostream>
#include <vector>
#include <algorithm>

int main()
{
  std::vector<int> v = {3, 1, 4};

  std::cout << std::boolalpha;

  std::cout << "before: is heap? "
            << (std::ranges::is_heap_until(v) == v.end()) << std::endl;

  std::ranges::make_heap(v);
  std::cout << " after: is heap? "
            << (std::ranges::is_heap_until(v) == v.end()) << std::endl;
}
```
* std::ranges::is_heap_until[color ff0000]
* std::ranges::make_heap[link ranges_make_heap.md]

### 出力
```
before: is heap? false
 after: is heap? true
```

## バージョン
### 言語
- C++20

### 処理系
- [Clang](/implementation.md#clang): ??
- [GCC](/implementation.md#gcc): 10.1.0
- [ICC](/implementation.md#icc): ??
- [Visual C++](/implementation.md#visual_cpp): 2019 Update 10

## 参照
- [N4861 25 Algorithms library](https://timsong-cpp.github.io/cppwp/n4861/algorithms)
