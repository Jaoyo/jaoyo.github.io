---
title: C 可变参数
date: 2025-08-26 16:00:00 +0800
categories: [编程语言, C]
tags: [C语言]
description: 
---

**可变参数**可以使函数接受**不定量的参数**

## 基本语法

C语言的标准库`<stdarg.h>`提供了一组宏来操作可变参数

```c
#include <stdarg.h>

void func(const char *fmt, ...) {
    va_list args;         // 定义一个变量，保存参数列表
    va_start(args, fmt);  // 初始化，从 fmt 后的第一个参数开始
    int x = va_arg(args, int); // 依次取参数（指定类型）
    va_end(args);         // 清理
}
```

其中：

* `...` 表示可变参数。
* `va_list` 是一个类型，表示可变参数列表。
* `va_start(ap, last)`：初始化，last 是函数形参表里最后一个固定参数的名字。
* `va_arg(ap, type)`：取下一个参数，类型必须写对。
* `va_end(ap)`：结束时清理。

## 程序示例

```c
#include <stdio.h>
#include <stdarg.h>

int sum(int count, ...) {
    int total = 0;
    va_list args;
    va_start(args, count);

    for (int i = 0; i < count; i++) {
        total += va_arg(args, int); // 每次取一个 int
    }

    va_end(args);
    return total;
}

int main() {
    printf("%d\n", sum(3, 1, 2, 3));       // 输出 6
    printf("%d\n", sum(5, 10, 20, 30, 40, 50)); // 输出 150
    return 0;
}
```

