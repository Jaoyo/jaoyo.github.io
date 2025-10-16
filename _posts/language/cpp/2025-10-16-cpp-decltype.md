---
title: C++ decltype自动类型推导
date: 2025-10-16 21:00:00 +0800
categories: [编程语言, Cpp]
tags: [C++语言]
description: 
---

`decltype`是C++引入的一个关键字，用来**在编译期推导表达式的类型**，而不实际求值。

常用于模板编程、泛型代码中，用来获取变量或表达式的精确类型

## 基本语法

```
decltype(表达式)
```

作用：返回表达式的类型，仅查询，并不会对表达式进行求值

## 基本示例

```c++
int x = 10;
decltype(x) y = 20;	// y的类型是int
```

等价于

```c++
int y = 20;
```

* 与`using/typedef`结合，用于定义类型

```c++
using size_t = decltype(sizeof(0));	// sizeof(0)的返回值为size_t类型

using ptrdiff_t = decltype((int*)0 - (int*)0);	// 指针相减的结果是ptrdiff_t，有符号整数类型

using nullptr_t = decltype(nullptr);	// nullptr的类型是std::nullptr_t

vector<int> vec;
typedef decltype(vec.begin()) vectype;	
// vec.begin()是一个迭代器类型
// 等效于 typedef std::vector<int>::iterator vectype;
for (vectype i = vec.begin(); i != vec.end(); i++) {
	// ...
}

## decltype的核心规则

| 表达式                      | 推导结果     | 说明                     |
| :----------------------- | :------- | :--------------------- |
| `decltype(x)`            | `T`      | 如果 `x` 是一个变量名，则结果是它的类型 |
| `decltype((x))`          | `T&`     | 注意多了一层括号，结果是左值引用类型     |
| `decltype(std::move(x))` | `T&&`    | 如果表达式是右值，则返回右值引用类型     |
| `decltype(10)`           | `int`    | 字面量的类型                 |
| `decltype(x + y)`        | 表达式的结果类型 | 取决于运算符重载和操作数类型         |

代码：

```c++
int a = 0;
int& b = a;

decltype(a) c = a;	// c的类型是int
decltype(b) d = a;	// d的类型是int&

decltype((a)) e = a;	// e的类型是int&
```

* `decltype(x)`：变量的类型本身
* `decltype((x))`：始终是左值引用类型

## 在模板中的应用

在泛型编程中，`decltype`通常配合`auto`或`decltype(auto)`来使用，用来声明返回值的类型

```c++
template <typename T, typename U>
auto add(T a, U b) -> decltype(a + b) {
    return a + b;
}
```

C++14后可以简写：

```c++
template <typename T, typename U>
auto add(T a, U b) {
    return a + b;  // 编译器自动推导返回类型
}
```

## decltpye和auto的区别

| 特性   | `auto`         | `decltype`   |
| :--- | :------------- | :----------- |
| 依据   | 初始化表达式的**值类别** | 表达式的**类型本身** |
| 是否求值 | ✅ 求值           | ❌ 不求值        |
| 常用于  | 推导变量类型         | 获取表达式的类型     |
| 括号影响 | 不影响            | 有显著影响        |

```c++
int a = 0;
int& b = a;

auto x = (b);       // x 是 int
decltype((b)) y = a; // y 是 int&
```

* 高级用法：decltype(auto)

```c++
int x = 42;
int& f() { return x; }

auto a = f();        // a 是 int
decltype(auto) b = f(); // b 是 int&
```

## 总结

| 用法               | 含义                  |
| :--------------- | :------------------ |
| `decltype(expr)` | 获取 `expr` 的类型（不求值）  |
| `decltype(auto)` | 自动推导类型并保持引用/值性质     |
| `(x)` vs `x`     | 多一层括号时会返回引用类型       |
| 应用场景             | 模板推导、泛型函数返回值声明、类型萃取 |
