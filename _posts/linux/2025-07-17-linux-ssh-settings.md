---
title: Linux SSH配置
date: 2025-07-17 12:00:00 +0800
categories: [Linux]
tags: [Linux]
description: 
---

## SSH心跳

打开`/etc/ssh/sshd_config`，添加

```vim
ClientAliveInterval 300 # 表示300秒，即5分钟
ClientAliveCountMax 5   # 表示允许超时5次。
```

表示每过一段时间会发送一个KeepAlive请求，保证终端不会因为超时空闲而断开连接，当无响应次数达到`ClientAliveCountMax`时，就自动断开

## 修改端口、协议

ssh的默认端口为22

```vim
# 更改SSH端口，最好改为五位数以上，攻击者扫描到端口的机率也会下降。
Port 12323

# 禁用版本1协议, 因为其设计缺陷, 很容易使密码被黑掉。 
Protocol 2
```

指定特定用户、IP登录

```vim
# 允许特定IP、用户登录
ALLowUsers aliyun text@192.168.1.1,root@192.168.*

# 拒绝zhangsan、aliyun 帐户通过 SSH 登录系统
DenyUsers zhangsan aliyun
```

## 禁止root用户登录

```vim
PermitRootLogin no
```

## 禁止空密码登录

```vim
PermitEmptyPasswords no
```