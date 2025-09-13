---
title: C++ 动态内存
date: 2025-09-13 23:30:00 +0800
categories: [编程语言, C++]
tags: [C++语言]
description: 
---

C++程序中，内存分为：

* **栈**：在函数内部声明的所有变量都将占用栈内存。
* **堆**：程序中未使用的内存，在程序运行时可用于动态分配内存。

> 对于确定的变量，通常在**栈**上进行内存空间申请。
> 对于无法提前预知需要多少内存来存储某个定义变量的特定信息，所需内存的大小需要在运行时才能确定时，可以使用特殊的运算符为给定类型的变量在运行时分配堆内的内存。

申请动态内存时使用`new`，不需要动态内存需要释放时使用`delete`

## new和delete

### new

语法：

```c++
new data-type;
```

`data-type`可以是包含数组在内的任意内置的数据类型，也可以包括类或结构在内的用户自定义的任何数据类型。

使用示例：

```c++
double* pvalue  = NULL;
if( !(pvalue  = new double ))
{
   cout << "Error: out of memory." <<endl;
   exit(1);

}
```

上面的程序声明了一个指针变量`pvalue`，并且分配了一个`double`字节大小的内存

### delete

```c++
delete pvalue;	// 释放 pvalue 所指向的内存
```

使用示例：

```c++
#include <iostream>
using namespace std;

int main ()
{
   double* pvalue  = NULL; // 初始化为 null 的指针
   pvalue  = new double;   // 为变量请求内存
 
   *pvalue = 29494.99;     // 在分配的地址存储值
   cout << "Value of pvalue : " << *pvalue << endl;

   delete pvalue;         // 释放内存

   return 0;
}
```

## 数组的动态内存分配

* 字符串数组

```c++
#include <iostream>
#include <cstring>
using namespace std;

int main ()
{
	char *pvalue = NULL;	// 初始化为null的指针
	pvalue = new char[20];	// 为变量请求内存

	strcpy(pvalue, "Hello World");
	
	cout << pvalue << endl;
	delete[] pvalue;
	return 0;
}
```

* 二维数组

```c++
#include <iostream>
using namespace std;

int main ()
{
	// 1. 分配行指针数组
	double **pvalue = new double*[3];
	
	// 2. 为每一行分配列，并且初始化
	for (auto i = 0; i < 3; i++) {
	    pvalue[i] = new double[4];
	    for (auto j = 0; j < 4; j++) {
	        pvalue[i][j] = i + j;
	    }
	}
	
	// 3. 输出数组
	for (auto i = 0; i < 3; i++) {
	    for (auto j = 0; j < 4; j++) {
	        cout << pvalue[i][j] << " ";
	    }
	    cout << endl;
	}
	
	// 4. 释放内存 (先释放每一行，再释放行指针数组)
	for (auto i = 0; i < 3; i++) {
	    delete[] pvalue[i];
	}
	
	delete[] pvalue;
	return 0;
}
```

## 对象的动态内存分配

```c++
#include <iostream>
using namespace std;

class Box
{
   public:
      Box() { 
         cout << "调用构造函数！" <<endl; 
      }
      ~Box() { 
         cout << "调用析构函数！" <<endl; 
      }
};

int main( )
{
   Box* myBoxArray = new Box[4];

   delete [] myBoxArray; // Delete array

   return 0;
}
```



