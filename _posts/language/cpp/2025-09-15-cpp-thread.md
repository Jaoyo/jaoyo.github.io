---
title: C++ 多线程
date: 2025-09-14 23:30:00 +0800
categories: [编程语言, C++]
tags: [C++语言]
description: 
---

C++ 不包含多线程应用程序的任何内置支持。相反，它完全依赖于操作系统来提供此功能。

假设您使用的是 `Linux` 操作系统，我们要使用 `POSIX` 编写多线程 C++ 程序。`POSIX Threads` 或 `Pthreads` 提供的 API 可在多种`类 Unix POSIX` 系统上可用，比如 `FreeBSD`、`NetBSD`、`GNU/Linux`、`Mac OS X` 和 `Solaris`。

下面的例程，以`POSIX`为例子

## 创建线程

```c++
#include <pthread.h>
pthread_create (thread, attr, start_routine, arg) 
```

`pthread_create`创建一个新的线程，并且直接执行

| 参数 | 描述 |
| --- | --- |
| `thread` | 一个不透明的、唯一的标识符，用来标识例程返回的新线程。 |
| `attr` | 一个不透明的属性对象，可以被用来设置线程属性。<br>您可以指定线程属性对象，也可以使用默认值 NULL。 |
| `start_routine` | C++ 例程，一旦线程被创建就会执行。 |
| `arg` | 一个可能传递给 start_routine 的参数。<br>它必须通过把引用作为指针强制转换为 void 类型进行传递。<br>如果没有传递参数，则使用 NULL。 |

一个进程可以创建的最大线程数是依赖于实现的。线程一旦被创建，就是同等的，而且可以创建其他线程。线程之间没有隐含层次或依赖。

## 终止线程

```c++
#include <pthread.h>
pthread_exit (status) 
```

在这里，`pthread_exit` 用于显式地退出一个线程。通常情况下，`pthread_exit()` 例程是在线程完成工作后无需继续存在时被调用。

如果 `main()` 是在它所创建的线程之前结束，并通过 `pthread_exit()` 退出，那么其他线程将继续执行。否则，它们将在 `main()` 结束时自动被终止。

正常情况下，`main()`执行完成后，进程结束，那么`main()`创建的所有线程也会**强制终止**

如果在`main()`函数中添加`ptherad_exit()`，那么则只退出`main()`线程，进程不结束，`main()`创建的其他线程**继续运行**

## 连接和分离线程

```c++
pthread_join (threadid, status) 
pthread_detach (threadid) 
```

`pthread_join()` 子例程阻碍调用例程，直到指定的 `threadid` 线程终止为止。当创建一个线程时，它的某个属性会定义它是否是可连接的（`joinable`）或可分离的（`detached`）。只有创建时定义为可连接的线程才可以被连接。如果线程创建时被定义为可分离的，则它永远也不能被连接。