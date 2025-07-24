---
title: Rust函数
date: 2025-07-14 22:00:00 +0800
categories: [编程语言, Rust]
tags: [Rust]
published: false
description: 
---

> ```rust
> fn add(i: i32, j: i32) -> i32 {
>    i + j
> }
> ```

## 函数要点

- 函数名和变量名使用[蛇形命名法(snake case)](https://course.rs/practice/naming.html)，例如 `fn add_two() -> {}`
- 函数的位置可以随便放，Rust 不关心我们在哪里定义了函数，只要有定义即可
- 每个函数参数都需要标注类型

## 函数参数

Rust 是强类型语言，因此需要你为每一个函数参数都标识出它的具体类型，例如：

```rust
fn main() {
    another_function(5, 6.1);
}

fn another_function(x: i32, y: f32) {
    println!("The value of x is: {}", x);
    println!("The value of y is: {}", y);
}
```

## 函数返回

函数的返回值就是函数体最后一条表达式的返回值，当然我们也可以使用 `return` 提前返回。

### 无返回值()

例如单元类型 `()`，是一个零长度的元组。它没啥作用，但是可以用来表达一个函数没有返回值：

- 函数没有返回值，那么返回一个 `()`
- 通过 `;` 结尾的语句返回一个 `()`

### 永不返回的发散函数!

当用 `!` 作函数返回类型的时候，表示该函数永不返回( diverge function )，特别的，这种语法往往用做会导致程序崩溃的函数：

```rust
fn dead_end() -> ! {
  panic!("你已经到了穷途末路，崩溃吧！");
}
```

下面的函数创建了一个无限循环，该循环永不跳出，因此函数也永不返回：

```rust
fn forever() -> ! {
  loop {
    //...
  };
}
```
