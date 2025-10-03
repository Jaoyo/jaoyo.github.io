---
title: C++ volatile
date: 2025-10-04 00:00:00 +0800
categories: [编程语言, Cpp]
tags: [C++语言]
description: 
---

`volatile`是一个类型修饰符，作用是告诉编译器：

**这个变量的值随时可能被外部因素改变（不是由当前代码逻辑控制）**，因此 **编译器不要对它进行优化**，每次使用时都必须从内存中重新读取，而不是使用寄存器或缓存的值。

## 基本语法

```c++
volatile int x;
```

声明`x`是一个易变变量

## 使用场景

1. 硬件寄存器访问

	- 在嵌入式编程里，硬件寄存器的值可能随时由外部设备修改

	```c++
	volatile unsigned int *UART_STATUS = (unsigned int*)0x4000;
	while ((*UART_STATUS & 1) == 0) {
		// 等待硬件置位
	}
	```

2. 多线程共享变量

	- 当多个线程访问同一个变量时，如果没有增加`volatile`，编译器可能把值缓存到寄存器里，导致其他线程对该变量的修改不可见

	```c++
	volatile bool stop = false;

	void therad_func() {
		while (!stop) {
			// 循环工作
		}
	}
	```

	> `volatile` 并不能保证原子性或内存可见性顺序，所以多线程同步更应该使用 `std::atomic`
	{: .prompt-warn}

3. 信号处理函数

	- 在信号处理函数（如`SIGINT`的`handle`）里修改的标志变量也常用`volatile`

## volatile与const

`volatile`可以与`const`结合

```c++
const volatile int y;
```

这代表这个变量，代码里不能修改它，且编译器也不能假设它不变，必须每次从内存取

主要用于**外部环境能修改变量，但是程序自身不应该修改**的情况

例如：

1. 硬件寄存器只读

	- 程序访问一个温度传感寄存器，硬件会不断刷新它的值，但是程序只能读取不能覆盖写入

	```c++
	const volatile int *TEMP_REG = (int*)0x40001000;
	int t = *TEMP_REG;	// 每次读取都要从硬件寄存器拿最新值

2. 只读状态标志

	- 某些MCU单片机的标志位，硬件更新，程序只能读不能写

3. 内存映射只读数据

	- 比如DMA缓冲区，外设会更新它，但代码只能读

## 实例

```c++
/* Compile code without optimization option */
#include <stdio.h>
int main(void) {
  const int local = 10;
  int *ptr = (int *)&local;

  printf("Initial value of local : %d \n", local);	// 10

  *ptr = 100;

  printf("Modified value of local: %d \n", local);	// 10

  return 0;
}
```

如果将`local`变量的声明修改为`const volatile int local`，那么第二次输出的结果才是20
