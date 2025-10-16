---
title: C++ friend友元
date: 2025-10-14 11:00:00 +0800
categories: [编程语言, Cpp]
tags: [C++语言]
description: 
---

## 概述

友元提供了一种非成员函数或者其他类，访问另一个类中的私有或保护成员的方法。

友元分为：

* 友元函数：普通函数访问某个类中的私有或保护成员
* 友元类：类A中的成员函数访问类B中的私有或保护成员
* 友元成员函数：某个类的特定成员可以访问另一个类的私有或保护成员

友元的存在提高了程序的运行效率，但是破坏了类的封装性和数据透明性

## 友元函数

在类声明的任何区域中声明，但是定义在类的外部

```
friend <类型><友元函数名>(<参数表>)
```

> 友元函数只是一个普通函数，而不是该类的成员函数

```c++
#include <iostream>

using namespace std;

class A
{
public:
    A(int _a):a(_a){};
    friend int geta(A &ca);  ///< 友元函数
private:
    int a;
};

int geta(A &ca) 
{
    return ca.a;
}

int main()
{
    A a(3);    
    cout<<geta(a)<<endl;

    return 0;
}
```

## 友元类

友元类的声明在该类的声明中，而实现在该类外

```
friend class <友元类名>;
```

如果类B是类A的友元，那么类B可以直接访问类A的私有成员

```c++
#include <iostream>

using namespace std;

class A
{
public:
    A(int _a):a(_a){};
    friend class B;
private:
    int a;
};

class B
{
public:
    int getb(A ca) {
        return  ca.a; 
    };
};

int main() 
{
    A a(3);
    B b;
    cout<<b.getb(a)<<endl;
    return 0;
}
```

> 友元关系是**单项**的，且**不能被继承**

## 友元成员函数

```c++
#include <iostream>
using namespace std;

class Engine; // 前向声明

class Car {
public:
    void show(const Engine& e);
};

class Engine {
private:
    int power = 250;
    friend void Car::show(const Engine&); // 指定Car的某个成员函数为友元
};

void Car::show(const Engine& e) {
    cout << "Engine power: " << e.power << " HP" << endl;
}
``` 

## 注意事项

* 友元关系不是继承的，继承后的类也无法访问私有成员，或者继承后的类也声明为友元类
* 友元关系不是传递的，不存在友元的友元这种关系
