# range_formatter
* format[meta header]
* class template[meta id-type]
* std[meta namespace]
* cpp23[meta cpp]

```cpp
namespace std {
  template <class T, class charT = char>
    requires same_as<remove_cvref_t<T>, T> && formattable<T, charT>
  class range_formatter;
}
```
* formattable[link formattable.md]

## 概要
`range_formatter`は、Range・コンテナに対する[`formatter`](formatter.md)クラスの特殊化を実装するためのユーティリティクラスである。

ユーザー定義のコンテナ・RangeをRange書式に対応する場合は、以下のようにする：

- オリジナル書式を定義しないのであれば、このクラスではなく、[`format_kind`](format_kind.md)を特殊化する
- オリジナル書式を定義するのであれば、このクラスおよび[`format_kind`](format_kind.md)を特殊化して[`parse()`](range_formatter/parse.md.nolink)メンバ関数と[`format()`](range_formatter/format.md.nolink)メンバ関数を実装する


## メンバ関数

| メンバ関数 | 説明 | 対応バージョン |
|------------|------|----------------|
| [`set_separator`](range_formatter/set_separator.md.nolink) | 要素の区切り文字を設定する | C++23 |
| [`set_brackets`](range_formatter/set_brackets.md.nolink)   | 全体の囲み文字を設定する | C++23 |
| [`underlying`](range_formatter/underlying.md.nolink)       | 要素型の`formatter`を取得する | C++23 |
| [`parse`](range_formatter/parse.md.nolink)                 | 書式の解析を行う | C++23 |
| [`format`](range_formatter/format.md.nolink)               | 書式化を行う | C++23 |


## 例
### オリジナル書式を定義する例
```cpp example
#include <iostream>
#include <format>
#include <vector>

template <class T>
class std::range_formatter<std::vector<T>> : public std::range_formatter<std::vector<T>> {
  bool is_colon = false;
  using base_type = std::range_formatter<std::vector<T>>;
public:

  // コンパイル時の書式文字列の解析があるため、
  // constexprにする必要がある。
  // この関数に渡されるパラメータは、{:%j}の%以降。
  // 解析がおわった場所を指すイテレータを返す。
  constexpr auto parse(std::format_parse_context& ctx) {
    auto it = ctx.begin();
    if (*it == 'c') {
      is_colon = true;
      ++it;
    }
    ctx.advance_to(it);
    return base_type::parse(ctx);
  }

  // format()関数は書式の情報をもたない。
  // parse()関数で解析した書式をメンバ変数で保持しておいて、
  // それをもとに書式化する
  auto format(const std::vector<T>& v, std::format_context& ctx) const {
    if (is_colon) {
      auto out = ctx.out();
      bool is_first = true;
      for (const T& x : v) {
        if (is_first) {
          is_first = false;
        }
        else {
          *out = ':';
          ++out;
        }
        ctx.advance_to(out);
        out = underlying().format(x, ctx);
      }
      return out;
    }
    return base_type::format(v, ctx);
  }
};

int main()
{
  std::vector<std::uint8_t> v = {0xaa, 0xbb, 0xcc, 0xdd, 0xee, 0xff};
  std::cout << std::format("{:c:02x}", v) << std::endl;
}
```
* std::format_parse_context[link basic_format_parse_context.md]
* ctx.begin()[link basic_format_parse_context/begin.md]
* std::format_context[link basic_format_context.md]
* ctx.out()[link basic_format_context/out.md]
* std::format_to[link format_to.md]
* std::format[link format.md]
* underlying()[link range_formatter/underlying.md.nolink]


#### 出力
```
aa:bb:cc:dd:ee:ff
```

(動作確認はできていない)

## バージョン
### 言語
- C++23

### 処理系
- [Clang](/implementation.md#clang): ??
- [GCC](/implementation.md#gcc): ??
- [ICC](/implementation.md#icc): ??
- [Visual C++](/implementation.md#visual_cpp): ??

## 関連項目
- [`range-default-formatter`](range-default-formatter.md.nolink)
- [`formatter`](formatter.md)


## 参照
- [P2286R8 Formatting Ranges](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2022/p2286r8.html)
- [P2585R1 Improve default container formatting](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2022/p2585r1.html)
    - C++23から、Range・コンテナ、`pair`、`tuple`のフォーマット出力、および文字・文字列のデバッグ指定 (`"?"`) が追加された
