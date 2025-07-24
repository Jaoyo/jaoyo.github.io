---
title: Rust基本类型
date: 2025-07-14 22:00:00 +0800
categories: [编程语言, Rust]
tags: [Rust]
published: false
description: 
---

## 基本类型

1. [数值类型](/_posts/language/rust/basic-type/2025-07-14-rust-Numerical.md)：
	* 有符号整数（i8，i16，i32，i64，isize）
	* 无符号整数（u8，u16，u32，u64，usize）
	* 浮点数（f32，f64）
	* 有理数、复数
2. [字符串](/_posts/language/rust/basic-type/2025-07-14-rust-char-bool.md) `&str`
3. 布尔类型 `true`和`false`
4. 字符类型：单个Uincde字符，存储为4个字节
5. 单元类型

## 复合类型

## 类型推导和标注

Rust编译器可以根据变量的值和上下文中的使用方法来自动推导出变量的类型

```rust
let guess = "42".parse().expect("Not a number!");

// 修改为 let guess: i32 = ...
// 或 "42".parse::<i32>
```


