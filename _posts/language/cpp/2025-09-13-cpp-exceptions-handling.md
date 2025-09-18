---
title: C++ 异常处理
date: 2025-09-13 22:30:00 +0800
categories: [编程语言, C++]
tags: [C++语言]
description: 
---

C++主要有以下三种异常处理的关键字

* `throw`: 当问题出现时，程序通过**throw**抛出一个异常。
* `catch`: 使用**catch**来捕获异常
* `try`: **try**块中的代码标识将被激活的特定异常。它后面通常跟着一个或多个 `catch` 块。

## 基本语法

```c++
try {
    // 可能会出错的代码
    if (条件不满足) {
        throw 错误对象;   // 抛出异常
    }
} 
catch (类型 参数) {
    // 处理异常
}
```

## 简单示例

```c++
#include <iostream>
using namespace std;

int main() {
    try {
        int a, b;
        cout << "Enter two numbers: ";
        cin >> a >> b;

        if (b == 0) {
            throw runtime_error("除数不能为 0");
        }

        cout << "Result: " << (a / b) << endl;
    } 
    catch (const runtime_error& e) {
        cout << "捕获异常: " << e.what() << endl;
    }

    return 0;
}
```

## 自定义异常类

C++允许使用`std::exception`来定义自己的异常类

```c++
#include <iostream>
#include <exception>
using namespace std;

class MyException : public exception {
public:
    const char* what() const noexcept override {
        return "自定义异常: 出错了!";
    }
};

int main() {
    try {
        throw MyException();
    } 
    catch (const exception& e) {
        cout << e.what() << endl;
    }
}
```

## 常见标准异常

| 异常                | 描述                                                         |
| -------------------- | ------------------------------------------------------------- |
| std::runtime_error  | 理论上不可以通过读取代码来检测到的异常。                      |
| std::logic_error    | 理论上可以通过读取代码来检测到的异常。                        |
| std::out_of_range   | 该异常可以通过方法抛出，例如 std::vector 和 std::bitset<>::operator[]()。 |
| std::invalid_argument | 当使用了无效的参数时，会抛出该异常。                       |
| std::exception      | 该异常是所有标准 C++ 异常的父类。                             |
| std::bad_alloc      | 该异常可以通过 new 抛出。                                    |
| std::bad_cast       | 该异常可以通过 dynamic_cast 抛出。                           |
| std::bad_exception  | 这在处理 C++ 程序中无法预期的异常时非常有用。                 |
| std::bad_typeid     | 该异常可以通过 typeid 抛出。                                 |
| std::domain_error   | 当使用了一个无效的数学域时，会抛出该异常。                   |
| std::length_error   | 当创建了太长的 std::string 时，会抛出该异常。                 |
| std::overflow_error | 当发生数学上溢时，会抛出该异常。                             |
| std::range_error    | 当尝试存储超出范围的值时，会抛出该异常。                     |
| std::underflow_error| 当发生数学下溢时，会抛出该异常。                             |

