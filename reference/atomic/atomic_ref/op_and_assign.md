# operator&=
* atomic[meta header]
* std[meta namespace]
* atomic_ref[meta class]
* function[meta id-type]
* cpp20[meta cpp]

```cpp
T
  operator&=(T operand) const noexcept;          // (1) C++20
constexpr value_type
  operator&=(value_type operand) const noexcept; // (1) C++26
```

## 概要
AND演算を行う


## テンプレートパラメータ制約
- C++26 : [`is_const_v`](/reference/type_traits/is_const.md)`<T>`が`false`であること


## 戻り値
以下と等価の式により、演算結果の値が返る：

```cpp
return fetch_and(operand) & operand;
```
* fetch_and[link fetch_and.md]


## 例外
投げない


## 備考
この関数は、`atomic_ref`クラスの整数型に対する特殊化で定義される。


## 例
### 基本的な使い方
```cpp example
#include <iostream>
#include <atomic>
#include <bitset>

int main()
{
  int value = 0b1011;
  std::atomic_ref<int> x{value};

  x &= 0b1110;

  std::cout << std::bitset<4>(value).to_string() << std::endl;
}
```
* to_string()[link /reference/bitset/bitset/to_string.md]

#### 出力
```
1010
```

### 複数スレッドからビット複合演算を行う例
```cpp example
#include <iostream>
#include <atomic>
#include <thread>
#include <bitset>

int main()
{
  int value = 0b1111;

  // 複数スレッドでビット複合演算を呼んでも、
  // 最終的に全てのスレッドでのビット複合演算が処理された値になる
  std::thread t1 {[&value] {
    std::atomic_ref{value} &= 0b0111;
  }};
  std::thread t2 {[&value] {
    std::atomic_ref{value} &= 0b0101;
  }};

  t1.join();
  t2.join();

  std::cout << std::bitset<4>(value).to_string() << std::endl;
}
```
* to_string()[link /reference/bitset/bitset/to_string.md]

#### 出力
```
0101
```


## バージョン
### 言語
- C++20

### 処理系
- [Clang](/implementation.md#clang): 9.0 [mark noimpl]
- [GCC](/implementation.md#gcc): 10.1 [mark verified]
- [Visual C++](/implementation.md#visual_cpp): ??


## 参照
- [P3323R1 cv-qualified types in `atomic` and `atomic_ref`](https://open-std.org/jtc1/sc22/wg21/docs/papers/2024/p3323r1.html)
    - C++26でCV修飾されたテンプレート引数を受け取れるようになった
- [P3309R3 `constexpr atomic` and `atomic_ref`](https://open-std.org/jtc1/sc22/wg21/docs/papers/2024/p3309r3.html)
    - C++26で`constexpr`に対応した
