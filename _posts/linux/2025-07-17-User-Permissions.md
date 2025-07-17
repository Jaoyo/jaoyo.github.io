---
title: Ubuntu用户权限配置
date: 2025-07-17 12:00:00 +0800
categories: [Linux]
tags: [Linux]
description: 
---

> 在Linux系统中，root的权限是最高的，所有一般都会禁止root用户直接登录ssh，使用普通用户登录有特殊需求的话，可以用`su`切换到root用户或使用`sudo`权限。

<!-- ### 新建普通用户 -->

### 新建普通用户并设置密码

```shell
# 按照提示输入密码，其他非必填可跳过
adduser newuser 
```

### 为普通用户添加超级用户权限

**方法1： 添加新用户到sudo用户组**

```shell
# 显示newuser所在的用户组
groups newuser

# 添加到sudo组
# -a 添加用户到指定的组
# -G sudo 指定了添加到的组
usermod -aG sudo newuser
```

**方法2：在/etc/sudoers中指定用户的权限(未验证)**

```shell
# 在sudo权限下执行
visudo
# 或
sudo vim /etc/sudoers

# 在文件中添加
newuser ALL=(ALL:ALL) ALL
# Ctrl-X,接着y退出
```

### 删除用户命令

```shell
deluser newuser

# 删除用户的同时删除/home/newusr
deluser --remove-home newuser
```

### 禁止root用户ssh登录

```vim
# 打开配置文件
vim /etc/ssh/sshd_config

# 找到PermitRootLogin参数，将yes改成no
PermitRootLogin no

# 保存文件并退出，重启sshd服务
systemctl restart sshd
```

### 普通用户免密切换到root用户(未验证)

在`/etc/sudoers`有这么一行

`%wheel ALL=(ALL) NOPASSWD: ALL` 

表示`wheel`组中的所有用户都可以免密码切换用户的权限，因此可以通过三种方法使普通用户拥有免密切换用户的权限

1. 修改普通用户的组为wheel
	```shell
   usermod -g wheel username
	```
2. 修改/etc/sudoers添加用户
   ```shell
   # 在/etc/sudoers中添加
   username ALL=(ALL) NOPASSWD: ALL
	```
3. 修改/etc/sudoers中的wheel组
   ```shell
   # 在/etc/sudoers中修改 %wheel ALL=(ALL) NOPASSWD: ALL 为
   username ALL=(ALL) NOPASSWD: ALL
	```

	遇到`sudo: /etc/sudo.conf is group writable`的问题，去掉`/etc/sudo.conf`文件的写入权限即可，即运行命令`chmod 440 /etc/sudo.conf`