# デストラクタ
* expected[meta header]
* function[meta id-type]
* std[meta namespace]
* expected<void,E>[meta class]
* cpp23[meta cpp]

```cpp
// expected<cv void, E>部分特殊化
constexpr ~expected();
```

## 概要
`expected`オブジェクトを破棄する。


## 効果
エラー値を保持している場合は、エラー値オブジェクトのデストラクタを呼び出す。


## トリビアルに定義される条件
[`is_trivially_destructible_v`](/reference/type_traits/is_trivially_destructible.md)`<E>`


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
