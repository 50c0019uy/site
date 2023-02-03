# error
* expected[meta header]
* function[meta id-type]
* std[meta namespace]
* expected[meta class]
* cpp23[meta cpp]

```cpp
constexpr const E& error() const & noexcept;   // (1)
constexpr E& error() & noexcept;               // (2)
constexpr const E&& error() const && noexcept; // (3)
constexpr E&& error() && noexcept;             // (4)
```

## 概要
エラー値を取得する。


## 事前条件
[`has_value()`](has_value.md) `== false`


## 戻り値
動作説明用のメンバ変数として、エラー値を保持する`unex`を導入する。

- (1), (2) : エラー値を保持していたら、`unex`
- (3), (4) : エラー値を保持していたら、[`std::move`](/reference/utility/move.md)`(unex)`


## 例外
投げない


## 例
```cpp example
#include <cassert>
#include <expected>
#include <iostream>
#include <string>

int main()
{
  std::expected<int, std::string> x = std::unexpected{"ERR"};
  assert(not x.has_value());
  std::cout << x.error() << std::endl;
}
```
* error()[color ff0000]
* has_value()[link has_value.md]
* std::unexpected[link ../unexpected.md]

### 出力
```
ERR
```


## バージョン
### 言語
- C++23

### 処理系
- [Clang](/implementation.md#clang): 16.0
- [GCC](/implementation.md#gcc): 12.1
- [ICC](/implementation.md#icc): ??
- [Visual C++](/implementation.md#visual_cpp): ??


## 関連項目
- [`error_or`](error_or.md)


## 参照
- [P0323R12 std::expected](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2022/p0323r12.html)
