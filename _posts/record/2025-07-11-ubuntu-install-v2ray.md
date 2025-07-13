---
title: Ubuntu解压安装v2ray
date: 2025-07-11 23:20:00 +0800
categories: [应用记录]
tags: [Linux, proxy, v2ray]
description: Ubuntu系统下安装v2ray的过程记录。
---

## 前期准备

1. Ubuntu(或者其他Linux发行版也行，步骤应该差不多)
2. v2节点

## 安装过程

网上推荐的都是官方的一键脚本安装方法，不过在我这边使用这种方法安装有很多问题，比如说安装方法太老官方已经不再支持、下载内容的时候发生错误等，最后通过直接解压可执行文件的方式来完成安装配置，本文就这过程记录一下。

### 下载v2ray

#### 方式一：通过其他设备下载然后上传到Ubuntu主机中

#### 方式二：使用wget下载

1. 首先，找到V2Ray的发行版程序。

    进入github的[v2ray仓库](https://github.com/v2fly/v2ray-core/releases)，找到下载的版本，这里选择的是最新的5.3.0版本，在`Assets`列表中找到系统对应的发行包版本，例如我是x86-64的Ubuntu，这里选择了`v2ray-linux-64.zip`。右键获取下载地址直接下载，或者进入[`Github加速网站`](https://gh.api.99988866.xyz/)粘贴下载地址获取加速后的下载地址

2. 下载v2ray发行包

    下载V2Ray的发行版程序,解压压缩包并查看目录中的文件。

    ```shell
    wget https://gh.ddlc.top/https://github.com/v2fly/v2ray-core/releases/download/v5.3.0/v2ray-linux-64.zip
    unzip v2ray-linux-64.zip
    ls
    ```

    压缩包内包含了以下文件：
    - config.json : 节点的配置文件
    - v2ray & v2ctl : V2Ray的主程序以及控制程序
    - geoip.dat & geosite.dat : 程序所需要数据文件
    - systemd/ : 目录下包含了用于生成服务的文件

3. 配置环境

    依次执行以下命令，将v2ray服务所需要的文件配置到相关的位置

    ```shell
    mkdir /usr/local/bin/v2ray
    cp v2ray /usr/local/bin/v2ray/v2ray
    cp v2ctl /usr/local/bin/v2ray/v2ctl
    cp geoip.dat /usr/local/bin/v2ray/geoip.dat
    cp geosite.dat /usr/local/bin/v2ray/geosite.dat

    mkdir /etc/v2ray
    cp vpoint_vmess_freedom.json /etc/v2ray/config.json
    ```

    接下来是修改配置，将v2ray的服务器信息填入`/etc/v2ray/config.json`中，具体操作可以在Windows端V2Ray客户端（例如v2rayN）中，右键节点信息，选择`导出所选服务器为客户端配置`，保存下来，将里面的相关信息填入到`/etc/v2ray/config.json`中，或者简单粗暴直接替换掉。

    注意，请先把`/etc/v2ray/config.json`用命令`sudo chmod 766 /etc/v2ray/config.json`修改为可写入文件。

    最后是systemctl服务的生成，方便V2Ray的管理。

    ```shell
    sudo cp systemd/system/v2ray.service /usr/lib/systemd/system/

    mkdir /var/log/v2ray/
    touch /var/log/v2ray/access.log
    touch /var/log/v2ray/error.log
    touch /var/run/v2ray.pid
    ```

    同时对`/etc/v2ray/config.json`的第一项log修改为

    ```json
    "log": {
        "access": "/var/log/v2ray/access.log",
        "error": "/var/log/v2ray/error.log",
        "loglevel": "warning"
    },
    ```

    最后，运行v2ray程序并查看它的状态

    ```shell
    systemctl start v2ray
    systemctl status v2ray
    ```

    运行`systemctl status v2ray`没有错误一般就是启动成功了。

    同时，可以通过

    `systemctl enable v2ray`

    或者

    `systemctl disable v2ray`

    来设置v2ray服务是否开机自启动

4. 注意事项

    1. 解压后的文件都是只读文件，编辑的时候需要使用命令`chmod 766 [filename]`去授予读写权限，修改完配置后最好再用命令`chmod 644 [filename]`将权限改回来
    2. 如果运行`system status v2ray`发现类似于`Active: failed (Result: start-limit-hit)`的错误的话，一般都是路径配置有问题，使用`vim /usr/lib/systemd/system/v2ray.service`查看systemctl服务配置文件，配置中有一行`ExecStart= xxx`的，直接命令行运行xxx里面的内容，就知道你的配置哪里出问题了

### 启动终端代理

用`vim ~/.bashrc`打开用户的配置文件，在行末添加如下的内容：

```shell
# set proxy
function setproxy() {
    export http_proxy=socks5://127.0.0.1:10808
    export https_proxy=socks5://127.0.0.1:10808
    export ftp_proxy=socks5://127.0.0.1:10808
    export no_proxy="172.16.x.x"
}
# unset proxy
function unsetproxy() {
    unset http_proxy https_proxy ftp_proxy no_proxy
}
```

运行`source ~/.bashrc`使配置生效并重启终端，可以通过`setproxy`或`unsetproxy`来启动或者关闭终端的代理（仅对命令行生效），在开启了代理后，可以使用命令`curl https://www.google.com`来测试代理是否配置成功并生效，如果输入命令后一直没有输出并且最后返回了超时的信息就代表代理配置失败了，如果返回了一大堆的html语句就说明代理设置成功了。

## 最后

以上就是Ubuntu安装v2ray的过程记录，安装在其他Linux发行版，或者其他更新或者更老版本的v2ray方法都大同小异。
