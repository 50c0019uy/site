# partition_copy
* algorithm[meta header]
* std::ranges[meta namespace]
* function template[meta id-type]
* cpp20[meta cpp]
* cpp11[meta cpp]

```cpp
namespace std {
  template <class InputIterator,
            class OutputIterator1,
            class OutputIterator2,
            class Predicate>
  pair<OutputIterator1, OutputIterator2>
    partition_copy(InputIterator first,
                   InputIterator last,
                   OutputIterator1 out_true,
                   OutputIterator2 out_false,
                   Predicate pred);            // (1) C++11

  template <class InputIterator,
            class OutputIterator1,
            class OutputIterator2,
            class Predicate>
  constexpr pair<OutputIterator1, OutputIterator2>
    partition_copy(InputIterator first,
                   InputIterator last,
                   OutputIterator1 out_true,
                   OutputIterator2 out_false,
                   Predicate pred);            // (1) C++20

  template <class ExecutionPolicy,
            class ForwardIterator,
            class ForwardIterator1,
            class ForwardIterator2,
            class Predicate>
  pair<ForwardIterator1, ForwardIterator2>
    partition_copy(ExecutionPolicy&& exec,
                   ForwardIterator first,
                   ForwardIterator last,
                   ForwardIterator1 out_true,
                   ForwardIterator2 out_false,
                   Predicate pred);            // (2) C++17
}
```
* pair[link /reference/utility/pair.md]

## 概要
与えられた範囲を条件によって 2 つの出力の範囲へ分けてコピーする。


## 要件
- `InputIterator` の value type は `Assignable` で、`out_true` と `out_false` の `OutputIterator` へ書き込み可能で、`Predicate` の argument type へ変換可能でなければならない。
- 入力範囲は出力範囲のどちらとも重なっていてはならない。


## 効果
`[first,last)` 内にあるそれぞれのイテレータ `i` について、`pred(*i)` が `true` なら `*i` を `out_true` へコピーし、そうでない場合は `out_false` へコピーする。


## 戻り値
`first` には `out_true` の終端が、`second` には `out_false` の終端が格納された[`pair`](/reference/utility/pair.md)オブジェクトを返す。


## 計算量
正確に `last - first` 回述語が適用される。


## 例
```cpp example
#include <iostream>
#include <vector>
#include <algorithm>
#include <iterator>
#include <string>

void print(const std::string& name, const std::vector<int>& v)
{
  std::cout << name << " : ";
  std::for_each(v.begin(), v.end(), [](int x) {
    std::cout << x << ",";
  });
  std::cout << std::endl;
}

bool is_even(int x) { return x % 2 == 0; }

int main()
{
  std::vector<int> v = {1, 2, 3, 4, 5};

  // 偶数グループと奇数グループに分ける
  std::vector<int> evens;
  std::vector<int> odds;
  std::partition_copy(v.begin(), v.end(),
                      std::back_inserter(evens),
                      std::back_inserter(odds),
                      is_even);

  print("v", v);
  print("evens", evens);
  print("odds", odds);
}
```
* std::partition_copy[color ff0000]

### 出力
```
v : 1,2,3,4,5,
evens : 2,4,
odds : 1,3,5,
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
