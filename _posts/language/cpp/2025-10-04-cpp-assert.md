---
title: C++ 断言
date: 2025-10-04 00:00:00 +0800
categories: [编程语言, Cpp]
tags: [C++语言]
description: 
---

`assert`是一个**运行时调试宏**，定义在头文件`<cassert>`中

> 在C语言中则定义在`<assert.h>`

## 基本作用

`assert(expr)`会检查`expr`是否为真

* 如果`expr == true`，那么什么都不会发生，程序继续执行
* 如果`expr == false`，那么程序会打印错误信息，并调用`abort()`来终止程序

## 示例

```c++
#include <cassert>
#include <iostream>
using namespace std;

int divide(int a, int b) {
    assert(b != 0); // 确保分母不为 0
    return a / b;
}

int main() {
    cout << divide(10, 2) << endl; // 正常
    cout << divide(10, 0) << endl; // 触发 assert
}
```

运行时检测到`b == 0`，那么会输出报错

```
Assertion failed: (b != 0), file main.cpp, line 6
```

## NDEBUG

`assert`在Debug模式下有效，在Release模式下通常会禁用

如果在编译时定义了宏`NDEBUG`

```c++
#define NDEBUG
#include <cassert>
```

那么所有的`assert`语句都会被替换成空语句，并且没有运行时的开销


