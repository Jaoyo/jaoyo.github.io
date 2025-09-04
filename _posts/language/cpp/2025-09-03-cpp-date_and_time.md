---
title: C++ 引用
date: 2025-09-03 23:00:00 +0800
categories: [编程语言, C++]
tags: [C++语言]
description: 
---

C++ 继承了 C 语言用于日期和时间操作的结构和函数。为了使用日期和时间相关的函数和结构，需要在 C++ 程序中引用` <ctime>` 头文件。

## 结构体`tm`

```c++
struct tm {
    int tm_sec;   // 秒，正常范围从 0 到 59，但允许至 61
    int tm_min;   // 分，范围从 0 到 59
    int tm_hour;  // 小时，范围从 0 到 23
    int tm_mday;  // 一月中的第几天，范围从 1 到 31
    int tm_mon;   // 月，范围从 0 到 11
    int tm_year;  // 自 1900 年起的年数
    int tm_wday;  // 一周中的第几天，范围从 0 到 6，从星期日算起
    int tm_yday;  // 一年中的第几天，范围从 0 到 365，从 1 月 1 日算起
    int tm_isdst; // 夏令时
}
```

## 获取当前日期和时间

```c++
#include <iostream>
#include <ctime>

using namespace std;

int main( )
{
    // 基于当前系统的当前日期/时间
    time_t now = time(0);
   
    // 把 now 转换为字符串形式
    char* dt = ctime(&now);

    cout << "本地日期和时间：" << dt << endl;

    // 把 now 转换为 tm 结构
    tm *gmtm = gmtime(&now);
    dt = asctime(gmtm);
    cout << "UTC 日期和时间："<< dt << endl;
}
```

## 使用结构 `tm` 格式化时间

`tm` 结构以 C 结构的形式保存日期和时间。大多数与时间相关的函数都使用了 `tm` 结构。

```c++
#include <iostream>
#include <ctime>

using namespace std;

int main(){
    // 基于当前系统的当前日期/时间
    time_t now = time(0);
    cout << "1970年1月1日到目前经过的秒数:" << now << endl;

    tm *ltm = localtime(&now);

    // 输出 tm 结构的各个组成部分
    cout << "年: "<< 1900 + ltm->tm_year << endl;
    cout << "月: "<< 1 + ltm->tm_mon<< endl;
    cout << "日: "<<  ltm->tm_mday << endl;
    cout << "时间: "<< 1 + ltm->tm_hour << ":";
    cout << 1 + ltm->tm_min << ":";
    cout << 1 + ltm->tm_sec << endl;
} 
```

## 常用函数

### time()

C 库函数 `time_t time(time_t *seconds)` 返回自纪元 `Epoch（1970-01-01 00:00:00 UTC）`起经过的时间，以秒为单位。如果 `seconds` 不为空，则返回值也存储在变量 `seconds` 中。

```c++
time_t time(time_t *seconds)
```

**参数**

`seconds`: 指向类型为`time_t`的对象的指针，用于存储`seconds`的值

**返回值**

以time_t对象返回当前日历时间

**用法**

```c++
#include <stdio.h>
#include <time.h>

int main ()
{
  time_t seconds;

  seconds = time(NULL);
  printf("自 1970-01-01 起的小时数 = %ld\n", seconds/3600);
  
  return(0);
}
```

### ctime()

C 库函数 `char *ctime(const time_t *timer)` 返回一个表示当地时间的字符串，当地时间是基于参数 `timer`。

返回的字符串格式如下： **Www Mmm dd hh:mm:ss yyyy** 其中，*Www* 表示星期几，*Mmm* 是以字母表示的月份，*dd* 表示一月中的第几天，*hh:mm:ss* 表示时间，*yyyy* 表示年份。

```c++
char *ctime(const time_t *timer)
```

**参数**

`timer`: 指向time_t对象的指针，该对象包含了一个日历时间

**返回值**

该函数返回了一个C字符串，该字符串包含了可读格式的日期和时间信息

**使用示例**

```c++
#include <stdio.h>
#include <time.h>

int main ()
{
   time_t curtime;

   time(&curtime);

   printf("当前时间 = %s", ctime(&curtime));

   return(0);
}
```

### localtime()

C 库函数 `struct tm *localtime(const time_t *timer)` 使用 `timer` 的值来填充 `tm` 结构。`timer` 的值被分解为 `tm` 结构，并用本地时区表示

```c++
struct tm *localtime(const time_t *timer)
```

**参数**

`timer`: 指向表示日历时间的time_t值的指针

**返回值**

该函数返回指向 tm 结构的指针，该结构带有被填充的时间信息

```c++
struct tm {
   int tm_sec;         /* 秒，范围从 0 到 59				*/
   int tm_min;         /* 分，范围从 0 到 59				*/
   int tm_hour;        /* 小时，范围从 0 到 23				*/
   int tm_mday;        /* 一月中的第几天，范围从 1 到 31	                */
   int tm_mon;         /* 月份，范围从 0 到 11				*/
   int tm_year;        /* 自 1900 起的年数				*/
   int tm_wday;        /* 一周中的第几天，范围从 0 到 6		        */
   int tm_yday;        /* 一年中的第几天，范围从 0 到 365	                */
   int tm_isdst;       /* 夏令时						*/	
};
```

**使用示例**

```c++
#include <stdio.h>
#include <time.h>

int main ()
{
   time_t rawtime;
   struct tm *info;
   char buffer[80];

   time( &rawtime );

   info = localtime( &rawtime );
   printf("当前的本地时间和日期：%s", asctime(info));

   return(0);
}
```

### clock()

