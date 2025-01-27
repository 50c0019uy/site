# keys
* flat_map[meta header]
* std[meta namespace]
* flat_map[meta class]
* function[meta id-type]
* cpp23[meta cpp]

```cpp
const key_container_type& keys() const noexcept; // C++23
```

## 概要
キーのコンテナを取得する。


## 戻り値
`flat_map` クラス内部で保持しているキーのコンテナ。


## 計算量
定数時間


## 例
```cpp example
#include <flat_map>
#include <iostream>
#include <type_traits>
#include <vector>

int main()
{
  std::flat_map<int, char> fm;
  fm[3] = 'C';
  fm[1] = 'A';
  fm[2] = 'B';

  static_assert(std::is_same_v<decltype(fm.keys()), const std::vector<int>&>);
    
  for (auto i : fm.keys()) {
      std::cout << i << std::endl;
  }
}
```
* keys()[color ff0000]

### 出力
```
1
2
3
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
|-----------------------------------|-------------------------------------------|
| [`flat_map::values`](values.md)   | 値のコンテナを取得する |
