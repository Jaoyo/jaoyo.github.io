---
title: C++ 预处理器
date: 2025-09-14 21:00:00 +0800
categories: [编程语言, C++]
tags: [C++语言]
description: 
---

预处理器是一些指令，指示编译器在实际编译之前所需完成的预处理。

所有的预处理器指令都是以井号（`#`）开头，只有空格字符可以出现在预处理指令之前。预处理指令不是 C++ 语句，所以它们不会以分号（`;`）结尾。

## define预处理

`#define` 预处理指令用于创建符号常量。该符号常量通常称为宏，指令的一般形式是：

```c++
#define macro-name replacement-text 
```

当这一行代码出现在一个文件中时，在该文件中后续出现的所有宏都将会在程序编译之前被替换为 replacement-text。

## 函数宏

您可以使用 #define 来定义一个带有参数的宏，如下所示：

```c++
#include <iostream>
using namespace std;

#define MIN(a,b) (((a)<(b)) ? a : b)

int main ()
{
   int i, j;
   i = 100;
   j = 30;
   cout <<"The minimum is " << MIN(i, j) << endl;

    return 0;
}
```

## 条件编译

有几个指令可以用来有选择地对部分程序源代码进行编译。这个过程被称为条件编译。

语法：

```c++
#ifndef NULL
   #define NULL 0
#endif
```

## #和##运算符

`#`运算符会把参数转化为字符串，`##`运算符用于连接两个参数

```c++
#include <iostream>
using namespace std;

#define MKSTR( x ) #x
#define concat(a, b) a##b

int main ()
{
    int num1 = 100;
    
    cout << MKSTR(HELLO C++) << endl;
    
    cout << concat(num, 1) << endl;

    return 0;
} 
```

运行结果

```
HELLO C++
100
```

## 预定义宏

宏 | 描述
---- | ----
`__DATE__` | 当前日期，一个以 "MMM DD YYYY" 格式表示的字符常量。
`__TIME__` | 当前时间，一个以 "HH:MM:SS" 格式表示的字符常量。
`__FILE__` | 这会包含当前文件名，一个字符串常量。
`__LINE__` | 这会包含当前行号，一个十进制常量。
