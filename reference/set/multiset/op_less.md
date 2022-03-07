# operator<
* set[meta header]
* std[meta namespace]
* function template[meta id-type]

```cpp
namespace std {
  template <class Key, class Compare, class Allocator>
  bool operator< (const multiset<Key,Compare,Allocator>& x, const multiset<Key,Compare,Allocator>& y);
}
```

## 概要
`multiset`において、左辺が右辺より小さいかの判定を行う。


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
[`size()`](size.md) に対して線形時間。


## 例
```cpp example
#include <iostream>
#include <set>

int main ()
{
  std::multiset<int> s1 = {1, 2, 3};
  std::multiset<int> s2 = {4, 5, 6};

  std::cout << std::boolalpha;

  std::cout << (s1 < s2) << std::endl;
}
```
* <[color ff0000]

### 出力
```
true
```
