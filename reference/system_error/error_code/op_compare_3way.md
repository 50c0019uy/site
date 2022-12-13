# operator<=>
* system_error[meta header]
* std[meta namespace]
* function[meta id-type]
* cpp20[meta cpp]

```cpp
namespace std {
  strong_ordering
    operator<=>(const error_code& lhs,
                const error_code& rhs) noexcept; // (1) C++20
}
```

## 概要
`error_code`オブジェクトの三方比較を行う。


## 効果
以下と等価：

```cpp
if (auto c = lhs.category() <=> rhs.category(); c != 0)
  return c;
return lhs.value() <=> rhs.value();
```
* category()[link category.md]
* value()[link value.md]


## 例外
投げない


## 備考
- この演算子により、以下の演算子が使用可能になる (C++20)：
    - `operator<`
    - `operator<=`
    - `operator>`
    - `operator>=`


## 例
```cpp example
#include <cassert>
#include <system_error>

int main()
{
  std::error_code ec1 = std::make_error_code(std::errc::invalid_argument);
  std::error_code ec2 = std::make_error_code(std::errc::invalid_argument);
  std::error_code ec3 = std::make_error_code(std::errc::permission_denied);

  assert((ec1 <=> ec2) == 0);
  assert((ec1 <=> ec3) != 0);
}
```
* std::make_error_code[link /reference/system_error/make_error_code.md]
* std::errc::invalid_argument[link /reference/system_error/errc.md]
* std::errc::permission_denied[link /reference/system_error/errc.md]

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
