# erase
* flat_map[meta header]
* std[meta namespace]
* flat_map[meta class]
* function[meta id-type]
* cpp23[meta cpp]

```cpp
iterator erase(iterator position);                         // (1) C++23

iterator erase(const_iterator position);                   // (2) C++23

size_type erase(const key_type& x);                        // (3) C++23

template <class K>
size_type erase(K&& x);                                    // (4) C++23

iterator erase(const_iterator first, const_iterator last); // (5) C++23
```

## 概要
単一要素またはイテレータ範囲`[first, last)`を `map` コンテナから削除する。

これは削除された要素の数だけコンテナの `size` を減らし、それぞれの要素のデストラクタを呼び出す。

- (1) : 指定されたイテレータが指す要素を削除する
- (2) : 指定された読み取り専用イテレータが指す要素を削除する
- (3) : 指定されたキーをもつ要素を削除する
- (4) : `key_type`と比較可能な`K`型のキーを受け取って要素を削除する
- (5) : 指定されたイテレータ範囲の要素をすべて削除する


## パラメータ
- `position` : 削除する単一要素を指すイテレータ。`iterator` はメンバ型であり、双方向イテレータとして定義される。
- `x` : 削除する値のキー。`key_type` はメンバ型であり、コンテナの中で `Key` の別名として定義される。ここで `Key` は 1 番目のテンプレートパラメータであり、コンテナに格納される要素のキーの型である。
- `first, last` : 削除するイテレータ範囲 `[first, last)` を指定するイテレータ。ここでいう範囲は `first` と `last` の間の全ての要素を含み、`first` が指す要素を含むが `last` が指す要素は含まない。


## 戻り値
- (1), (2), (5) : 削除された要素の次を指すイテレータを返す。そのような要素がない場合、[`end()`](end.md)を返す (要素を削除した結果としてコンテナが空になった場合)
- (3), (4) : 削除された要素の数を返す。


## 計算量
- (1), (2) : 定数時間
- (3), (4) : コンテナの [`size()`](/reference/map/map/size.md) について対数時間
- (5) : コンテナの [`size()`](/reference/map/map/size.md) について対数時間、それに加えて `first` と `last` の間の距離に対する線形時間


## 備考
- (1), (2) : この関数に、範囲外のイテレータ (終端イテレータを含む) を指定した場合の動作は未定義
- 削除された要素を指すイテレータ、および、参照のみ無効になる。なお、規格書に明確な記載は無いが、削除された要素を指すポインタも無効になる
- ループ中で `map` の要素を削除するためには、C++03 までは以下のようなコードを書く必要があった。
    ```cpp
    while (it != map_object.end()) {
      if (条件) {
        map_object.erase(it++);
      }
      else {
        ++it;
      }
    }
    ```

    - これは、`erase` で指定したイテレータが、対象となる要素が削除されることによって無効になるため、後置インクリメント `it++` を使用することで要素が削除されるより先に削除対象の次の要素を指すようにするためである。
    - このような書き方は C++11 以降でも依然として有効だが、`erase` が削除された次の要素を指すイテレータを返すようになったため、以下のようなコードを
    ```cpp
    map_object.erase(it++);
    ```

    - 以下のように書くこともできるようになった
    ```cpp
    it = map_object.erase(it);
    ```


## 例
### 基本的な使い方 (C++23)
```cpp example
#include <iostream>
#include <flat_map>

int main()
{
  std::flat_map<int, char> fm = {
    {1, 'A'},
    {2, 'B'},
    {3, 'C'}
  };

  std::cout << fm.size() << std::endl;

  fm.erase(1);
  std::cout << fm.size() << std::endl;

  fm.erase(fm.begin(), fm.end());
  std::cout << fm.size() << std::endl;
}
```
* erase[color ff0000]
* fm.size()[link size.md]
* fm.begin()[link begin.md]
* fm.end()[link end.md]

#### 出力
```
3
2
0
```

### イテレート中に要素を削除する (C++23)
```cpp example
#include <iostream>
#include <flat_map>

int main()
{
  std::flat_map<int, char> fm = {
    {3, 'a'},
    {1, 'b'},
    {4, 'c'}
  };

  // イテレート中に要素削除をするような場合には、
  // 範囲for文は使用できない
  for (auto it = fm.begin(); it != fm.end();) {
    // 条件一致した要素を削除する
    if (it->first == 1) {
      // 削除された要素の次を指すイテレータが返される。
      it = fm.erase(it);
    }
    // 要素削除をしない場合に、イテレータを進める
    else {
      ++it;
    }
  }

  for (const auto& [key, value] : fm) {
    std::cout << key << ':' << value << std::endl;
  }
}
```

#### 出力
```
3:a
4:c
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
|---------------------------------|----------------------|
| [`flat_map::clear`](clear.md)   | 全ての要素を削除する |
| [`flat_map::insert`](insert.md.nolink) | 要素を挿入する |
| [`flat_map::find`](find.md)     | 指定したキーで要素を探す |

