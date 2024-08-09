# state
* locale[meta header]
* std[meta namespace]
* wstring_convert[meta class]
* function[meta id-type]
* cpp11[meta cpp]
* cpp17deprecated[meta cpp]
* cpp26removed[meta cpp]

```cpp
state_type state() const;
```

このクラスはC++17から非推奨となり、C++26で削除された。

## 概要
変換の状態を取得する。


## 戻り値
これによって返される状態は、初期状態か、部分的に変換した状態かのどちらかである。

全ての文字が変換された場合は、初期状態が設定される。


## 例外
`state_type`型のコピーコンストラクタが例外を送出しない限り、この関数は例外を送出しない。


## 例
```cpp example
#include <iostream>
#include <string>
#include <locale>
#include <codecvt>
#include <cwchar>

int main()
{
  // UTF-8とUTF-32の相互変換を行うコンバーター
  std::wstring_convert<std::codecvt_utf8<char32_t>, char32_t> converter;

  std::string input = u8"あいうえお";
  std::u32string result = converter.from_bytes(input);
  std::mbstate_t state = converter.state();

  // 全ての文字が変換された
  if (std::mbsinit(&state) != 0) {
    std::cout << "converted all" << std::endl;
  }
  // 変換されなかった文字がある
  else {
    std::cout << "converted partial" << std::endl;
  }
}
```
* std::codecvt_utf8[link /reference/codecvt/codecvt_utf8.md]
* std::mbstate_t[link /reference/cwchar/mbstate_t.md.nolink]
* std::mbsinit[link /reference/cwchar/mbsinit.md.nolink]

### 出力
```
converted all
```


## バージョン
### 言語
- C++11

### 処理系
- [Clang](/implementation.md#clang): 3.0 [mark verified], 3.1 [mark verified], 3.2 [mark verified], 3.3 [mark verified], 3.4 [mark verified]
- [GCC](/implementation.md#gcc): 5.1 [mark verified]
- [ICC](/implementation.md#icc):
- [Visual C++](/implementation.md#visual_cpp): 2010 [mark verified], 2012 [mark verified], 2013 [mark verified]

## 参照
- [P2872R3 Remove `wstring_convert` From C++26](https://open-std.org/jtc1/sc22/wg21/docs/papers/2024/p2872r3.pdf)
