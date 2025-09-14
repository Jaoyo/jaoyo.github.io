---
title: C++ 信号处理
date: 2025-09-14 23:00:00 +0800
categories: [编程语言, C++]
tags: [C++语言]
description: 
---

以下的信号动作被定义在C++头文件`<csignal>`中

| 信号 | 描述 |
| --- | --- |
| SIGABRT | 程序的异常终止，如调用 abort。 |
| SIGFPE | 错误的算术运算，比如除以零或导致溢出的操作。 |
| SIGILL | 检测非法指令。 |
| SIGINT | 接收到交互注意信号。 |
| SIGSEGV | 非法访问内存。 |
| SIGTERM | 发送到程序的终止请求。 |

## signal()函数

C++信号处理库提供了`signal()`函数，用于捕获突发事件

```c++
void (*signal (int sig, void (*func)(int)))(int); 
```

这个函数接收两个参数，第一个参数是一个整数，代表了信号的编号；第二个参数是一个指向信号处理函数的指针。

程序示例如下

```c++
#include <iostream>
#include <csignal>
#include <unistd.h>

using namespace std;

void signalHandler( int signum )
{
    cout << "Interrupt signal (" << signum << ") received.\n";

    // 清理并关闭
    // 终止程序  

   exit(signum);  

}

int main ()
{
    // 注册信号 SIGINT 和信号处理程序
    signal(SIGINT, signalHandler);  

    while(1){
       cout << "Going to sleep...." << endl;
       sleep(1);
    }

    return 0;
}
```

上面的代码执行时，会循环打印下面结果

```
Going to sleep....
Going to sleep....
Going to sleep....
```

当使用`ctrl+c`中断程序时，程序捕获到了信号，会进行打印

```
Going to sleep....
Going to sleep....
Going to sleep....
Interrupt signal (2) received.
```

## raise()函数

使用`raise()`函数可以生成信号

```c++
int raise (signal sig);
```

`sig`是要发送的信号编号，包括`SIGINT`,`SIGABRT`,`SIGFPE`,`SIGILL`,`SIGSEGV`,`SIGTERM`,`SIGHUP`等

```c++
#include <iostream>
#include <csignal>

using namespace std;

void signalHandler( int signum )
{
    cout << "Interrupt signal (" << signum << ") received.\n";

    // 清理并关闭
    // 终止程序 

   exit(signum);  

}

int main ()
{
    int i = 0;
    // 注册信号 SIGINT 和信号处理程序
    signal(SIGINT, signalHandler);  

    while(++i){
       cout << "Going to sleep...." << endl;
       if( i == 3 ){
          raise( SIGINT);
       }
       sleep(1);
    }

    return 0;
}
```

当上面的代码被编译和执行时，它会产生下列结果，并会自动退出：

```
Going to sleep....
Going to sleep....
Going to sleep....
Interrupt signal (2) received.
```

