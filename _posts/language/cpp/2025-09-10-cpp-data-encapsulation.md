---
title: C++ 数据封装
date: 2025-09-11 15:00:00 +0800
categories: [编程语言, C++]
tags: [C++语言]
description: 
---

数据封装（Encapsulation） 是 **面向对象编程（OOP）** 的三大特性之一。它的核心思想是 把数据和操作数据的函数绑定在一起，并通过访问权限来保护数据。

也就是说，对象的内部状态对外部隐藏，只能通过类提供的接口访问。

## 实现方式

数据封装主要依赖**类**和**访问控制符**

* `public`: 对所有代码可见
* `protected`: 对子类和本类可见
* `private`: 仅对本类可见

## 封装示例

```c++
#include <iostream>
using namespace std;

class Student {
private:
    string name;   // 私有数据，外部无法直接访问
    int age;

public:
    // 构造函数
    Student(string n, int a) : name(n), age(a) {}

    // 公有接口：修改和访问数据
    void setAge(int a) {
        if (a > 0 && a < 150) {
            age = a;
        } else {
            cout << "非法年龄!" << endl;
        }
    }

    int getAge() const {
        return age;
    }

    string getName() const {
        return name;
    }
};

int main() {
    Student s("Alice", 20);

    cout << s.getName() << " 的年龄是 " << s.getAge() << endl;

    s.setAge(25);   // ✅ 正确
    cout << "更新后的年龄: " << s.getAge() << endl;

    // s.age = -100; ❌ 错误，age 是 private
}
```

## 数据封装和数据抽象对比

| 特性   | 数据封装                               | 数据抽象                       |
| ---- | ---------------------------------- | -------------------------- |
| 关注点  | **隐藏数据**（内部状态保护）                   | **隐藏实现**（只暴露接口）            |
| 实现方式 | private/protected/public           | 抽象类、虚函数                    |
| 目的   | 防止外部随意修改数据                         | 让用户只关心功能而非实现               |
| 举例   | `private int age;` + getter/setter | `virtual void draw() = 0;` |

> 封装是手段（保护数据），抽象是目的（简化接口）