---
title: C++ sizeof
date: 2025-10-03 12:00:00 +0800
categories: [编程语言, C++]
tags: [C++语言]
description: 
---

## 空类的大小为1个字节

C++标准规定，任何两个不同的对象都必须在内存中拥有不同的地址。

如果一个空类的大小为0，那么当你创建一个空类的数组时，所有元素的地址都会相同，这就无法区分它们了。为了保证对象实例的唯一性，编译器会为空类分配一个字节的最小内存空间。

```c++
#include <iostream>
using namespace std;
class A{};

int main() {
	cout << sizeof(A) << endl;	// 1
	return 0;
}

## 一个类中，虚函数本身、成员函数（包括静态与非静态）和静态数据成员都是不占用类对象的存储空间

* 非静态成员函数、虚函数 属于“行为”，存放在代码区，不占对象空间。
* 静态数据成员属于类而非对象，存放在静态存储区，不占对象空间。
* 非静态成员变量才会真正占对象内存。

```c++
// 静态数据成员被编译器放在程序的一个global data members中，它是类的一个数据成员，但不影响类的大小。不管这个类产生了多少个实例，还是派生了多少新的类，静态数据成员只有一个实例。静态数据成员，一旦被声明，就已经存在。 
#include<iostream>
using namespace std;
class A
{
    public:
        char b;
        virtual void fun() {};
        static int c;
        static int d;
        static int f;
};

class B
{
    public:
        virtual void fun() {};
        static int c;
        static int d;
        static int f;
};

int main()
{
    cout<<sizeof(A)<<endl; 	// 输出16
    cout<<sizeof(B)<<endl;	// 输出8
    return 0;
}
```

## 对于包含虚函数的类，不管有多少个虚函数，只有一个虚指针`vptr`的大小

* 在**单继承**和**无继承**的情况下，一个类无论有多少个虚函数，编译器都会创建一个虚函数表`vtable`来存放虚函数的地址，因此只需要一个虚指针`vptr`指向`vtable`即可
* 在**多重继承**的情况下，如果多个基类都含有虚函数，那么派生类对象为了维护每个基类子对象的独立性，可能会有多个`vptr`

```c++
class Base1 { virtual void func1(); };
class Base2 { virtual void func2(); };
class Derived : public Base1, public Base2 {};
// 在这种情况下，一个 Derived 对象内部通常会有两个 vptr，
// 一个指向 Base1 相关的 vtable，一个指向 Base2 相关的 vtable。
// 所以 sizeof(Derived) 会比 sizeof(Base1) + sizeof(Base2) 更大。
```


```c++
#include<iostream>
using namespace std;
class A{
    virtual void fun();
    virtual void fun1();
    virtual void fun2();
    virtual void fun3();
};
int main()
{
    cout<<sizeof(A)<<endl; // 8
    return 0;
}
```

## 普通继承、派生类继承了所有基类的函数与成员，要按照字节对齐来计算大小

* 普通继承时，派生类会包含基类的非静态成员，布局上要考虑**字节对齐规则**
* 成员函数依旧不占用对象空间

`sizeof(Derived)` 通常大于等于 `sizeof(Base)` + `sizeof(派生类新增成员)`。

```c++
#include<iostream>

using namespace std;

class A
{
    public:
        char a;
        int b;
};

class B:A
{
    public:
        short a;
        long b;
};

class C
{
	A a;
	char c;
};

class D
{
    public:
        char a;
    private:
        int b;
};

class E:D
{
    public:
        short a;
    // private:
        long b;
};


// 此时B按照顺序声明 char a; int b; short a; long b;
// 根据字节对其 4+4=8， 8+8+8=24

int main() {
	cout << sizeof(A) << endl;	// 8
	cout << sizeof(B) << endl;	// 24
	cout << sizeof(C) << endl;	// 12
	cout << sizeof(D) << endl;  // 8
	cout << sizeof(E) << endl;  // 24
	return 0;
}
```

## 虚函数继承，不管是单继承还是多继承，都是继承了基类的`vptr`

派生类会构建自己的虚函数表（`vtable`），并且其对象拥有自己的虚函数指针（`vptr`）。

* 派生类的`vtable`是基于基类`vtable`构建的。如果派生类重写了基类的某个虚函数，那么派生类`vtable`中对应的条目就会更新为派生类重写后的函数地址。

* 派生类的对象中的`vptr`会指向这个属于派生类自己的`vtable`。

* 所以，派生类并不是直接“继承”或“复制”了基类的`vptr`，而是拥有了一个指向自己`vtable`的`vptr`，这个`vtable`在结构上与基类的vtable有关联。

```c++
#include<iostream>

using namespace std;

class A
{
	virtual void fun(){}
};

class C:public A
{

};

int main() {
	cout << sizeof(C) << endl;	// 8
	return 0;
}
```

## 虚函数、虚继承

* 虚函数 (Virtual Function)：目的是实现运行时多态。其实现机制是虚函数指针 (`vptr`) 和虚函数表 (`vtable`)。

* 虚继承 (Virtual Inheritance)：目的是在多重继承中解决“菱形继承”问题，确保共同的基类在派生类中只存在一个实例。其实现机制通常是虚基类指针 (`vbptr`) 或虚基类表。这个指针指向一个偏移量表，用于在运行时查找虚基类子对象的位置。

一个类可以有虚继承而没有虚函数，也可以有虚函数而没有虚继承。如果一个类同时使用了虚继承并且基类中有虚函数，那么这个类的对象可能会同时包含 `vptr` 和 `vbptr`，会变得更加复杂。
