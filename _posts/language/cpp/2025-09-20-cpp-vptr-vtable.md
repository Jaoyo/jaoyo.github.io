---
title: C++ 虚函数
date: 2025-09-20 16:00:00 +0800
categories: [编程语言, Cpp]
tags: [C++语言]
description: 
---

## 定义

虚函数是基类中使用`virtual`关键字声明的成员函数，允许在派生类中**重写（override）**，并在运行时通过**动态绑定（dynamic dispatch）**来决定实际调用哪个版本的函数

```c++
#include <iostream>
using namespace std;

class Animal {
public:
    virtual void speak() {  // 虚函数
        cout << "Animal makes a sound" << endl;
    }
};

class Dog : public Animal {
public:
    void speak() override {  // 重写虚函数
        cout << "Dog barks" << endl;
    }
};

class Cat : public Animal {
public:
    void speak() override {
        cout << "Cat meows" << endl;
    }
};

int main() {
    Animal* a1 = new Dog();
    Animal* a2 = new Cat();

    a1->speak();  // 输出: Dog barks
    a2->speak();  // 输出: Cat meows

    delete a1;
    delete a2;
}
```

## 特点

* 必须在基类中用`virtual`声明
* 通过**基类指针或引用**调用时，才触发多态
* 在对象直接调用时，不会多态

	```c++
	Dog d;
	d.speak();	// 静态绑定，不依赖virtual
	```

* 派生类重写时建议加上`override`，防止拼写错误或函数签名不一致

## 虚函数表和虚函数指针

### 虚函数表(V-Table)

编译器为含有虚函数的类生成一张**虚函数表（vtable）**，存储虚函数的指针

> 该表只是编译器在编译时设置的静态数组，数组中的每个元素是一个函数指针，指向类中对应的虚函数的实现

* 每个对象内部都会有一个隐藏的**虚函数指针（vptr）**，指向该类的`table`
* 调用时，运行时根据`vptr`查表，执行正确的函数

### 虚函数指针(vptr)

`vptr`是指向基类的指针，在创建类实例时自动设置，以便指向该类的虚拟表

与`this`指针不同，`this`指针实际上是编译器用来解析自引用的函数参数，`vptr`是一个真正的指针

> 每个对象中，编译器会在其内存布局的最前面放一个**vptr(虚指针)**，指向该类的`vtable`

因此可以通过对象的首地址，得到`vptr`，再通过`vptr`找到`vtable`，进一步得到实际的虚函数地址

* 虚函数表和虚函数指针的内存布局示意

```
对象内存布局：
+------------------+
|      vptr        |  <- 指向vtable
+------------------+
|   其他成员变量    |
+------------------+

虚函数表(vtable)：
+------------------+
|   fun1的地址     |  <- offset=0
+------------------+
|   fun2的地址     |  <- offset=1  
+------------------+
|   fun3的地址     |  <- offset=2
+------------------+
```


下面是一个虚函数指针和虚函数表的代码：

```c++
/**
 * @file vptr1.cpp
 * @brief C++虚函数vptr和vtable
 * 编译：g++ -g -o vptr vptr1.cpp -std=c++11
 * @version v1
 */

#include <iostream>
#include <stdio.h>
using namespace std;

/**
 * @brief 函数指针
 */
typedef void (*Fun)();  // 函数指针类型Fun

/**
 * @brief 基类
 */
class Base
{
    public:
        Base(){};
        virtual void fun1()
        {
            cout << "Base::fun1()" << endl;
        }
        virtual void fun2()
        {
            cout << "Base::fun2()" << endl;
        }
        virtual void fun3(){}
        ~Base(){};
};

/**
 * @brief 派生类
 */
class Derived: public Base
{
    public:
        Derived(){};
        void fun1()
        {
            cout << "Derived::fun1()" << endl;
        }
        void fun2()
        {
            cout << "DerivedClass::fun2()" << endl;
        }
        ~Derived(){};
};

/**
 * @brief 获取vptr地址与func地址,vptr指向的是一块内存，这块内存存放的是虚函数地址，这块内存就是我们所说的虚表
 *
 * @param obj
 * @param offset
 *
 * @return 
 */
Fun getAddr(void* obj, unsigned int offset)
{
    cout << "=======================" << endl;
    
    // 取出对象首地址的内容 (前8字节)，即 vptr
    void* vptr_addr = (void *)*(unsigned long *)obj;
    printf("vptr_addr:%p\n", vptr_addr);

    // 通过vptr访问vtable中的函数地址，取第offset个元素
    void* func_addr = (void *)*((unsigned long *)vptr_addr + offset);
    printf("func_addr:%p\n", func_addr);

    // 将该虚函数返回
    return (Fun)func_addr;
}

int main(void)
{
    Base ptr;
    Derived d;
    Base *pt = new Derived(); // 基类指针指向派生类实例
    Base &pp = ptr; // 基类引用指向基类实例
    Base &p = d; // 基类引用指向派生类实例
    cout<<"基类对象直接调用"<<endl;
    ptr.fun1();
    cout<<"基类引用指向基类实例"<<endl;
    pp.fun1(); 
    cout<<"基类指针指向派生类实例并调用虚函数"<<endl;
    pt->fun1();
    cout<<"基类引用指向派生类实例并调用虚函数"<<endl;
    p.fun1();

    // 手动查找vptr 和 vtable
    Fun f1 = getAddr(pt, 0);
    (*f1)();
    Fun f2 = getAddr(pt, 1);
    (*f2)();
    delete pt;
    return 0;
}
```

在程序中，变量有以下关系：

* `obj` → 对象地址
* `*(unsigned long *)obj` → 取出`vptr`的值（虚函数表的地址）
* `vptr_addr` → 虚函数表的起始地址
* `(unsigned long *)vptr_addr` → 将`vtable`地址转换为`unsigned long`指针
* `+ offset` → 指针运算，取第`offset`个虚函数的地址
* `*()` → 取出该位置存储的函数地址

`getAddr`原理说明：

1. 在`getAddr`中，参数`*obj`是类对象的指针
2. 首先通过`*(unsigned long *)obj`获取对象在内存分布中的前8个字符，即`vptr`
3. `(void *)*(unsigned long *)obj`为指向了`vtable`**虚函数表**的地址
4. `(unsigned long *)vptr_addr`将`vptr`转换为一个指针数组
5. `(unsigned long *)vptr_addr + offset`则是取数组的第offset个值，`0`是第一个函数，`1`是第二个函数
6. 返回虚函数的地址
7. 使用`unsigned long`是因为在Linux系统下，指针的长度和`unsigned long`一样都是8个字节
* 在Windows系统下，`unsigned long`需要替换为`uintptr_t`（需要引入头文件`cstdint`）


运行结果：

```
基类对象直接调用
Base::fun1()
基类引用指向基类实例
Base::fun1()
基类指针指向派生类实例并调用虚函数
Derived::fun1()
基类引用指向派生类实例并调用虚函数
Derived::fun1()
=======================
vptr_addr:0x64a1e0a29d08
func_addr:0x64a1e0a275b2
Derived::fun1()
=======================
vptr_addr:0x64a1e0a29d08
func_addr:0x64a1e0a275f0
DerivedClass::fun2()
```


## 纯虚函数&抽象类

如果基类中的函数没有实现，可以用**纯虚函数**声明

```c++
class Shape {
public:
	virtual void drwo() = 0;	// 纯虚函数
};
```

包含纯虚函数的类称为**抽象类**，不能直接实例化，必须由派生类实现
