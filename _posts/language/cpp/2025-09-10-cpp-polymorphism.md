---
title: C++ 多态
date: 2025-09-10 16:00:00 +0800
categories: [编程语言, Cpp]
tags: [C++语言]
description: 
---

多态（Polymorphism）就是**同一操作作用于不同对象，可以产生不同的行为**。

主要分为：

1. 编译时多态（静态多态）

	* 函数重载

	* 运算符重载

	* 模板（泛型编程）
		在编译阶段就确定调用哪个函数。

2. 运行时多态（动态多态）

	* 基类指针或引用调用虚函数（`virtual`）
		在运行时根据对象的实际类型决定调用哪个函数。

## 动态多态

动态多态是通过 **虚函数（virtual function）** 实现的。

```c++
#include <iostream>
using namespace std;

class Base {
public:
    virtual void show() {   // 虚函数
        cout << "Base show()" << endl;
    }
};

class Derived : public Base {
public:
    void show() override {  // 重写虚函数
        cout << "Derived show()" << endl;
    }
};

int main() {
    Base* b = new Derived();  // 基类指针指向子类对象
    b->show();                // 调用 Derived::show()
    delete b;
}
```

程序输出

```
Derived show()
```

> 如果去掉 `virtual`，输出就是 `Base show()` —— **静态绑定**。

## 虚函数表（vtable）

* 每个有虚函数的类，编译器会生成一个 **虚函数表**。

* 对象内部会有一个隐藏的 **虚表指针**（`vptr`），指向这个表。

* 调用虚函数时，会通过 `vptr` 查找实际要调用的函数地址。
	这就是为什么动态多态比普通函数调用略慢。

## 纯虚函数

如果基类函数没有具体实现，可以定义为**纯虚函数**：

```c++
class Shape {
public:
    virtual void draw() = 0;   // 纯虚函数
};

class Circle : public Shape {
public:
    void draw() override {
        cout << "Draw Circle" << endl;
    }
};
```

特点：

* 含有纯虚函数的类叫 **抽象类**。

* 抽象类不能实例化，只能作为**接口**。

* 子类必须实现所有纯虚函数，否则它也是抽象类。