---
title: C++ 模板
date: 2025-09-14 17:00:00 +0800
categories: [编程语言, Cpp]
tags: [C++语言]
description: 
---

模板是**泛型编程**的基础，**泛型编程**是以一种独立于任何特定类型的方式编写代码。

使用模板可以下厨**类型无关**的代码，编译器会在使用时生成具体类型版本。

简单来说，模板就是在实例化函数或类的时候，把类型当作参数传进去或者编译一起自动推断类型，然后分别生成不同类型的函数或者类。

## 函数模板

模板函数定义的一般形式如下：

```c++
template <typename type> ret-type func-name(parameter list)
{
   // 函数的主体
}  
```

`type`是函数所使用的数据类型的占位符名称，这个名称可以在函数定义中使用

模板示例：

```c++
#include <iostream>
using namespace std;

// 函数模板
template <typename T>
T myMax(T a, T b) {
    return (a > b) ? a : b;
}

int main() {
    cout << myMax(3, 7) << endl;        // int 版本
    cout << myMax(3.5, 2.1) << endl;    // double 版本
    cout << myMax('a', 'z') << endl;    // char 版本
}
```

`T`是模板参数，编译器会根据调用时的类型生成对应的函数

## 类模板

泛型类声明的一般形式如下：

```c++
template <class type> class class-name {
.
.
.
}
```

`type`是占位符类型名称，可以在类被实例化的时候进行指定

通用数组容器的例子：

```c++
#include <iostream>
using namespace std;

template <typename T>
class MyArray {
private:
    T* arr;
    int size;
public:
    MyArray(int s) : size(s) {
        arr = new T[s];
    }
    ~MyArray() { delete[] arr; }

    void set(int index, T value) {
        arr[index] = value;
    }
    T get(int index) {
        return arr[index];
    }
};

int main() {
    MyArray<int> a(5);   // int 数组
    a.set(0, 42);
    cout << a.get(0) << endl;

    MyArray<double> b(3);  // double 数组
    b.set(1, 3.14);
    cout << b.get(1) << endl;
}
```

## 多参数模板

```c++
template <typename T1, typename T2>
auto add(T1 a, T2 b) {
    return a + b;
}

int main() {
    cout << add(3, 4.5) << endl;  // int + double
}
```

C++14起可以使用`auto`作为返回类型，编译器会自动推导

## 模板特化(Specialization)

有时需要针对特定类型写不同实现：

```c++
template <typename T>
T myAbs(T x) {
    return (x < 0) ? -x : x;
}

// 针对 bool 的特化
template <>
bool myAbs<bool>(bool x) {
    return x;  // bool 不需要 abs
}

int main() {
    cout << myAbs(-10) << endl;    // 10
    cout << myAbs(true) << endl;   // true
}
```

## 非类型模板参数

模板也能接受值参数

```c++
template <typename T, int N>
class FixedArray {
private:
    T arr[N];
public:
    void fill(T value) {
        for (int i = 0; i < N; i++) arr[i] = value;
    }
    void print() {
        for (int i = 0; i < N; i++) cout << arr[i] << " ";
        cout << endl;
    }
};

int main() {
    FixedArray<int, 5> fa;
    fa.fill(7);
    fa.print();   // 7 7 7 7 7
}
```

