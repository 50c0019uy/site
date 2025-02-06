# begin
* flat_set[meta header]
* std[meta namespace]
* flat_set[meta class]
* function[meta id-type]
* cpp23[meta cpp]

```cpp
iterator begin() noexcept;
const_iterator begin() const noexcept;
```


## 概要
コンテナの先頭要素を参照するイテレータを取得する。

内部的に、コンテナは要素を下位から上位へと並べており、従って`begin()`はコンテナ内の最下位のキーにあたる値へのイテレータを返す。


## 戻り値
コンテナの先頭要素へのイテレータ。
`iterator` と `const_iterator` はともにメンバ型である。このクラステンプレートにおいて、これらはランダムアクセスイテレータである。


## 計算量
定数時間。


## 例
```cpp example
#include <flat_set>
#include <iostream>

int main()
{
  std::flat_set<int> fs = {3, 1, 4};

  for (auto i = fs.begin(); i != fs.end(); ++i) {
    std::cout << *i << std::endl;
  }
}
```
* begin()[color ff0000]
* fs.end()[link end.md]

### 出力
```
1
3
4
```

## バージョン
### 言語
- C++23

### 処理系
- [Clang](/implementation.md#clang): ??
- [GCC](/implementation.md#gcc): ??
- [Visual C++](/implementation.md#visual_cpp): ??


## 関連項目

| 名前 | 説明 |
|-----------------------------------|--------------------------------|
| [`flat_set::end`](end.md)         | 末尾の次を指すイテレータを取得する |
| [`flat_set::cbegin`](cbegin.md)   | 先頭を指すconstイテレータを取得する |
| [`flat_set::cend`](cend.md)       | 末尾の次を指すconstイテレータを取得する |
| [`flat_set::rbegin`](rbegin.md)   | 末尾を指す逆イテレータを取得する |
| [`flat_set::rend`](rend.md)       | 先頭の前を指す逆イテレータを取得する |
| [`flat_set::crbegin`](crbegin.md) | 末尾を指す逆constイテレータを取得する |
| [`flat_set::crend`](crend.md)     | 先頭の前を指す逆constイテレータを取得する |
