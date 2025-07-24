---
title: C 函数
date: 2025-07-24 17:00:00 +0800
categories: [编程语言, C]
tags: [C语言]
description: 
---

> 函数是一段代码，只有在被调用时才会运行。
> 您可以将数据（称为参数）传递给函数。
> 函数用于执行某些操作，它们对于重用代码很重要：定义代码一次，多次使用。

## 函数的创建和调用

### 创建函数

```c
void myFunction() {
  // 要执行的代码
}
```

* `myFunction()` 是函数的名称
* `void` 表示函数没有返回值。 您将在下一章稍后了解有关返回值的更多信息
* 在函数（主体）内部，添加定义函数应该做什么的代码

### 调用函数

声明的函数不会立即执行。 它们被“保存以备后用”，并在调用时执行。

要调用函数，请编写函数名，后跟两个括号 `()` 和一个分号 `;`

在以下示例中，`myFunction()` 用于在调用时打印文本（操作）：

在 `main` 中，调用 `myFunction()`：

```c
// 创建函数
void myFunction() {
  printf("晚上好！");
}

int main() {
  myFunction();  // 调用函数
  return 0;
}
// 输出 -> "晚上好！"
```

一个函数可以被多次调用：

```c
void myFunction() {
  printf("晚上好！");
}

int main() {
  myFunction();
  myFunction();
  myFunction();
  return 0;
}

// 晚上好！
// 晚上好！
// 晚上好！
```

## 函数参数

信息可以作为参数(Parameters/Arguments)传递给函数。 参数在函数内部充当变量。

### 单个参数

```c
void myFunction(char name[]) {
  printf("Hello %s\n", name);
}

int main() {
  myFunction("Liam");
  myFunction("Jenny");
  myFunction("Kenny");
  return 0;
}

// Hello Liam
// Hello Jenny
// Hello Kenny
```

当一个 `parameter` (形参)被传递给函数时，它被称为一个 `argument`(实参)。 因此，从上面的示例中：`name` 是一个`parameter`，而`Liam`、`Jenny` 和`Anja` 是`arguments`。

### 多个参数

```c
void myFunction(char name[], int age) {
  printf("Hello %s. You are %d years old.\n", name, age);
}

int main() {
  myFunction("Liam", 3);
  myFunction("Jenny", 14);
  myFunction("Anja", 30);
  return 0;
}

// Hello Liam. You are 3 years old.
// Hello Jenny. You are 14 years old.
// Hello Anja. You are 30 years old.
```

请注意，当您使用多个参数(`parameters`)时，函数调用必须具有与参数(`arguments`)相同数量的参数(`parameters`)，并且参数(`arguments`)必须以相同的顺序传递。

### 返回值

函数定义时使用的 `void` 关键字表示函数不应返回值。如果希望函数返回值，可以使用数据类型（如`int`或`float`等）代替`void`，并在函数内部使用`return`关键字：

```c
// 一个输入参数
int myFunction1(int x) {
  return 5 + x;
}

// 两个输入参数
int myFunction2(int x, int y) {
  return x + y;
}

int main() {
  printf("Result is: %d", myFunction1(3));
  printf("Result is: %d", myFunction2(5, 3));

  // 将结果赋值保存
  int result = myFunction2(5, 3);
  printf("Result is = %d", result);

  return 0;
}

// 输出 8    (5 + 3)
```

## 函数递归

递归是使函数调用本身的技术。 这种技术提供了一种将复杂问题分解为更容易解决的简单问题的方法。

将两个数字相加很容易，但将一系列数字相加则比较复杂。 在以下示例中，递归用于将一系列数字相加，方法是将其分解为两个数字相加的简单任务：

```c
int sum(int k);

int main() {
  int result = sum(10);
  printf("%d", result);
  return 0;
}

int sum(int k) {
  if (k > 0) {
    return k + sum(k - 1);
  } else {
    return 0;
  }
}
```

当调用 `sum()` 函数时，它会将参数 `k` 添加到所有小于 `k` 的数字的总和中并返回结果。 当 `k` 变为 0 时，函数只返回 0。运行时，程序按以下步骤操作：

```
10 + sum(9)
10 + ( 9 + sum(8) )
10 + ( 9 + ( 8 + sum(7) ) )
...
10 + 9 + 8 + 7 + 6 + 5 + 4 + 3 + 2 + 1 + sum(0)
10 + 9 + 8 + 7 + 6 + 5 + 4 + 3 + 2 + 1 + 0
```
