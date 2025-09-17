---
title: C++ Vector
date: 2025-09-17 17:30:00 +0800
categories: [编程语言, C++]
tags: [C++语言]
description: 
---

`vector`是一个能够存放任意类型的动态数组，能够增加和压缩数据。

`vector`的用法类似于Python中的`list`数据类型。

## 基本概念

* `std::vector`是**动态数组**，支持随机访问
* 内存连续存储，支持下标操作
* 插入/删除尾部效率高，在中间或头部插入效率低
* 会自动扩容，容量(`capacity`) ≥ 元素个数(`size`)

## 容器特性

1. 顺序特性

	顺序容器中的元素按照严格的线性顺序排序，可以通过元素在序列中的位置访问对应的元素

2. 动态数组

	支持对序列中的任意元素进行快速直接访问，甚至可以通过指针算术进行该操作。

3. 能够感知内存分配器

	容器使用一个内存分配器对象来动态地处理它的存储需求

## 基本函数实现

1. 构造函数

	* `vector()`: 创建一个空的`vector`
	* `vector(int nSize)`: 创建一个`vector`，元素个数为nSize
	* `vector(int nSize, const t& t)`: 创建一个`vector`，元素个数为nSize，且值均为`t`
	* `vector(const vector&)`: 复制构造函数
	* `vector(begin, end)`: 复制`[begin, end)`区间内的另一个数组的元素到`vector`中

2. 增加函数

	* `void push_back(const T& x)`: 向量尾部添加一个元素x
	* `iterator insert(iterator it, const T& x)`: 向量中迭代器指向元素前增加一个元素x
	* `iterator insert(iterator it, int n, const T& x)`: 向量中迭代器指向元素前增加n个相同的元素x
	* `iterator insert(iterator it, const_iterator first, const_iterator last)`: 向量中迭代器指向元素前插入另一个相同类型向量的`[first,last)`间的数据

3. 删除函数

	* `iterator erase(iterator it)`: 删除向量中迭代器指向元素
	* `iterator erase(iterator first, iterator last)`: 删除向量中`[first,last)`中元素
	* `void pop_back()`: 删除向量中最后一个元素
	* `void clear()`: 清空向量中所有元素

4. 遍历函数

	* `reference at(int pos)`: 返回pos位置元素的引用
	* `reference front()`: 返回首元素的引用
	* `reference back()`: 返回尾元素的引用
	* `iterator begin()`: 返回向量头指针，指向第一个元素
	* `iterator end()`: 返回向量尾指针，指向向量最后一个元素的下一个位置
	* `reverse_iterator rbegin()`: 反向迭代器，指向最后一个元素
	* `reverse_iterator rend()`: 反向迭代器，指向第一个元素之前的位置

5. 判断函数

	* `bool empty() const`: 判断向量是否为空，若为空，则向量中无元素

6. 大小函数

	* `int size() const`: 返回向量中元素的个数
	* `int capacity() const`: 返回当前向量所能容纳的最大元素值
	* `int max_size() const`: 返回最大可允许的 vector 元素数量值

7. 其他函数

	* `void swap(vector&)`: 交换两个同类型向量的数据
	* `void assign(int n, const T& x)`: 设置向量中前n个元素的值为x
	* `void assign(const_iterator first, const_iterator last)`: 向量中`[first,last)`中元素设置成当前向量元素

8. 常用成员方法

| 方法                    | 作用                   |
| --------------------- | -------------------- |
| `push_back(x)`        | 在尾部插入元素              |
| `pop_back()`          | 删除尾部元素               |
| `insert(pos, x)`      | 在指定位置插入元素            |
| `erase(pos)`          | 删除指定位置元素             |
| `erase(first, last)`  | 删除范围内元素              |
| `clear()`             | 删除所有元素               |
| `operator[]`          | 通过下标访问元素（不检查越界）      |
| `at(i)`               | 通过下标访问元素（检查越界，抛异常）   |
| `front()`             | 返回第一个元素              |
| `back()`              | 返回最后一个元素             |
| `begin()`             | 返回指向第一个元素的迭代器        |
| `end()`               | 返回指向最后一个元素后面的位置迭代器   |
| `rbegin()` / `rend()` | 反向迭代器                |
| `size()`              | 当前元素个数               |
| `capacity()`          | 当前分配的空间大小            |
| `resize(n)`           | 改变大小，多余元素会被删除或新元素初始化 |
| `reserve(n)`          | 预留容量，避免频繁扩容          |
| `shrink_to_fit()`     | 把容量缩小到正好容纳当前元素数      |
| `empty()`             | 判断是否为空               |
| `swap(v2)`            | 与另一个 vector 交换内容     |

## 常见操作

```c++
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> v;   // 定义一个空 vector<int>

    // 插入
    v.push_back(10);
    v.push_back(20);
    v.push_back(30);

    // 遍历
    cout << "Elements: ";
    for (int x : v) cout << x << " ";
    cout << endl;

    // 下标访问
    cout << "v[1] = " << v[1] << endl;

    // 大小
    cout << "size=" << v.size() << " capacity=" << v.capacity() << endl;

    // 删除最后一个
    v.pop_back();

    // 插入指定位置
    v.insert(v.begin() + 1, 15);

    // 删除指定位置
    v.erase(v.begin());  

    // 清空
    v.clear();

    cout << "Final size=" << v.size() << endl;
}
```

## vector对象的定义和初始化

```c++
#include <vector>
using namespace std;
```

`std::vector`是一个**类模板（class template）**，在编译时根据指定的元素类型`T`生成对应的类

```c++
template <
	class T,								// 存储的元素类型
	class Allocator = std::allocator<T>		// 内存分配器（默认用std::allocator>
> class vector;	
```

例如：

```c++
vector<int> v1;		// 实例化为存int的动态数组
vector<string> v2;  // 实例化为存string的动态数组
```


## 和数组对比

* `vector` 动态分配，支持自动扩容，安全且方便。
* 普通数组大小固定，越界访问不会报错。
* 推荐优先用 `vector` 代替裸数组。