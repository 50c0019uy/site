# remove_copy_if
* algorithm[meta header]
* std::ranges[meta namespace]
* function template[meta id-type]
* cpp20[meta cpp]

```cpp
namespace std::ranges {
  // (1)
  template<input_iterator I, sentinel_for<I> S, weakly_incrementable O, class Proj = identity, indirect_unary_predicate<projected<I, Proj>> Pred>
    requires indirectly_copyable<I, O>
  constexpr remove_copy_if_result<I, O>
    remove_copy_if(I first, S last, O result, Pred pred, Proj proj = {});

  // (2)
  template<input_range R, weakly_incrementable O, class Proj = identity, indirect_unary_predicate<projected<iterator_t<R>, Proj>> Pred>
    requires indirectly_copyable<iterator_t<R>, O>
  constexpr remove_copy_if_result<borrowed_iterator_t<R>, O>
    remove_copy_if(R&& r, O result, Pred pred, Proj proj = {});
}
```
* input_iterator[link /reference/iterator/input_iterator.md]
* sentinel_for[link /reference/iterator/sentinel_for.md]
* weakly_incrementable[link /reference/iterator/weakly_incrementable.md]
* identity[link /reference/functional/identity.md]
* indirect_unary_predicate[link /reference/iterator/indirect_unary_predicate.md]
* projected[link /reference/iterator/projected.md]
* indirectly_copyable[link /reference/iterator/indirectly_copyable.md]
* remove_copy_result[link ranges_in_out_result.md]
* input_range[link /reference/ranges/input_range.md]
* iterator_t[link /reference/ranges/iterator_t.md]
* borrowed_iterator_t[link /reference/ranges/borrowed_iterator_t.md]

## 概要
条件を満たす要素を除け、その結果を出力の範囲へコピーする。

* (1): イテレーターペアで範囲を指定する
* (2): 範囲を直接指定する

## 事前条件
- `[first,last)` と `[result,result + (last - first)` は重なってはならない。


## 効果
`[first,last)` 内にあるイテレータ `i` について、`pred(*i) != false` でない要素を `result` へコピーする


## 戻り値
`{ .in = last, .out = result + (last - first) }`


## 計算量
正確に `last - first` 回の述語の適用を行う


## 備考
安定


## 例
```cpp example
#include <algorithm>
#include <iostream>
#include <vector>
#include <iterator>

int main() {
  std::vector<int> v = { 2,3,1,2,1 };

  // 奇数を除去した結果を出力する
  std::ranges::remove_copy_if(v, std::ostream_iterator<int>(std::cout, ","), [](int x) { return x%2 != 0; });
}
```
* std::remove_copy_if[color ff0000]

### 出力
```
2,2,
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
