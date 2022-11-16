# get_id
* thread[meta header]
* std[meta namespace]
* jthread[meta class]
* function[meta id-type]
* cpp20[meta cpp]

```cpp
id get_id() const noexcept;
```
* id[link /reference/thread/thread/id.md]


## 概要
関連付けられているスレッドのスレッド識別子を取得する。


## 戻り値
`thread`オブジェクトがスレッドに関連付けられていない場合はデフォルトコンストラクトされた[`id`](/reference/thread/thread/id.md)オブジェクト、そうでなければ`*this`が関連付けられているスレッドを表す[`std::this_thread::get_id()`](/reference/thread/this_thread/get_id.md)を返す。


## 例外
送出しない。


## 例
```cpp example
#include <thread>
#include <cassert>
#include <iostream>

int main()
{
  std::jthread t1;
  assert( t1.get_id() == std::jthread::id{} );

  std::jthread t2{ []{ /*...*/ } };
  assert( t2.get_id() != std::jthread::id{} );

  std::cout << t1.get_id() << std::endl;
  std::cout << t2.get_id() << std::endl;
}
```
* get_id()[color ff0000]
* std::jthread::id[link /reference/thread/thread/id.md]

### 出力例
```
thread::id of a non-executing thread
139880063837952
```

## バージョン
### 言語
- C++20

### 処理系
- [Clang](/implementation.md#clang):
- [GCC](/implementation.md#gcc): 10.2.0
- [Visual C++](/implementation.md#visual_cpp): ??
