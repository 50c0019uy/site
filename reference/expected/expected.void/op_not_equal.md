# operator!=
* expected[meta header]
* function template[meta id-type]
* std[meta namespace]
* expected.void[meta class]
* cpp23[meta cpp]

```cpp
// expected<cv void, E>部分特殊化
// operator==により、下記オーバーロードが使用可能になる
template<class T2, class E2> requires is_void_v<T2>
friend constexpr bool operator!=(const expected& x, const expected<T2, E2>& y); // (1)

template<class E2>
friend constexpr bool operator!=(const expected& x, const unexpected<E2>& e);   // (2)
template<class E2>
friend constexpr bool operator!=(const unexpected<E2>& e, const expected& x);   // (3)
```
* operator==[link op_equal.md]
* is_void_v[link /reference/type_traits/is_void.md]
* unexpected[link ../unexpected.md]

## 概要
- (1) : `expected`オブジェクト同士の非等値比較を行う。
- (2), (3) : `expected`オブジェクトとエラー値の非等値比較を行う。


## 適格要件
- (1) : 式`x.`[`error()`](error.md) `== y.`[`error()`](error.md)が適格であり、その結果を`bool`へ変換可能であること。
- (2), (3) : 式`x.`[`error()`](error.md) `== e.`[`error()`](../unexpected/error.md)が適格であり、その結果を`bool`へ変換可能であること。


## 戻り値
- (1) : `!`[`(x == y)`](op_equal.md)
- (2), (3) : `!`[`(x == e)`](op_equal.md)


## 例
```cpp example
#include <cassert>
#include <expected>

int main()
{
  std::expected<void, long>  x1;
  std::expected<void, short> y1;
  std::expected<void, long>  x2 = std::unexpected{1};
  std::expected<void, short> y2 = std::unexpected{100};

  // (1)
  assert(not (x1 != y1));
  assert(x2 != y2);
  assert(x1 != y2);
  assert(x2 != y1);

  // (2), (3)
  assert(x2 != std::unexpected{2});
  assert(std::unexpected{2} != x2);
}
```
* !=[color ff0000]
* std::unexpected[link ../unexpected.md]

### 出力
```
```


## バージョン
### 言語
- C++23

### 処理系
- [Clang](/implementation.md#clang): 16.0
- [GCC](/implementation.md#gcc): 12.1
- [ICC](/implementation.md#icc): ??
- [Visual C++](/implementation.md#visual_cpp): ??


## 参照
- [P0323R12 std::expected](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2022/p0323r12.html)
