# operator<
* set[meta header]
* std[meta namespace]
* function template[meta id-type]

```cpp
namespace std {
  // operator<=>により、以下の演算子が使用可能になる (C++20)
  template <class Key, class Compare, class Allocator>
  bool
    operator<(const set<Key,Compare,Allocator>& x,
              const set<Key,Compare,Allocator>& y); // (1) C++03
}
```

## 概要
`set`において、左辺が右辺より小さいかの判定を行う。


## パラメータ
- `x`, `y`<br/>
比較するコンテナ


## 戻り値
```cpp
lexicographical_compare(x.begin(), x.end(), y.begin(), y.end());
```
* lexicographical_compare[link /reference/algorithm/lexicographical_compare.md]
* begin()[link begin.md]
* end()[link end.md]


## 計算量
[`size()`](size.md) に対して線形時間


## 例
```cpp example
#include <iostream>
#include <set>

int main ()
{
  std::set<int> s1 = {1, 2, 3};
  std::set<int> s2 = {4, 5, 6};

  std::cout << std::boolalpha;

  std::cout << (s1 < s2) << std::endl;
}
```
* <[color ff0000]

### 出力
```
true
```


## 参照
- [P1614R2 The Mothership has Landed](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2019/p1614r2.html)
    - C++20での三方比較演算子の追加と、関連する演算子の自動導出
