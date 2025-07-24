---
title: Rust变量绑定和解构
date: 2025-07-14 22:00:00 +0800
categories: [编程语言, Rust]
tags: [Rust]
published: false
description: 
---

## 变量绑定

```rust
let a = "hello world"
```

任何内存对象都是有主人，且完全属于他的主人的，绑定就是把这个对象绑定给一个变量，让这个变量成为他的主人

## 变量可变性

1. 变量默认情况下是 **不可变** 的
2. 使用 `mut` 关键字可以是变量变为 **可变的**

## 未使用变量

1. 创建变量但不使用会引发警告
2. 在变量前加入下划线 `_` ，可以忽略警告，例如 `_x`

## 变量结构

`let` 表达式可以进行复杂变量的结构：

```rust
fn main() {
	let (a, mut b): (bool, bool) = (true, false);
	// a = true，不可变；b = false，可变
	println!("a = {:?}, b = {:?}", a, b);

	b = true;
	assert_eq!(a, b);
}
```

也可以使用元组、切片和结构体模式去解析

```rust
struct Struct {
	e: i32;
}

fn main() {
	let (a, b, c, d, e);

	(a, b) = (1, 2);
	[c, .., d, _] = [1, 2, 3, 4, 5];
	Struct {e, ..} = Struct {e: 5};
	// _ 代表匹配一个值但不关心值是多少，.. 代表匹配几个值

	assert_eq!([1, 2, 1, 4, 5], [a, b, c, d, e]);
}
```

## 常量

1. 常量不允许使用 `mut`
2. 常量使用 `const` 关键字，并且必须标志值得类型

```rust
const MAX_POINTS: u32 = 100_000;
// 使用 _ 分隔数字，提高可读性
```

## 变量遮蔽

* 允许声明相同的变量名，后面声明的变量会屏蔽掉前面声明的变量

```rust
fn main() {
	let x = 5;
	let x = x + 1;

	{
		// 在花括号作用域内，对之前的x进行屏蔽
		let x = x * 2;
		println!("The value of x in the inner scope is: {}", x);
	}
	println!("The value of x is: {}", x);
}
```
