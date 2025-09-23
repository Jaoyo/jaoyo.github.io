---
title: C++ 继承
date: 2025-09-07 23:00:00 +0800
categories: [编程语言, C++]
tags: [C++语言]
description: 
---

当创建一个类时，可以不需要重新编写新的数据成员和成员函数，只需指定新建的类继承了一个已有的类的成员即可。这个已有的类称为**基类**，新建的类称为**派生类**。

## 基类&派生类

一个类可以派生自多个类，这意味着，它可以从**多个基类继承**数据和函数。

定义一个派生类，我们使用一个类派生列表来指定基类。类派生列表以一个或多个基类命名，形式如下：

```c++
class derived-class: access-specifier base-class
```

> 访问修饰符 `access-specifier` 是 *public*、*protected* 或 *private* 其中的一个，`base-class` 是之前定义过的某个类的名称。

假设有一个基类**Shape**，派生出了`Rectangle`类，示例如下：

```c++
#include <iostream>
 
using namespace std;

// 基类
class Shape 
{
   public:
      void setWidth(int w)
      {
         width = w;
      }
      void setHeight(int h)
      {
         height = h;
      }
   protected:
      int width;
      int height;
};

// 派生类
class Rectangle: public Shape
{
   public:
      int getArea()
      { 
         return (width * height); 
      }
};

int main(void)
{
   Rectangle Rect;
 
   Rect.setWidth(5);
   Rect.setHeight(7);

   // 输出对象的面积
   cout << "Total area: " << Rect.getArea() << endl;

   return 0;
} 
```

## 访问控制和继承

派生类可以访问基类中**所有的非私有成员**。因此基类成员如果不想被派生类的成员函数访问，则应在基类中声明为 `private`。

访问权限有如下表格的关系：

| --- | --- | --- | --- |
| 访问 		| public | protected | private |
| 同一个类   | 可以 | 可以 | 可以 |
| 派生类 	| 可以 | 可以 | 禁止 |
| 其他类		| 可以 | 禁止 | 禁止 |

一个派生类继承了所有的基类方法，但下列情况除外：

* 基类的构造函数、析构函数和拷贝构造函数。
* 基类的重载运算符。
* 基类的友元函数。

## 继承类型

* 三种继承方式对基类成员可见性的影响

| **继承方式**         | **基类 public 成员<br>在派生类中的权限** | **基类 protected 成员<br>在派生类中的权限** | **基类 private 成员<br>在派生类中的权限** |
| ---------------- | ------------------------ | --------------------------- | ------------------------- |
| **public 继承**    | 仍然是 **public**           | 仍然是 **protected**           | **不可访问**（只能通过基类的接口间接访问）   |
| **protected 继承** | 变为 **protected**         | 仍然是 **protected**           | **不可访问**                  |
| **private 继承**   | 变为 **private**           | 变为 **private**              | **不可访问**                  |

* public 继承（最常用）

	- 表达的是“is-a”关系，例如：Student 是一个 Person。

	- 基类的 public 保持可公开访问，protected 仍然保护，private 不直接继承。

	- 这是最符合面向对象设计直觉的继承方式。

* protected 继承（少用）

	- 表达的是“is-implemented-in-terms-of”关系，但不想让外部把它当作基类。

	- 基类的 public 变成 protected，即对派生类自己和子类可见，但外部不可见。

	- 常用于框架/库的内部继承机制。

* private 继承（更少用）

	- 表达的是“is-implemented-using”关系，类似于组合（但用继承实现）。

	- 基类的 public 和 protected 全部变为 private，外部完全不可见。

	- 派生类只是在内部借用了基类的实现，而不暴露其接口。

## 多继承

多继承即一个子类可以有多个父类，它继承了多个父类的特性。

C++ 类可以从多个类继承成员，语法如下：

```
class <派生类名>:<继承方式1><基类名1>,<继承方式2><基类名2>,…
{
<派生类类体>
};
```

程序示例如下：

```c++
#include <iostream>
 
using namespace std;

// 基类 Shape
class Shape 
{
   public:
      void setWidth(int w)
      {
         width = w;
      }
      void setHeight(int h)
      {
         height = h;
      }
   protected:
      int width;
      int height;
};

// 基类 PaintCost
class PaintCost 
{
   public:
      int getCost(int area)
      {
         return area * 70;
      }
};

// 派生类
class Rectangle: public Shape, public PaintCost
{
   public:
      int getArea()
      { 
         return (width * height); 
      }
};

int main(void)
{
   Rectangle Rect;
   int area;
 
   Rect.setWidth(5);
   Rect.setHeight(7);

   area = Rect.getArea();
   
   // 输出对象的面积
   cout << "Total area: " << Rect.getArea() << endl;

   // 输出总花费
   cout << "Total paint cost: $" << Rect.getCost(area) << endl;

   return 0;
}
```
