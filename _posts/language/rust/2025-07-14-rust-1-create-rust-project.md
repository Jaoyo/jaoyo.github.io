---
title: 创建Rust项目
date: 2025-07-14 22:00:00 +0800
categories: [编程语言, Rust]
tags: [Rust]
published: false
description: 
---

## 新建项目

```shell
cargo new hello_world
cd hello_world
```

使用 `cargo new` 创建一个项目，项目名为 `hello_world`，该项目的结构和配置文件都是由 `cargo` 生成

项目分为 `bin` 和 `lib` ，`bin` 为一个可运行的项目， `lib` 为以一个依赖库项目，项目的目录如下：

```shell
tree -a

.
├── .git
├── .gitignore
├── Cargo.toml
└── src
    └── main.rs
```


## 运行项目

1. 命令运行

    ```shell
    cargo run
    ```

2. 手动生成运行

    ```shell
    cargo build
    ./target/debug/hello_world
    ```

3. 程序发布

    ```shell
    cargo run --release
    cargo build --release
    ```

## 代码验证

使用 `cargo build` 或 `cargo run` 都需要一定的时间，如果只是检查代码而不生成，可以使用 `cargo check` 命令

他的作用是快速的检查一下代码是否编译通过


## Cargo.toml和Cargo.lock

`Cargo.toml` 和 `Cargo.lock` 是 `Cargo` 的核心文件，他的所有活动都基于这两者

* `Cargo.toml` 是 `Cargo` 特有的 **项目数据描述文件** ，存储了项目的所有元配置信息
* `Cargo.lock` 是 `Cargo` 工具根据同一项目的 `toml` 文件生成的**项目依赖详细清单**，一般不需要需改它，只需要修改`Cargo.toml`就行

什么情况下该把 `Cargo.lock` 上传到 git 仓库里？很简单，当你的项目是一个可运行的程序时，就上传 `Cargo.lock`，如果是一个依赖库项目，那么请把它添加到 `.gitignore` 中。


### Cargo.toml结构

#### package配置段落

`package`中记录了项目的详细信息

```toml
[package]
name = "world_hello"
version = "0.1.0"
edition = "2021"
```

`name` : 项目名称
`version` : 当前项目版本，默认0.1.0
`edition` : Rust的大版本号

#### 定义项目依赖

在 `Cargo.toml`中，主要通过各种依赖段落来描述该项目的各种依赖：
* 基于Rust官方仓库`crates.io`，通过版本说明来描述
* 基于项目源代码的git仓库地址，通过url来描述
* 基于本地项目的绝对路径或相对路径，通过类Unix模式的路径来描述

```toml
[dependencies]
rand = "0.3"
hammer = { version = "0.5.0"}
color = { git = "https://github.com/bjz/color-rs" }
geometry = { path = "crates/geometry" }
```

## 其他

1. VSCode 插件rust-analyzer在Ubuntu16.04上的运行问题  [Issues11558](https://github.com/rust-lang/rust-analyzer/issues/11558#issuecomment-1053979557)