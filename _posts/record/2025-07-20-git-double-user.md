---
title: 一个环境下配置两个Github账号
date: 2025-07-20 17:00:00 +0800
categories: [应用记录]
tags: [Git, Github]
description: 
---

> 想象一下，你平时工作时会用到一个 GitHub 账户，而周末维护一些自己的项目时又需要使用到自己的 Github 账号，恰巧它们还都发生在了同一台电脑上，这样你就会需要来回切换账号，是不是很头疼呢？

> 本文将带你在十分钟内为同一台电脑配置好两个 GitHub 账号，从此不再为切换账号而烦恼。

### 清除环境

> 这一步主要是避免配置冲突，可以跳过
{: .prompt-info}

在shell下进入`.ssh`目录，执行清理目录

```shell
rm -rf ~/.ssh/*
```

### 生成SSH密钥

#### 1. 生成第一个密钥

```shell
ssh-keygen -t ed25519 -C "personal@example.com" -f ~/.ssh/id_ed25519_personal
```

#### 2. 生成第二个密钥

```shell
ssh-keygen -t ed25519 -C "work@example.com" -f ~/.ssh/id_ed25519_work
```

执行完后会生成4个文件

```shell
id_ed25519_personal     id_ed25519_personal.pub
id_ed25519_work     id_ed25519_work.pub
```

### 为每个密钥配置对应的 GitHub 账号

```shell
cat id_ed25519_personal.pub
cat id_ed25519_work.pub
```

将公钥内容粘贴到[Github SSH keys配置](https://github.com/settings/keys)

### 配置 `~/.ssh/config`，自动匹配不同账号

编辑或创建 `~/.ssh/config`：

```shell
# 账号1
Host github-personal
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_personal
  IdentitiesOnly yes

# 账号2
Host github-work
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_work
  IdentitiesOnly yes

```

### 测试连接

```shell
ssh -T git@github-personal
ssh -T git@github-work
```

### 使用方法

当你想使用哪个账号推送时，就用 `git@github-xxx:user/repo.git` 的方式添加远程地址：

1. 克隆个人账号的项目：

```shell
git clone git@github-personal:yourusername/personal-repo.git
```

2. 克隆工作账号的项目：

```shell
git clone git@github-work:yourworkname/work-repo.git
```
