# コンストラクタ
* string_view[meta header]
* std[meta namespace]
* basic_string_view[meta class]
* function[meta id-type]
* cpp17[meta cpp]

```cpp
constexpr basic_string_view() noexcept;                       // (1)
constexpr basic_string_view(
            const basic_string_view&) noexcept = default;     // (2)
constexpr basic_string_view(const CharT* str);                // (3)
constexpr basic_string_view(const CharT* str, size_type len); // (4)

template <class It, class End>
constexpr basic_string_view(It first, End last);               // (5)
```

## 概要
- (1) : デフォルトコンストラクタ。空の`basic_string_view`オブジェクトを構築する
- (2) : コピーコンストラクタ。コピー元と同じ文字列を参照する
- (3) : 文字配列を受けとって、その文字配列の全体(ただしヌル文字列を含む場合はそこまで)を参照する
- (4) : 文字配列と長さを受けとって、文字配列`str`の先頭`len`文字を参照する
- (5) : 文字のイテレータ範囲`[first, last)`を参照する


## テンプレートパラメータ制約
- (5) :
    - `It`は[`contiguous_iterator`](/reference/iterator/contiguous_iterator.md)の要件を満たすこと
    - `End`は[`sized_sentinel_for`](/reference/iterator/sized_sentinel_for.md)`<It>`の要件を満たすこと
    - [`is_same_v`](/reference/type_traits/is_same.md)`<`[`iter_value_t`](/reference/iterator/iter_value_t.md)`<It>, charT>`が`true`であること
    - [`is_convertible_v`](/reference/type_traits/is_convertible.md)`<End, size_type>`が`false`であること


## 事前条件
- (3) : 範囲`[str, str + Traits::`[`length`](/reference/string/char_traits/length.md)`(str))`が妥当であること (アクセス可能であること)
- (4) : 範囲`[str, str + len)`が妥当であること
- (5) :
    - 範囲`[first, last)`が妥当であること
    - `It`が[`contiguous_iterator`](/reference/iterator/contiguous_iterator.md)のモデルであること
    - `End`が[`sized_sentinel_for`](/reference/iterator/sized_sentinel_for.md)`<It>`のモデルであること


## 効果
メンバ変数として、参照する文字配列へのポインタを`const CharT* data_`、文字数を`size_type size_`があるものとして、

- (1) : `data_ = nullptr;`および`size_ = 0;`とする
- (3) : `data_ = str;`および`size_ = Traits::`[`length`](/reference/string/char_traits/length.md)`(str);`とする
- (4) : `data_ = str;`および`size_ = len;`とする
- (5) : `data_ =` [`to_address`](/reference/memory/to_address.md)`(first);`および`size_ = last - first;`とする


## 計算量
- (1), (4) : 定数時間
- (3): 文字数に対して線形時間

## 備考
- `basic_string_view`のコンストラクタに`template<size_t N>basic_string_view(const charT (&str)[N])`タイプの配列を受け取るコンストラクタが無いのは次の使い方をしたとき`str`のサイズが`sizeof(buf)`となり、それは利用者の意図しない挙動になる可能性が高いと判断されたからである。

```cpp example
char buf[128];
snprintf(buf, sizeof(buf), "abc");
string_view str(buf);
```

- ヌル文字を含む文字列リテラル全体から`basic_string_view`を構築したい場合は[`std::string_view_literals::svリテラル`](op_sv.html)を用いる。


## 例
```cpp example
#include <cassert>
#include <iostream>
#include <string_view>

int main()
{
  // (1)
  // デフォルト構築
  {
    std::string_view sv;
    assert(sv.data() == nullptr);
    assert(sv.size() == 0);
  }

  // (2)
  // コピーコンストラクタ。コピー元と同じ文字列を参照する
  {
    std::string_view base{"Hello World"};
    std::string_view sv = base;

    std::cout << "(2) : " << sv << std::endl;
  }

  // (3)
  // 文字配列を受けとって参照するコンストラクタ
  {
    std::string_view sv = "Hello World";
    std::cout << "(3) : " << sv << std::endl;
  }

  // (4)
  // 文字配列と文字数を受けとって部分文字列を参照するコンストラクタ
  {
    // "Hello World"の先頭5文字"Hello"を参照
    std::string_view sv{"Hello World", 5};
    std::cout << "(4) : " << sv << std::endl;
  }

  // (5)
  // 文字のイテレータ範囲を受け取って参照するコンストラクタ
  {
    std::string s = "Hello World";
    std::string_view sv{s.begin(), s.begin() + 5};
    std::cout << "(5) : " << sv << std::endl;
  }
}
```
* sv.data()[link data.md]
* sv.size()[link size.md]
* s.begin()[link /reference/string/basic_string/begin.md]

### 出力
```
(2) : Hello World
(3) : Hello World
(4) : Hello
(5) : Hello
```

## バージョン
### 言語
- C++17

### 処理系
- [Clang](/implementation.md#clang): 4.0 (5以外)
- [GCC](/implementation.md#gcc): 7.1 (5以外), 10.1
- [ICC](/implementation.md#icc): ??
- [Visual C++](/implementation.md#visual_cpp): ??

## 参照
- [ISO/IEC JTC1 SC22 WG21 N3762](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2013/n3762.html#avoid-strlen)
- [P1391R4 Range constructor for `std::string_view`](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2019/p1391r4.pdf)
