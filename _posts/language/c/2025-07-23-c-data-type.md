---
title: C 数据类型
date: 2025-07-23 17:00:00 +0800
categories: [编程语言, C]
tags: [C语言]
description: 
---

### 基本数据类型

> 数据类型指定变量将存储的信息的大小和类型。

在64位Windows系统下，基本的数据类型大小如下所示

数据类型 | 大小size | 范围Range | 描述
--- | --- | --- | ---
`char` | 1字节 | `-128`~`127` | 单个字符/字母/数字/ASCII
signed char | 1字节 | `-128`~`127` |
unsigned char | 1字节 | `0`~`255` |
`int` | 4字节 | `-2,147,483,648`~`2,147,483,647` |
signed int | 4字节 | `-2,147,483,648`~`2,147,483,647` |
unsigned int | 4字节 | `0`~`4,294,967,295` |
short int | 2字节 | `-32,768`~`32,767` |
signed short int | 2字节 | `-32,768`~`32,767` |
unsigned short int | 2字节 | `0`~`65,535` |
`long int` | 4字节 | `-2,147,483,648`~`2,147,483,647` | **在64位Linux下大小为8字节**
signed long int | 4字节 | `-2,147,483,648`~`2,147,483,647` | **在64位Linux下大小为8字节**
unsigned long int | 4字节 | `0`~`4,294,967,295` | **在64位Linux下大小为8字节**
`float` | 4字节 | `±3.4 × 10^38` |
`double` | 8字节 | `±1.7 × 10^308` |
long double | 16字节 | |
long long int | 8字节 | `-9,223,372,036,854,775,808`~`9,223,372,036,854,775,807` |
signed long long int | 8字节 | `-9,223,372,036,854,775,808`~`9,223,372,036,854,775,807` |
unsigned long long int | 8字节 | `0`~`18,446,744,073,709,551,615` |

参考代码如下：

```c
#include <stdio.h>

int main(){
    printf("size of char: %d\n", sizeof(char));
    printf("size of signed char: %d\n", sizeof(signed char));
    printf("size of unsigned char: %d\n", sizeof(unsigned char));
    printf("size of int: %d\n", sizeof(int));
    printf("size of signed int: %d\n", sizeof(signed int));
    printf("size of unsigned int: %d\n", sizeof(unsigned int));
    printf("size of short: %d\n", sizeof(short));
    printf("size of signed short: %d\n", sizeof(signed short));
    printf("size of unsigned short: %d\n", sizeof(unsigned short));
    printf("size of long: %d\n", sizeof(long));
    printf("size of signed long: %d\n", sizeof(signed long));
    printf("size of unsigned long: %d\n", sizeof(unsigned long));
    printf("size of float: %d\n", sizeof(float));
    printf("size of double: %d\n", sizeof(double));
    printf("size of long double: %d\n", sizeof(long double));
    printf("size of long long: %d\n", sizeof(long long));
    printf("size of signed long long: %d\n", sizeof(signed long long));
    printf("size of unsigned long long: %d\n", sizeof(unsigned long long));
    return 0;
}
```

在64位Windows下的输出结果：

```
size of char: 1
size of signed char: 1
size of unsigned char: 1
size of int: 4
size of signed int: 4
size of unsigned int: 4
size of short: 2
size of signed short: 2
size of unsigned short: 2
size of long: 4
size of signed long: 4
size of unsigned long: 4
size of float: 4
size of double: 8
size of long double: 16
size of long long: 8
size of signed long long: 8
size of unsigned long long: 8
```

在64位Linux下的输出结果：

```
size of char: 1
size of signed char: 1
size of unsigned char: 1
size of int: 4
size of signed int: 4
size of unsigned int: 4
size of short: 2
size of signed short: 2
size of unsigned short: 2
size of long: 8
size of signed long: 8
size of unsigned long: 8
size of float: 4
size of double: 8
size of long double: 16
size of long long: 8
size of signed long long: 8
size of unsigned long long: 8
```

### 基本格式说明符

每种数据类型都有不同的格式说明符。这里是其中的一些：

格式说明符 | 数据类型
--- | ---
`%d` 或 `%i` | int 整数
`%f` | float 单精度的十进制类型
`%lf` | double 高精度浮点数据或数字
`%c` | char 字符
`%s` | 用于 strings

| | short | int | long
--- | --- | --- | ---
8 进制 | `%ho` | `%o` | `%lo`
10 进制 | `%hd` | `%d` | `%ld`
16 进制 | `%hx` 或者 `%hX` |` %x` 或者 `%X` | `%lx` 或者 `%lX`

#### int 整数

```c
// 创建 整数 变量
int myNum = 5;
// 打印输出变量
printf("%d\n", myNum);
```

#### float 单精度的十进制类型

```c
float myFloatNum = 5.99; // 浮点数
// 打印输出变量
printf("%f\n", myFloatNum);
```

#### double 双精度的十进制类型

```c
double myDouble = 3.2325467;  
// 打印输出变量
printf("%lf\n", myDouble);
```

#### char 字符

```c
char myLetter = 'D'; // 字符串
// 打印输出变量
printf("%c\n", myLetter);
```
