---
title: C++ 数据抽象
date: 2025-09-10 21:00:00 +0800
categories: [编程语言, Cpp]
tags: [C++语言]
description: 
---

数据抽象指的是，隐藏对象的具体实现，只暴露必要的接口。

数据抽象是一种依赖于接口和实现分离的编程（设计）技术。

## 用类封装数据和接口

```c++
#include <iostream>
using namespace std;

class BankAccount {
private:
    string owner;     // 账户持有人 —— 内部实现，外部不可直接访问
    double balance;   // 余额 —— 内部实现

public:
    BankAccount(string name, double money) {
        owner = name;
        balance = money;
    }

    void deposit(double amount) {  // 对外公开接口
        balance += amount;
    }

    void withdraw(double amount) { // 对外公开接口
        if (amount <= balance) balance -= amount;
        else cout << "余额不足！" << endl;
    }

    double getBalance() const {    // 对外只读接口
        return balance;
    }
};

int main() {
    BankAccount acc("Alice", 1000);

    acc.deposit(500);
    acc.withdraw(200);
    cout << "当前余额: " << acc.getBalance() << endl;

    // acc.balance = -99999; ❌ 错误！balance 是私有的，不能直接访问
}
```

用户只能通过 `deposit`、`withdraw`、`getBalance` 来操作账户，无法直接修改 `balance`。

## 抽象类+纯虚函数

```c++
class Shape {
public:
    virtual void draw() = 0;   // 纯虚函数：接口
    virtual double area() = 0;
};

class Circle : public Shape {
private:
    double r;
public:
    Circle(double radius): r(radius) {}
    void draw() override { cout << "画一个圆" << endl; }
    double area() override { return 3.14 * r * r; }
};

class Rectangle : public Shape {
private:
    double w, h;
public:
    Rectangle(double width, double height): w(width), h(height) {}
    void draw() override { cout << "画一个矩形" << endl; }
    double area() override { return w * h; }
};
```

用户只依赖 `Shape` 提供的 **抽象接口**，而不必关心 `Circle` 或 `Rectangle` 的具体实现。