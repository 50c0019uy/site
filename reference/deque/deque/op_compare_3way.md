# operator<=>
* deque[meta header]
* std[meta namespace]
* function template[meta id-type]
* cpp20[meta cpp]

```cpp
namespace std {
  template <class T, class Allocator>
  synth-three-way-result<T>
    operator==(const deque<T, Allocator>& x, const deque<T, Allocator>& y);
}
```

## 概要
`deque`オブジェクトの三方比較を行う。


## テンプレートパラメータ制約
- 型 (`const`) `T` の値に対して`operator<=>`が定義されるか、型 (`const`) `T` の値に対して`operator<`が定義され全順序をもつこと


## 効果
```cpp
return lexicographical_compare_three_way(
    x.begin(), x.end(),
    y.begin(), y.end(),
    synth-three-way)
```
* lexicographical_compare_three_way[link /reference/algorithm/lexicographical_compare_three_way.md]
* begin()[link begin.md]
* end()[link end.md]


## 計算量
線形時間


## 備考
- この演算子により、以下の演算子が使用可能になる (C++20)：
    - `operator<`
    - `operator<=`
    - `operator>`
    - `operator>=`


## 例
```cpp example
#include <cassert>
#include <deque>

int main ()
{
  std::deque<int> c1 = {1, 2, 3};
  std::deque<int> c2 = {1, 2, 3};
  std::deque<int> c3 = {1, 2, 3, 4};

  // 要素数と要素の値が等しい
  assert((c1 <=> c2) == 0);

  // 要素の値は(左辺の要素数分まで)等しいが要素数が異なる
  assert((c1 <=> c3) != 0);
}
```

### 出力
```
```

## バージョン
### 言語
- C++20

### 処理系
- [Clang](/implementation.md#clang):
- [GCC](/implementation.md#gcc): 10
- [Visual C++](/implementation.md#visual_cpp): ??


## 参照
- [P1614R2 The Mothership has Landed](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2019/p1614r2.html)
    - C++20での三方比較演算子の追加と、関連する演算子の自動導出