C 库函数 `clock_t clock(void)` 返回程序执行起（一般为程序的开头），处理器时钟所使用的时间。为了获取 CPU 所使用的秒数，您需要除以 `CLOCKS_PER_SEC`。

在 32 位系统中，`CLOCKS_PER_SEC` 等于 1000000，该函数大约每 72 分钟会返回相同的值。

```c++
clock_t clock(void)
```

**返回值**

该函数返回自程序启动起，处理器时钟所使用的时间。如果失败，则返回 -1 值。

**使用示例**

```c++
#include <time.h>
#include <stdio.h>

int main()
{
   clock_t start_t, end_t, total_t;
   int i;

   start_t = clock();
   printf("程序启动，start_t = %ld\n", start_t);
    
   printf("开始一个大循环，start_t = %ld\n", start_t);
   for(i=0; i< 10000000; i++)
   {
   }
   end_t = clock();
   printf("大循环结束，end_t = %ld\n", end_t);
   
   total_t = (double)(end_t - start_t) / CLOCKS_PER_SEC;
   printf("CPU 占用的总时间：%f\n", total_t  );
   printf("程序退出...\n");

   return(0);
}
```

### asctime()

C 库函数 `char *asctime(const struct tm *timeptr)` 返回一个指向字符串的指针，它代表了结构 `struct timeptr` 的日期和时间。

```c++
char *asctime(const struct tm *timeptr)
```

**参数**

`timeptr` 是指向 `tm` 结构的指针，包含了分解为如下各部分的日历时间：

```c++
struct tm {
   int tm_sec;         /* 秒，范围从 0 到 59				*/
   int tm_min;         /* 分，范围从 0 到 59				*/
   int tm_hour;        /* 小时，范围从 0 到 23				*/
   int tm_mday;        /* 一月中的第几天，范围从 1 到 31	                */
   int tm_mon;         /* 月份，范围从 0 到 11				*/
   int tm_year;        /* 自 1900 起的年数				*/
   int tm_wday;        /* 一周中的第几天，范围从 0 到 6		        */
   int tm_yday;        /* 一年中的第几天，范围从 0 到 365	                */
   int tm_isdst;       /* 夏令时						*/	
};
```

**返回值**

该函数返回一个 C 字符串，包含了可读格式的日期和时间信息 **Www Mmm dd hh:mm:ss yyyy**，其中，*Www* 表示星期几，*Mmm* 是以字母表示的月份，*dd* 表示一月中的第几天，*hh:mm:ss* 表示时间，*yyyy* 表示年份。

**使用示例**

```c++
#include <stdio.h>
#include <string.h>
#include <time.h>

int main()
{
   struct tm t;

   t.tm_sec    = 10;
   t.tm_min    = 10;
   t.tm_hour   = 6;
   t.tm_mday   = 25;
   t.tm_mon    = 2;
   t.tm_year   = 89;
   t.tm_wday   = 6;

   puts(asctime(&t));
   
   return(0);
}
```

### gmtime()

C 库函数 `struct tm *gmtime(const time_t *timer)` 使用 `timer` 的值来填充 `tm` 结构，并用协调世界时（UTC）也被称为格林尼治标准时间（GMT）表示。

```c++
struct tm *gmtime(const time_t *timer)
```

**参数**

`timer`: 指向表示日历时间的time_t值指针

**返回值**

该函数返回指向 `tm` 结构的指针

```c++
struct tm {
   int tm_sec;         /* 秒，范围从 0 到 59				*/
   int tm_min;         /* 分，范围从 0 到 59				*/
   int tm_hour;        /* 小时，范围从 0 到 23				*/
   int tm_mday;        /* 一月中的第几天，范围从 1 到 31	                */
   int tm_mon;         /* 月份，范围从 0 到 11				*/
   int tm_year;        /* 自 1900 起的年数				*/
   int tm_wday;        /* 一周中的第几天，范围从 0 到 6		        */
   int tm_yday;        /* 一年中的第几天，范围从 0 到 365	                */
   int tm_isdst;       /* 夏令时						*/	
};
```

**使用示例**

```c++
#include <stdio.h>
#include <time.h>

#define BST (+1)
#define CCT (+8)

int main ()
{

   time_t rawtime;
   struct tm *info;

   time(&rawtime);
   /* 获取 GMT 时间 */
   info = gmtime(&rawtime );
   
   printf("当前的世界时钟：\n");
   printf("伦敦：%2d:%02d\n", (info->tm_hour+BST)%24, info->tm_min);
   printf("中国：%2d:%02d\n", (info->tm_hour+CCT)%24, info->tm_min);

   return(0);
}
```

### mktime()

C 库函数 `time_t mktime(struct tm *timeptr)` 把 `timeptr` 所指向的结构转换为一个依据本地时区的 `time_t` 值。

```c++
time_t mktime(struct tm *timeptr)
```

**参数**

`timeptr`: 指向表示日历时间的time_t值

**返回值**

该函数返回一个 time_t 值，该值对应于以参数传递的日历时间。如果发生错误，则返回 -1 值。

**使用示例**

```c++
#include <stdio.h>
#include <time.h>

int main ()
{
   int ret;
   struct tm info;
   char buffer[80];

   info.tm_year = 2001 - 1900;
   info.tm_mon = 7 - 1;
   info.tm_mday = 4;
   info.tm_hour = 0;
   info.tm_min = 0;
   info.tm_sec = 1;
   info.tm_isdst = -1;

   ret = mktime(&info);
   if( ret == -1 )
   {
       printf("错误：不能使用 mktime 转换时间。\n");
   }
   else
   {
      strftime(buffer, sizeof(buffer), "%c", &info );
      print(buffer);
   }

   return(0);
}
```

