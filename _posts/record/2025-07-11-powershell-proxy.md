---
title: powershell7设置代理
date: 2025-07-11 23:20:00 +0800
categories: [应用记录]
tags: [proxy, powershell]
description: 设置powershell7终端的流量走代理
---

> 作为程序员，都会有一个工具来对外网资源的加速访问。但是在powershell终端中进行外网资源访问时，常常会发生一些问题。因为一般的代理工具都是Web代理，即使开了全局模式，终端也无法使用，需要单独进行一些配置。

## powershell设置代理

首先，打开你的代理工具程序，查看你的代理工具端口

我用的是clash，点击右侧的常规就可以在中间面板看到端口号了。例如我的端口号是7890.  
*点击clash端口旁面的terminal终端图标可以一键进行配置，不过本文主要介绍使用命令的配置方法。*

![clash](https://cdn.jsdelivr.net/gh/JaoYoo/cdn/img/clash.png)

v2ray的端口号通过点击顶部的设置，或者直接查看底部状态栏查看

![v2ray](https://cdn.jsdelivr.net/gh/JaoYoo/cdn/img/v2ray.png)

以管理员模式打开powershell7终端，输入命令

```shell
$Env:http_proxy="http://127.0.0.1:7890"
$Env:https_proxy="http://127.0.0.1:7890"
```

注意，7890是你代理软件里的端口号

接下来，输入以下命令来测试你的代理连接情况

```shell
curl -I https://www.google.com
```

> *为什么使用curl测试代理而不使用ping命令呢？因为ping使用的是ICMP协议，属于TCP/IP协议，而代理通常用的是HTTP或Socks协议*

返回以下的提示，则代表powershell的代理配置成功

```shell
HTTP/1.1 200 Connection established

HTTP/1.1 200 OK
```

## 清除代理

powershell终端输入以下命令清除代理的配置

```shell
$Env:http_proxy=""
$Env:https_proxy=""
```

## !!!注意

以上的配置方法只针对本终端生效，即新建一个终端后代理配置效果不存在，要想代理配置对所有终端生效，需要把命令写入powershell的配置文件中

1. 在终端中输入命令查看系统配置文件的路径

    ```shell
    echo $PROFILE
    ```

2. 打开配置文件，若不存在则自己创建一个，在配置文件中添加代理配置

    ```shell
    $Env:http_proxy="http://127.0.0.1:7890"
    $Env:https_proxy="http://127.0.0.1:7890"
    ```

3. 保存退出，新建一个终端输入命令测试代理，返回200的话说明系统配置设置成功了

    ```shell
    curl -I https://www.google.com
    ```

## CMD配置

windows的命令提示符cmd使用以下命令配置

```shell
set HTTP_PROXY=http://127.0.0.1:7890
set HTTPS_PROXY=https://127.0.0.1:7890
```

通过curl命令测试代理

```shell
curl.exe -I http://www.google.com
curl.exe -I https://www.google.com
```

看到返回`HTTP/1.1 200 OK`代表配置成功

还原命令：

```shell
set HTTP_PROXY=
set HTTPS_PROXY=
```

## 其他的方法

网上其他博客介绍的是这种方法，不过我使用这种方法后并没有生效，可能这种方式适用于旧版的powershell。以下的命令都要在管理员模式下进行。

### 查看代理

```shell
netsh winhttp show proxy
```

### 设置代理

```shell
netsh winhttp set proxy 127.0.0.1:7890
```

### 取消代理

```shell
netsh winhttp reset proxy
```
