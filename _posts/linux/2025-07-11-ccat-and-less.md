---
title: Linux cat以及less命令的语法高亮配置
date: 2025-07-11 23:20:00 +0800
categories: [Linux]
tags: [cat, less]
description: Linux cat以及less命令的语法高亮配置。
---

> 在Linux命令行下，要想方便地查看文件内容，通常会使用以下命令：cat、more、less、tail、head  
> 然而，这几个命令的输出都不带有任何颜色标记  
> 即使用这几个命令查看代码文件时，没有语法高亮显示
> 下面就来介绍一下如何将这几个命令的输出设置成语法高亮

## ccat

cat命令来源于英语单词concatenate的缩写，主要用于查看文件内容，适合查看内容较少、纯文本的文件。  
cat命令同时也可以将多个文件的内容进行连接并打印到标准输出。  
它是一个非常常用的命令。  

ccat则是一个功能类似于cat的命令，不同点在于对于输出内容，ccat会进行语法高亮
对于显示代码文件时，语法高亮会更加容易阅读  

在Ubuntu下，ccat可以采用以下的方法安装

1. 进入到`下载`或`Downloads`目录
2. 使用以下命令下载ccat源码归档压缩包
    `wget https://github.com/jingweno/ccat/releases/download/v1.1.0/linux-amd64-1.1.0.tar.gz`
3. 解压下载的压缩包
    `tar xfz linux-amd64-1.1.0.tar.gz`
4. 将ccat执行文件复制到系统$PATH中，这样在输入ccat时就可以直接使用
    `sudo cp linux-amd64-1.1.0/ccat /usr/local/bin/`
5. 给ccat命令添加可执行权限
    `sudo chmod +x /usr/local/bin/ccat`
6. 安装完成，输入`ccat <文件名>`就可以看到带语法高亮的文档输出

如果你想将系统默认的cat命令替换为ccat的话，可以采使用alias别名设置：

1. 打开系统配置文件
    `sudo vim ~/.bashrc`
2. 在最后一行输入
    `alias cat=/usr/local/bin/ccat`
3. 使.bashrc生效
    `source ~/.bashrc`

这样输入`cat`命令时就默认使用`ccat`可执行文件了

## less

1. 安装source-highlight
    `sudo apt-get install source-highlight`

2. 配置less
    打开`.bashrc`文件，加上以下配置
    `export LESSOPEN="| /usr/share/source-highlight/src-hilite-lesspipe.sh %s"`
    `export LESS=" -R"`

3. 如果想输出的内容带有行号的话，添加以下配置
    `export LESS=" -N -R"`

4. 使配置生效
    `source ~/.bashrc`
