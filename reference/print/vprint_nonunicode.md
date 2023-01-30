# vprint_nonunicode
* print[meta header]
* std[meta namespace]
* function[meta id-type]
* cpp23[meta cpp]

```cpp
namespace std {
  void vprint_nonunicode(string_view fmt,
                         format_args args); // (1) C++23

  void vprint_nonunicode(FILE* stream,
                         string_view fmt,
                         format_args args); // (2) C++23
}
```
* FILE[link /reference/cstdio/file.md.nolink]
* format_args[link /reference/format/basic_format_args.md]

## 概要
書式指定で非Unicode出力する。

- (1) : 標準出力に、書式指定で非Unicode出力する
- (2) : 指定された[`FILE`](/reference/cstdio/file.md.nolink)に、書式指定で非Unicode出力する

[`std::ostream`](/reference/ostream/basic_ostream.md)から派生したクラスオブジェクトに対して出力したい場合は、[`<ostream>`](/reference/ostream.md)ヘッダの[`std::vprint_nonunicode()`](/reference/ostream/vprint_nonunicode.md.nolink)関数を使用すること。


## 事前条件
- (2) : `stream`が有効な出力Cストリームを指していること


## 効果
- (1) : 以下と等価：
    ```cpp
    vprint_unicode(stdout, fmt, args);
    ```
    * stdout[link /reference/cstdio/stdout.md.nolink]

- (2) : [`vformat`](/reference/format/vformat.md)`(fmt, args)`の結果を`stream`に書き出す


## 例外
- [`vformat()`](/reference/format/vformat.md)関数がなんらかの例外を送出する可能性がある
- 端末かストリームへの書き込みに失敗した場合、[`system_error`](/reference/system_error/system_error.md)を送出する
- [`bad_alloc`](/reference/new/bad_alloc.md)を送出する可能性がある


## バージョン
### 言語
- C++23

### 処理系
- [Clang](/implementation.md#clang): ??
- [GCC](/implementation.md#gcc): ??
- [ICC](/implementation.md#icc): ??
- [Visual C++](/implementation.md#visual_cpp): ??


## 関連項目
- [`std::print()`](print.md)
- [`std::println()`](println.md)
- [`std::vprint_unicode()`](vprint_nonunicode.md)


## 参照
- [P2093R14 Formatted output](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2022/p2093r14.html)
