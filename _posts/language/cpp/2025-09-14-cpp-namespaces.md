---
title: C++ 命名空间
date: 2025-09-14 15:30:00 +0800
categories: [编程语言, C++]
tags: [C++语言]
description: 
---

命名空间主要解决不同库中，相同名称的**命名冲突**问题，它可作为附加信息来区分不同库中相同名称的函数、类、变量等。

## 定义命名空间

命名空间的定义使用关键字 **namespace**，后跟命名空间的名称，如下所示：

```c++
namespace namespace_name {
	// 代码声明
}
```

为了调用带有命名空间的函数或变量，需要在前面加上命名空间的名称，如下所示：

```c++
name::code;
```

程序示例：

```c++
#include <iostream>
using namespace std;

namespace A {
    void print() { cout << "Hello from A\n"; }
}

namespace B {
    void print() { cout << "Hello from B\n"; }
}

int main() {
    A::print();  // 调用 A 的 print
    B::print();  // 调用 B 的 print
    return 0;
}
```

## using指令

使用 `using namespace` 指令，这样在使用命名空间时就可以不用在前面加上命名空间的名称。这个指令会告诉编译器，后续的代码将使用指定的命名空间中的名称。

```c++
#include <iostream>
using namespace std;

// 第一个命名空间
namespace first_space{
   void func(){
      cout << "Inside first_space" << endl;
   }
}
// 第二个命名空间
namespace second_space{
   void func(){
      cout << "Inside second_space" << endl;
   }
}
using namespace first_space;
int main ()
{
 
   // 调用第一个命名空间中的函数
   func();
   
   return 0;
}
```

> 使用`using namespace`容易在大型项目里引发冲突
{: .prompt-warn}

因此更推荐使用：

```c++
using std::cout;
using std::endl;
```

## 不连续的命名空间

在 C++ 里，命名空间可以 **分散定义在多个位置**，编译时会自动合并到一起。

```c++
#include <iostream>
using namespace std;

// 第一次定义
namespace MyLib {
    void foo() {
        cout << "foo()" << endl;
    }
}

// 在另一个地方再次定义
namespace MyLib {
    void bar() {
        cout << "bar()" << endl;
    }
}

int main() {
    MyLib::foo();
    MyLib::bar();
    return 0;
}
```

作用：

* 在大型工程中，不同文件里扩展同一个命名空间，方便模块化
* 不连续的命名空间可以在头文件定义接口，并且在源文件中实现

## 嵌套命名空间

```c++
namespace Outer {
    namespace Inner {
        void foo() { cout << "Inner foo\n"; }
    }
}

int main() {
    Outer::Inner::foo();
}
```

在C++17中可以简写

```c++
namespace Outer::Inner {
    void foo() { cout << "Inner foo\n"; }
}
```

## 匿名命名空间

```c++
namespace {
    int hidden = 42;
    void secret() { cout << "secret\n"; }
}
```

* 匿名命名空间的内容只在当前文件可见，相当于`static`作用域
* 常用于实现文件内部的私有函数或变量

## 命名空间别名

```c++
namespace VeryLongNamespaceName {
    void foo() { cout << "foo\n"; }
}

namespace VLN = VeryLongNamespaceName;

int main() {
    VLN::foo();  // 简化调用
}
```
