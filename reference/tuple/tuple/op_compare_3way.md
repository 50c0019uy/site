# operator<=>
* tuple[meta header]
* std[meta namespace]
* function template[meta id-type]
* cpp20[meta cpp]

```cpp
namespace std {
  template<class... TTypes, class... UTypes>
  constexpr common_comparison_category_t<synth-three-way-result<TTypes, UTypes>...>
    operator<=>(const tuple<TTypes...>& t, const tuple<UTypes...>& u); // (1) C++20
}
```
* tuple[link ../tuple.md]
* common_comparison_category_t[link /reference/compare/common_comparison_category.md]

## 概要
2つの[`tuple`](../tuple.md)オブジェクトの三方比較を行う。


## 効果
`t`と`u`の辞書順比較を行う。

長さ0の`tuple`の場合は、`t <=> u`は[`strong_ordering`](/reference/compare/strong_ordering.md)`::equal`を返す。そうでなければ、以下と等価：

```cpp
if (auto c = synth-three-way(get<0>(t), get<0>(u)); c != 0)
  return c;
return t tail <=> u tail;
```
* tail[italic]
* get[link get.md]

ここで`r`<sub>tail</sub>は、`r`の最初の要素以外のすべてを含む`tuple`である。


## 備考
- この演算子により、以下の演算子が使用可能になる (C++20)：
    - `operator<`
    - `operator<=`
    - `operator>`
    - `operator>=`


## 例
```cpp example
#include <cassert>
#include <tuple>
#include <string>

int main()
{
  std::tuple<int, char, const char*> t1(1, 'a', "hello");
  std::tuple<int, char, std::string> t2(1, 'a', "hello");
  std::tuple<int, char, std::string> t3(1, 'b', "world");

  assert((t1 <=> t2) == 0); // ※比較可能であれば型は異なっていてもかまわない
  assert(t1 < t3);
  assert(t1 <= t3);
  assert(t3 > t1);
  assert(t3 >= t1);
}
```

### 出力
```
true
false
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
