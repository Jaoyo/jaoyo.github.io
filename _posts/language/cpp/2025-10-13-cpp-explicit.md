---
title: C++ explicit
date: 2025-10-13 23:00:00 +0800
categories: [编程语言, Cpp]
tags: [C++语言]
description: 
---

`explicit`关键字用于**防止编译器进行隐式类型转换**，主要用于构造函数和类型转换运算符，目的是避免意外的类型自动转换，提高代码的安全性和可读性

## 基本作用

```c++
#include <iostream>
using namespace std;

class A {
public:
    A(int x) { cout << "A(int) called\n"; }
};

class B {
public:
	explicit B(int x) { cout << "explicit A(int) called\n"; }
};

void funcA(A a) {}
void funcB(B b) {}

int main() {
    funcA(10);  	// ✅ 没有explicit，编译通过：10 被隐式转换为 A(10)

    funcB(10);		// ❌ 被explicit修饰，编译错误：不能从 int 隐式转换为 A
    funcB(B(10));	// ✅ 显式构造
}
```

* 没有`explicit`时，`funcA`自动进行隐式转换，将`10`自动转换成`A(10)`
* 被`explicit`修饰后，构造函数只能被显示调用，而不能在需要类型转换的上下文中隐式使用

## 使用场景

### 构造函数防止隐式转换

```c++
class String {
public:
    explicit String(int size);  // 防止 String s = 5;
    String(const char* str);
};

String s1 = "abc";  // ✅
String s2 = 5;      // ❌ 如果没有 explicit，会尝试构造长度为5的字符串
```

### 类型转换运算符防止隐式转换

```c++
#include <iostream>
using namespace std;

class A {
public:
    explicit operator int() const { return 42; }
};

int main() {
    A a;
    int x = a;        // ❌ 错误：explicit 禁止隐式转换
    int y = int(a);   // ✅ 显式转换
}
```

### `explicit`与多参数构造函数

```c++
struct B {
    explicit B(int a, int b) {}
};

B b = {1, 2};     // ❌ 错误
B b2{1, 2};       // ✅ 显式调用（花括号初始化）
```

