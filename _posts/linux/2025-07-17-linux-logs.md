---
title: Linux日志记录查看
date: 2025-07-17 12:00:00 +0800
categories: [Linux]
tags: [Linux]
description: 
---

#### 日志记录文件夹

机器的日志信息存放在`/var/log`下，可以查看日志信息是否还存在或者是否被清空

```shell
/var/log/messages: 记录 Linux 内核消息及各种应用程序的公共日志信息

/var/log/cron: 记录 crond 计划任务产生的事件信息

/var/log/dmesg: 记录 Linux 操作系统在引导过程中的各种事件信息

/var/log/maillog: 记录进入或发出系统的电子邮件活动

/var/log/lastlog: 记录每个用户最近的登录事件

/var/log/secure: 记录用户认证相关的安全事件信息

/var/log/wtmp: 记录每个用户登录、注销及系统启动和停机事件

/var/log/btmp: 记录失败的、错误的登录尝试及验证事件
```

#### 登录失败日志

`lastlog`命令对应的日志是`var/log/lastlog`，记录了最近一次的登录用户。
如果一个用户从未登陆过，`lastlog`显示`**Never logged**`

#### 查看当前登录全部用户

`who`命令对应的日志是`/var/run/utmp`

#### 查看机器创建以来登陆过的用户

`last`命令对应的日志是`/var/log/wtmp`

