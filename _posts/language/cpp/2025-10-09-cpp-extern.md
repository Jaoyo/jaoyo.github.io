---
title: C++ extern
date: 2025-10-09 00:00:00 +0800
categories: [编程语言, Cpp]
tags: [C++语言]
description: 
---

## 作用

`extern`用来声明一个在别的地方定义的变量或函数

它不会创建新的对象或函数实体，只是告诉编译器它的定义存在于别的源文件中

## 使用

```c++
int count = 42;  // 定义全局变量

```
{:file="file1.cpp"}

```c++
#include <iostream>
extern int count;  // 声明这个变量在别的文件中定义
int main() {
    std::cout << count << std::endl;  // ✅ 可以使用
}

```
{:file="file2.cpp"}

## extern与函数

函数在 C++ 中**默认就具有外部链接**（external linkage）

所以即使不加 `extern`，函数本身也可以被其他文件访问：

```c++
// file1.cpp
void show() {
    std::cout << "Hello\n";
}

// file2.cpp
extern void show();  // 可以写 extern，也可以不写
int main() { show(); }
```

## extern与const

| 情况                  | 默认链接类型                 | 是否能被其他文件访问          |
| ------------------- | ---------------------- | ------------------- |
| **普通的非 const 全局变量** | external linkage（外部链接） | ✅ 可以（默认就能被外部访问）     |
| **const 全局变量**      | internal linkage（内部链接） | ❌ 不能（默认只在当前编译单元内可见） |
        |

```c++
// file1.cpp
extern const int max_value = 100;  // 外部可见

// file2.cpp
extern const int max_value;        // 引用
```

* 非`const`的全局变量默认具有**外部链接**，可被外部访问
* `const`的全局变量默认具有**内部链接**，不可被外部访问
* 如果想让`const`全局变量被多个文件共享，必须显式添加`extern`

> 对于局部变量则不同
{: .prompt-warning}

在局部变量中，作用域都只在声明的函数内，他们没有链接性

如果对局部变量添加`extern`修饰

```c++
void func() {
	extern int a;	// 声明在别的文件中定义的全局变量
	cout << a;
}
```

> `extern`的作用是在函数内部声明一个在别的文件中定义的全局变量
{: .prompt-info}

而不是把局部变量声明为`extern`

## extern "C"

`extern "C"` 是 C++ 中用于指定使用 C 语言链接方式的关键字，主要用于 C++ 和 C 代码的混合编程。

> 在C++中，函数支持重载，所以需要在符号中编码参数类型信息，而C++不支持重载，符号名就是函数名本身，因此C++无法直接编译链接C编译的代码

`extern C`的作用是**让 C++ 编译器以 C 语言的方式来编译、命名和链接指定的函数或变量**

- `C 调用 C++ 函数时`：
    `extern "C"` 让 C++ 编译器 把函数导出成 C 风格符号，让 C 找得到。一般在声明定义函数时添加

- `C++ 调用 C 函数时`：
    `extern "C"` 告诉 C++ 编译器 按 C 的方式 去查找和链接。一般在引用C头文件时添加

* C++的名称修饰

```c++
// C++ 代码
void func(int x);
void func(double x);  // 函数重载

// 编译后的符号名（示例）：
// _Z4funci
// _Z4funcd
```

* C的名称修饰

```c
// C 代码
void func(int x);

// 编译后的符号名：
// _func 或 func
```

`extern "C"`的应用例子：

```c++
void normalCppFunction(int x) {
}

void overloadedFunction(int x) {}
void overloadedFunction(double x) {}

extern "C" void cStyleFunction(int x) {}

extern "C" {
        void anotherCFunction(int x) {}
        int yetAnotherCFunction(int x) {return 0;}
}
```
{: file="test.cpp"}

查看编译结果

```bash
gcc -c test.cpp -o test.o
nm test.o

# 结果：
000000000000003a T anotherCFunction
000000000000002c T cStyleFunction
0000000000000048 T yetAnotherCFunction
0000000000000000 T _Z17normalCppFunctioni
000000000000001c T _Z18overloadedFunctiond
000000000000000e T _Z18overloadedFunctioni
```

### 基本语法

* 单个函数声明

```c++
extern "C" void c_function();
```

* 多个函数声明

```c++
extern "C" {
    void func1();
    void func2(int x);
    int func3(double y);
}
```

* 条件编译（常见用法）

```c++
#ifdef __cplusplus
extern "C" {
#endif

void c_function();
int another_function(int x);

#ifdef __cplusplus
}
#endif
```

### 生效范围

* 非成员函数
* 变量（全局对象）
* 可以用于函数指针
* 不能用于成员函数：类的非静态成员函数不能有 C 链接
* 不能重载
* 模板不能有C链接
