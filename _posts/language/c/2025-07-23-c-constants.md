---
title: C 常量
date: 2025-07-23 18:00:00 +0800
categories: [编程语言, C]
tags: [C语言]
description: 在 C 语言中，常量（constant）是值在程序运行过程中不可改变的数据。
---

常量的意思就是这个数据在声明之后，就无法再改变。

### 字面量常量（Literal Constants）

直接出现在代码中的数值、字符或字符串等：

```c
42         // 整型常量
3.14       // 浮点型常量（double）
'a'        // 字符常量
"hello"    // 字符串常量
```

### 符号常量（`#define` 宏定义）

通过预处理指令定义：

```c
#define PI 3.14159
#define MAX_LEN 100
```

特点：

* 编译前由预处理器进行文本替换
* 没有类型检查
* 可用于条件编译

### `const` 常量（关键字修饰变量）

```c
const int max = 100;
const float pi = 3.14f;
```

特点：

* 有类型，参与类型检查
* 编译时或运行时赋值，之后不可修改
* 可以用于函数参数保护（防止被修改）

例如:

```c
#include <stdio.h>

int main(){
    const int a = 10;
    a = 20;
    printf("%d\n", a);
    return 0;
}
```

此时编译就会报错

```shell
.\data_format.c: In function 'main':
.\data_format.c:5:7: error: assignment of read-only variable 'a'
    5 |     a = 20;
      |       ^
```

### 枚举常量（`enum`）

```c
enum Color { RED, GREEN, BLUE };
```

默认 RED=0, GREEN=1, BLUE=2，也可以指定值。

### 字符串常量

```c
const char* msg = "Hello";
```

注意 `"Hello"` 是只读字符串常量，不能修改其内容.
