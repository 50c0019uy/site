# tanh
* cmath[meta header]
* std[meta namespace]
* function[meta id-type]
* [mathjax enable]

```cpp
namespace std {
  float tanh(float x);              // (1) C++03からC++20まで
  double tanh(double x);            // (2) C++03からC++20まで
  long double tanh(long double x);  // (3) C++03からC++20まで

  floating-point-type
    tanh(floating-point-type x);    // (4) C++23

  double tanh(Integral x);          // (5) C++11

  float tanhf(float x);             // (6) C++17
  long double tanhl(long double x); // (7) C++17
}
```
* Integral[italic]

## 概要
算術型の双曲線正接（ハイパボリックタンジェント）を求める。

- (1) : `float`に対するオーバーロード
- (2) : `double`に対するオーバーロード
- (3) : `long double`に対するオーバーロード
- (4) : 浮動小数点数型に対するオーバーロード
- (5) : 整数型に対するオーバーロード (`double`にキャストして計算される)
- (6) : `float`型規定
- (7) : `long double`型規定


## 戻り値
引数 `x` の双曲線正接を返す。


## 備考
- $$ f(x) = \tanh x $$
- C++11 以降では、処理系が IEC 60559 に準拠している場合（[`std::numeric_limits`](../limits/numeric_limits.md)`<T>::`[`is_iec559`](../limits/numeric_limits/is_iec559.md)`() != false`）、以下の規定が追加される。（複号同順）
    - `x = ±0` の場合、戻り値は `±0` となる。
    - `x = ±∞` の場合、戻り値は `±1` となる。
- C++23では、(1)、(2)、(3)が(4)に統合され、拡張浮動小数点数型を含む浮動小数点数型へのオーバーロードとして定義された


## 例
```cpp example
#include <cmath>
#include <iostream>

int main() {
  std::cout << std::fixed;
  std::cout << "tanh(-1.0) = " << std::tanh(-1.0) << std::endl;
  std::cout << "tanh(0.0)  = " << std::tanh(0.0) << std::endl;
  std::cout << "tanh(1.0)  = " << std::tanh(1.0) << std::endl;
}
```
* std::tanh[color ff0000]
* std::fixed[link ../ios/fixed.md]

### 出力
```
tanh(-1.0) = -0.761594
tanh(0.0)  = 0.000000
tanh(1.0)  = 0.761594
```

## バージョン
### 言語
- C++03

### 処理系
- [Clang](/implementation.md#clang): 1.9, 2.9, 3.1
- [GCC](/implementation.md#gcc): 3.4.6, 4.2.4, 4.3.5, 4.4.5, 4.5.1, 4.5.2, 4.6.1, 4.7.0
- [ICC](/implementation.md#icc): 10.1, 11.0, 11.1, 12.0
- [Visual C++](/implementation.md#visual_cpp): 2003, 2005, 2008, 2010

#### 備考
特定の環境で `constexpr` 指定されている場合がある。（独自拡張）
- GCC 4.6.1 以上

## 実装例
`tanh` のマクローリン展開はベルヌーイ数が登場するため計算には向かない。

$$ \tanh x = \sum_{n = 1}^{\infty} \frac{B_{2n}4^n(4^n - 1)}{(2n)!} x^{2n - 1} \quad \mathrm{for} \; |x| &lt; \frac{\pi}{2} $$

以下の公式から求めることができる。

$$ \tanh x = \frac{\sinh x}{\cosh x} $$


## 参照
- [P1467R9 Extended floating-point types and standard names](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2022/p1467r9.html)
    - C++23で導入された拡張浮動小数点数型への対応として、`float`、`double`、`long double`のオーバーロードを`floating-point-type`のオーバーロードに統合し、拡張浮動小数点数型も扱えるようにした
