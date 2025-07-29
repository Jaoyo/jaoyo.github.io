---
title: C 终端进度条模拟
date: 2025-07-29 14:30:00 +0800
categories: [编程语言, C]
tags: [C语言]
description: 
---

**Windows**

```c
#include <stdio.h>
#include <windows.h>
#include <time.h>

void process() {
    srand(time(NULL));

    char style[5] = {'+', '-', '*', '>', '/'};
    char loading[120] = {};
    char out[150] = {};

    int cnt = 0;

    while (cnt <= 100) {
        // 格式1： [>>>>>  ]
        loading[cnt] = style[3];

        // memcpy(loading+cnt, &style[4], sizeof(style[4]));

        // 格式2：[===>   ]
        // loading[cnt] = '=';
        // loading[cnt+1] = style[3];

        sprintf(out, "[%-102s] [%d%%] [%c]", loading, cnt, style[cnt%5]);

        printf("%s\r", out);
        fflush(stdout);
        cnt++;

        Sleep(rand() % 201 + 10);
    }

    printf("\n");
}

int main() {
    process();
    return 0;
}
```

**Linux**

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <time.h>

void process() {
    srand(time(NULL));

    char style[5] = {'+', '-', '*', '>', '/'};
    char loading[120] = {};
    char out[150] = {};

    int cnt = 0;

    while (cnt <= 100) {
        // 格式1： [>>>>>  ]
        // loading[cnt] = style[3];

        // memcpy(loading+cnt, &style[4], sizeof(style[4]));

        // 格式2：[===>   ]
        loading[cnt] = '=';
        loading[cnt+1] = style[3];

        sprintf(out, "[%-102s] [%d%%] [%c]", loading, cnt, style[cnt%5]);

        printf("%s\r", out);
        fflush(stdout);
        cnt++;

        usleep(rand() % 201000 + 10000);
    }

    printf("\n");
}

int main() {
    process();
    return 0;
}
```