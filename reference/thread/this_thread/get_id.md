# get_id
* thread[meta header]
* std::this_thread[meta namespace]
* function[meta id-type]
* cpp11[meta cpp]

```cpp
namespace std {
namespace this_thread {
  thread::id get_id() noexcept;
}}
```
* thread::id[link /reference/thread/thread/id.md]


## 概要
現スレッドのスレッド識別子を取得する。


## 戻り値
現在のスレッド、すなわちこの関数を呼び出したスレッドのスレッド識別子を返す。

この関数の戻り値は、デフォルト構築された[`thread::id`](/reference/thread/thread/id.md)オブジェクトとは必ず異なる。


## 例外
送出しない。


## 例
```cpp example
#include <iostream>
#include <thread>

int main()
{
  std::cout << "thread_id=" << std::this_thread::get_id() << std::endl;
  return 0;
}
```
* std::this_thread::get_id()[color ff0000]

### 出力例
```
thread_id=538af0
```

## バージョン
### 言語
- C++11

### 処理系
- [Clang](/implementation.md#clang):
- [GCC](/implementation.md#gcc): 4.6.3 [mark verified], 4.7.0 [mark verified]
- [ICC](/implementation.md#icc):
- [Visual C++](/implementation.md#visual_cpp): 2012 [mark verified], 2013 [mark verified], 2015 [mark verified]


## 参照
