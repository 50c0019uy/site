# out
* format[meta header]
* function[meta id-type]
* std[meta namespace]
* basic_format_context[meta class]
* cpp20[meta cpp]

```cpp
iterator out();
```
* basic_format_arg[link /reference/format/basic_format_arg.md]
* basic_format_context[link /reference/format/basic_format_context.md]

## 概要
出力イテレータを取得する。


## 効果
以下と等価：

```cpp
return std::move(out_);
```
* std::move[link /reference/utility/move.md]

ここで`out_`は、メンバ変数として保持している出力イテレータである。


## バージョン
### 言語
- C++20

### 処理系
- [Clang](/implementation.md#clang): ??
- [GCC](/implementation.md#gcc): 13
- [Visual C++](/implementation.md#visual_cpp): ??

## 参照

- [P0645R10 Text Formatting](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2019/p0645r10.html)
