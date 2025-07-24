---
title: Rust字符、布尔、单元类型
date: 2025-07-14 22:00:00 +0800
categories: [编程语言, Rust]
tags: [Rust]
published: false
description: 
---

## 字符类型char

```rust
fn main() {
    let c = 'z';
    let z = 'ℤ';
    let g = '国';
    let heart_eyed_cat = '😻';
}
```

所有的`Unicode`值都可以作为Rust的字符，范围从`U+0000 ~ U+D7FF`和`U+E000 ~ U+10FFFF`。`Unicode`都是4个字节编码，所以字符类型占用为4个字节。


## 布尔类型

布尔类型的值为：`true`和`false`，内存占用大小为1个字节


## 单元类型

单元类型就是`()`，唯一的值也是`()`，完全不占用内存空间

