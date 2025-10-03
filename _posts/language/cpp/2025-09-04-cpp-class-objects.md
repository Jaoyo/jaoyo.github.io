---
title: C++ 类和对象
date: 2025-09-04 15:00:00 +0800
categories: [编程语言, Cpp]
tags: [C++语言]
description: 
---

## 类定义

定义了类的对象包括了什么，以及可以在这个对象上执行哪些操作。

类定义是以关键字 `class` 开头，后跟类的名称。类的主体是包含在一对花括号中。类定义后必须跟着一个分号或一个声明列表。

```c++
class Box
{
   public:
      double length;   // Length of a box
      double breadth;  // Breadth of a box
      double height;   // Height of a box

   private:
   	  double price;
};
```

## 对象定义

```c++
Box Box1;          // 声明 Box1，类型为 Box
Box Box2;          // 声明 Box2，类型为 Box
```

## C++类成员函数

类的成员函数是指那些把定义和原型写在类定义内部的函数，就像类定义中的其他变量一样。类成员函数是类的一个成员，它可以操作类的任意对象，可以访问对象中的所有成员。

### 声明

```c++
class Box
{
   public:
      double length;         // 长度
      double breadth;        // 宽度
      double height;         // 高度
      double getVolume(void);// 返回体积
};
```

### 定义

* 在类内部定义。在类定义中定义的成员函数把函数声明为**内联**的，即便没有使用 inline 标识符

```c++
class Box
{
   public:
      double length;      // 长度
      double breadth;     // 宽度
      double height;      // 高度
   
      double getVolume(void)
      {
         return length * breadth * height;
      }
};
```

* 单独使用**范围解析运算符 ::** 来定义

```c++
double Box::getVolume(void)
{
    return length * breadth * height;
}
```

## C++类访问修饰符

类成员的访问限制是通过在类主体内部对各个区域标记 `public`、`private`、`protected` 来指定的。关键字 `public`、`private`、`protected` 称为访问说明符。

### 公有成员**public**

**公有**成员在程序中类的外部是可访问的，可以不使用任何成员函数来设置和获取公有变量的值。

### 私有成员**private**

**私有**成员变量或函数在类的外部是不可访问的，甚至是不可查看的。只有类和友元函数可以访问私有成员。

默认情况下，类的所有成员都是私有的。例如在下面的类中，width 是一个私有成员，这意味着，如果您没有使用任何访问修饰符，类的成员将被假定为私有成员：

```c++
class Box
{
   double width;
   public:
      double length;
      void setWidth( double wid );
      double getWidth( void );
};
```

### 保护成员**protected**

**保护**成员变量或函数与私有成员十分相似，但有一点不同，保护成员在派生类（即子类）中是可访问的

```c++
#include <iostream>
using namespace std;
 
class Box
{
   protected:
      double width;
};
 
class SmallBox:Box // SmallBox 是派生类
{
   public:
      void setSmallWidth( double wid );
      double getSmallWidth( void );
};
 
// 子类的成员函数
double SmallBox::getSmallWidth(void)
{
    return width ;
}
 
void SmallBox::setSmallWidth( double wid )
{
    width = wid;
}
 
// 程序的主函数
int main( )
{
   SmallBox box;
 
   // 使用成员函数设置宽度
   box.setSmallWidth(5.0);
   cout << "Width of box : "<< box.getSmallWidth() << endl;
 
   return 0;
}
```

在这个实例中，`SmallBox`是父类`Box`的子类，`width`成员可以被派生类`SmallBox`的任何成员函数访问。

## C++类构造函数&析构函数

### 类的构造函数

类的**构造函数**是类的一种特殊的成员函数，它会在**每次创建类的新对象**时执行。

构造函数的名称与类的名称是完全相同的，并且不会返回任何类型，也不会返回 void。构造函数可用于为某些成员变量设置初始值。

```c++
#include <iostream>
 
using namespace std;
 
class Line
{
   public:
      void setLength( double len );
      double getLength( void );
      Line();  // 这是构造函数
 
   private:
      double length;
};
 
// 成员函数定义，包括构造函数
Line::Line(void)
{
    cout << "Object is being created" << endl;
}
 
void Line::setLength( double len )
{
    length = len;
}
 
double Line::getLength( void )
{
    return length;
}
// 程序的主函数
int main( )
{
   Line line;
 
   // 设置长度
   line.setLength(6.0); 
   cout << "Length of line : " << line.getLength() <<endl;
 
   return 0;
}
```

程序输出

```
Object is being created
Length of line : 6
```

### 带参数的构造函数

默认的构造函数没有任何参数，但如果需要，构造函数也可以带有参数。这样在创建对象时就会给对象赋初始值

```c++
#include <iostream>
 
using namespace std;
 
class Line
{
   public:
      void setLength( double len );
      double getLength( void );
      Line(double len);  // 这是构造函数
 
   private:
      double length;
};
 
// 成员函数定义，包括构造函数
Line::Line( double len)
{
    cout << "Object is being created, length = " << len << endl;
    length = len;
}
 
void Line::setLength( double len )
{
    length = len;
}
 
double Line::getLength( void )
{
    return length;
}
// 程序的主函数
int main( )
{
   Line line(10.0);
 
   // 获取默认设置的长度
   cout << "Length of line : " << line.getLength() <<endl;
   // 再次设置长度
   line.setLength(6.0); 
   cout << "Length of line : " << line.getLength() <<endl;
 
   return 0;
}
```

程序输出

```
Object is being created, length = 10
Length of line : 10
Length of line : 6
```

### 使用初始化列表来初始化字段

使用初始化列表来初始化字段：

```c++
Line::Line( double len): length(len)
{
    cout << "Object is being created, length = " << len << endl;
}
```

上面这段代码，在构造函数的同时初始化了`length`字段，等同于

```c++
Line::Line( double len)
{
    cout << "Object is being created, length = " << len << endl;
    length = len;
}
```

如果有多个字段初始化，也可以使用上面的语法

```c++
C::C( double a, double b, double c): X(a), Y(b), Z(c)
{
  ....
}
```

### 类的析构函数

类的**析构函数**是类的一种特殊的成员函数，它会在**每次删除所创建的对象时**执行

析构函数的名称与类的名称是完全相同的，只是在前面加了个波浪号（~）作为前缀，它不会返回任何值，也不能带有任何参数。析构函数有助于在跳出程序（比如关闭文件、释放内存等）前释放资源。

```c++
#include <iostream>
 
using namespace std;
 
class Line
{
   public:
      void setLength( double len );
      double getLength( void );
      Line();   // 这是构造函数声明
      ~Line();  // 这是析构函数声明
 
   private:
      double length;
};
 
// 成员函数定义，包括构造函数
Line::Line(void)
{
    cout << "Object is being created" << endl;
}
Line::~Line(void)
{
    cout << "Object is being deleted" << endl;
}
 
void Line::setLength( double len )
{
    length = len;
}
 
double Line::getLength( void )
{
    return length;
}
// 程序的主函数
int main( )
{
   Line line;
 
   // 设置长度
   line.setLength(6.0); 
   cout << "Length of line : " << line.getLength() <<endl;
 
   return 0;
}
```

上面这段程序在创建`line`对象时，自动调用**构造函数**，输出

```
Object is being created
```

当程序结束时，`line`对象**超出作用域**，自动调用**析构函数**，输出

```
Object is being deleted
```

程序输出结果

```
Object is being created
Length of line : 6
Object is being deleted
```

## C++ 拷贝构造函数

```c++
#include <iostream>
using namespace std;

class CExample {
private:
      int a;
public:
      //构造函数
      CExample(int b)
      { a = b;}

      //一般函数
      void Show ()
      {
        cout<<a<<endl;
      }
};
int main()
{
      CExample A(100);
      CExample B = A; //注意这里的对象初始化要调用拷贝构造函数，而非赋值
      B.Show ();
      return 0;
}
```

这个程序中，首先创建了对象A，并赋值`A.a=100`

接着创建对象B

```c++
CExample B = A;
```

这里是拷贝初始化，所以调用的是**拷贝构造函数**而不是赋值运算符

因为类中没有自定义的拷贝构造函数，编译器会生成一个**默认拷贝构造函数**，它会逐个复制成员变量，于是有

```c++
B.a = A.a = 100;
```

**拷贝构造函数**是一种特殊的构造函数，它在创建对象时，是使用同一类中之前创建的对象来初始化新创建的对象。拷贝构造函数通常用于：

* 通过使用另一个同类型的对象来初始化新创建的对象。
* 复制对象把它作为参数传递给函数。
* 复制对象，并从函数返回这个对象。

如果在类中没有定义拷贝构造函数，编译器会自行定义一个。如果类带有指针变量，并有动态内存分配，则它必须有一个拷贝构造函数。拷贝构造函数的最常见形式如下：

```c++
classname (const classname &obj) {
   // 构造函数的主体
}
```

下面是一个自定义拷贝构造函数的示例：

```c++
#include <iostream>

using namespace std;

class Line
{
   public:
      int getLength( void );
      Line( int len );             // 简单的构造函数
      Line( const Line &obj);  // 拷贝构造函数
      ~Line();                     // 析构函数

   private:
      int *ptr;
};

// 成员函数定义，包括构造函数
Line::Line(int len)
{
    cout << "Normal constructor allocating ptr" << endl;
    // 为指针分配内存
    ptr = new int;
    *ptr = len;
}

Line::Line(const Line &obj)
{
    cout << "Copy constructor allocating ptr." << endl;
    ptr = new int;
   *ptr = *obj.ptr; // copy the value
}

Line::~Line(void)
{
    cout << "Freeing memory!" << endl;
    delete ptr;
}
int Line::getLength( void )
{
    return *ptr;
}

void display(Line obj)
{
   cout << "Length of line : " << obj.getLength() <<endl;
}

// 程序的主函数
int main( )
{
   Line line1(10);

   Line line2 = line1; // 这里也调用了拷贝构造函数

   display(line1);
   display(line2);

   return 0;
}
```

1. 首先执行`Line line1(10);`:

	调用普通构造函数`Line::Line(int len)`，输出

	```
	Normal constructor allocating ptr
	```

	并完成`ptr`的初始化

2. 用`line1`初始化`line2`:

	执行`Line line2 = line1;`，这里会调用到拷贝构造函数，输出

	```
	Copy constructor allocating ptr.
	```

	再把`line2.ptr`指向一个新的内存堆，并且值也是为10

3. 调用`display(line1)`

	函数参数是按值传递，会调用拷贝构造函数，输出

	```
	Copy constructor allocating ptr.
	```

	然后执行打印

	```
	Length of line : 10
	```

	`display`执行完成后，释放临时对象`obj`，进行析构函数

	```
	Freeing memory!
	```

4. 调用`display(line2)`

	跟调用`display(line1)`的流程一样，输出

	```
	Copy constructor allocating ptr.
	Length of line : 10
	Freeing memory!
	```

5. 程序结束，`line1`和`line2`离开了作用域，依次进行析构

	```
	Freeing memory!
	Freeing memory!
	```

因此，上面这一段代码的执行结果为

```
Normal constructor allocating ptr
Copy constructor allocating ptr.
Copy constructor allocating ptr.
Length of line : 10
Freeing memory!
Copy constructor allocating ptr.
Length of line : 10
Freeing memory!
Freeing memory!
Freeing memory!
```

## C++友元函数

类的友元函数是定义在类外部，但有权访问类的所有私有（private）成员和保护（protected）成员。尽管友元函数的原型有在类的定义中出现过，但是友元函数并不是成员函数。

友元可以是一个函数，该函数被称为友元函数；友元也可以是一个类，该类被称为友元类，在这种情况下，整个类及其所有成员都是友元。

如果要声明函数为一个类的友元，需要在类定义中该函数原型前使用关键字 `friend`，如下所示：

```c++
class Box
{
	double width;
public:
	double length;
	friend void printWidth( Box box );
	void setWidth( double wid );
};
```

程序示例：

```c++
#include <iostream>
 
using namespace std;
 
class Box
{
   double width;
public:
   friend void printWidth( Box box );
   void setWidth( double wid );
};

// 成员函数定义
void Box::setWidth( double wid )
{
    width = wid;
}

// 请注意：printWidth() 不是任何类的成员函数
void printWidth( Box box )
{
   /* 因为 printWidth() 是 Box 的友元，它可以直接访问该类的任何成员 */
   cout << "Width of box : " << box.width <<endl;
}
 
// 程序的主函数
int main( )
{
   Box box;
 
   // 使用成员函数设置宽度
   box.setWidth(10.0);
   
   // 使用友元函数输出宽度
   printWidth( box );
 
   return 0;
}
```

## C++内联函数

在C++中，**内联函数**主要是与类一起使用，目的是提高程序的执行效率。如果一个函数是**内联函数**，那么编译时，编译器会把这个函数的代码副本放置到每个调用该函数的地方

如果想把一个函数定义为内联函数，则需要在函数名前面放置关键字 `inline`，在调用函数之前需要对函数进行定义。内联函数体一般是1-5行，如果超过，编译器会忽略 `inline` 限定符。

在类定义中的定义的函数都是内联函数，即使没有使用 `inline` 说明符。

程序示例：

```c++
#include <iostream>
using namespace std;

inline int Max(int x, int y)
{
   return (x > y)? x : y;
}

// 程序的主函数
int main( )
{

   cout << "Max (20,10): " << Max(20,10) << endl;  // 等价于 (20 > 10)? 20 : 10
   cout << "Max (0,200): " << Max(0,200) << endl;  // 等价于 (0 > 200)? 0 : 200
   cout << "Max (100,1010): " << Max(100,1010) << endl;  // 等价于 (100 > 1010)? 100 : 1010
   return 0;
}
```

内联函数通过编译替换，省去了函数调用的过程

* `inline`关键字只是对编译器的**建议**，是否进行内联由编译器决定
* 通常内联函数的定义需要放在头文件`.h`中，如果定义在源文件`.cpp`中，其他源文件无法获取其定义从而导致链接错误
* 在类声明内部定义的成员函数默认是内联函数，无需使用`inline`关键字
* 在类声明外部定义的成员函数，如果希望其成为内联函数，需要定义时加上`inline`关键字

### 虚函数可以是内联函数吗

```c++
class Base {
public:
    inline virtual void who() {
        // ...
    }
};
```

* 虚函数可以是内联函数，内联是可以修饰虚函数，但当虚函数表现多态性的时候不能内联
* 内联是在编译阶段内联，而虚函数的多态性是在运行时期
* 当通过指针或引用进行动态派发调用时：

   ```c++
   base *ptr = new Derived();
   ptr->who(); // 动态绑定
   ```

   `who()`的调用必须在运行时通过虚函数表来确定，编译器无法知道`ptr`指向的是`Base`还是`Derived`对象，因此虚函数机制会生效，内联建议失效

* 当通过对象本身直接调用时：

   ```c++
   Base b;
   b.who();
   ```

   编译器在编译阶段明确知道了调用的是`Base::who()`，不涉及多态，编译器可以考虑内联展开

> 总结
{: .prompt-info}

* 一个被 `virtual` 修饰的函数，当它通过基类指针或引用被调用时，绝对不会被内联，因为它的调用地址是动态决定的。

* 将 `virtual` 函数声明为 `inline` 唯一可能生效的场景是：当通过对象本身（而不是指针或引用）直接调用该函数时。但这又违背了使用虚函数的主要目的——实现多态。

* 因此，在实践中，将一个虚函数声明为内联函数通常是没有意义的，而且可能会误导其他阅读代码的人

## C++中的this指针

在 C++ 中，每一个对象都能通过 `this` 指针来访问自己的地址。`this` 指针是所有成员函数的隐含参数。因此，在成员函数内部，它可以用来指向调用对象。

> 友元函数没有 `this` 指针，因为友元不是类的成员。只有成员函数才有 `this` 指针。
{: .prompt-warn}

```c++
#include <iostream>
 
using namespace std;

class Box
{
   public:
      // 构造函数定义
      Box(double l=2.0, double b=2.0, double h=2.0)
      {
         cout <<"Constructor called." << endl;
         length = l;
         breadth = b;
         height = h;
      }
      double Volume()
      {
         return length * breadth * height;
      }
      int compare(Box box)
      {
         return this->Volume() > box.Volume();
      }
   private:
      double length;     // Length of a box
      double breadth;    // Breadth of a box
      double height;     // Height of a box
};

int main(void)
{
   Box Box1(3.3, 1.2, 1.5);    // Declare box1
   Box Box2(8.5, 6.0, 2.0);    // Declare box2

   if(Box1.compare(Box2))
   {
      cout << "Box2 is smaller than Box1" <<endl;
   }
   else
   {
      cout << "Box2 is equal to or larger than Box1" <<endl;
   }
   return 0;
}
```

## C++指向类的指针

一个指向 C++ 类的指针与指向结构的指针类似，访问指向类的指针的成员，需要使用成员访问运算符 `->`，就像访问指向结构的指针一样。与所有的指针一样，您必须在使用指针之前，对指针进行初始化。

```c++
#include <iostream>
 
using namespace std;

class Box
{
   public:
      // 构造函数定义
      Box(double l=2.0, double b=2.0, double h=2.0)
      {
         cout <<"Constructor called." << endl;
         length = l;
         breadth = b;
         height = h;
      }
      double Volume()
      {
         return length * breadth * height;
      }
   private:
      double length;     // Length of a box
      double breadth;    // Breadth of a box
      double height;     // Height of a box
};

int main(void)
{
   Box Box1(3.3, 1.2, 1.5);    // Declare box1
   Box Box2(8.5, 6.0, 2.0);    // Declare box2
   Box *ptrBox;                // Declare pointer to a class.

   // 保存第一个对象的地址
   ptrBox = &Box1;

   // 现在尝试使用成员访问运算符来访问成员
   cout << "Volume of Box1: " << ptrBox->Volume() << endl;

   // 保存第二个对象的地址
   ptrBox = &Box2;

   // 现在尝试使用成员访问运算符来访问成员
   cout << "Volume of Box2: " << ptrBox->Volume() << endl;
  
   return 0;
}
```


## C++类的静态成员

* 静态成员变量

我们可以使用 `static` 关键字来把类成员定义为静态的。当我们声明类的成员为静态时，这意味着无论创建多少个类的对象，静态成员都只有一个副本。

静态成员在类的所有对象中是共享的。如果不存在其他的初始化语句，在创建第一个对象时，所有的静态数据都会被初始化为零。

* 静态成员函数

如果把函数成员声明为静态的，就可以把函数与类的任何特定对象独立开来。静态成员函数即使在类对象不存在的情况下也能被调用，静态函数只要使用类名加范围解析运算符 `::` 就可以访问。

> 静态成员函数只能访问静态数据成员，不能访问其他静态成员函数和类外部的其他函数。
{: .prompt-warn}

```c++
#include <iostream>
 
using namespace std;

class Box
{
   public:
      static int objectCount;
      // 构造函数定义
      Box(double l=2.0, double b=2.0, double h=2.0)
      {
         cout <<"Constructor called." << endl;
         length = l;
         breadth = b;
         height = h;
         // 每次创建对象时增加 1
         objectCount++;
      }
      double Volume()
      {
         return length * breadth * height;
      }
      static int getCount()
      {
         return objectCount;
      }
   private:
      double length;     // 长度
      double breadth;    // 宽度
      double height;     // 高度
};

// 初始化类 Box 的静态成员
int Box::objectCount = 0;

int main(void)
{
  
   // 在创建对象之前输出对象的总数
   cout << "Inital Stage Count: " << Box::getCount() << endl;

   Box Box1(3.3, 1.2, 1.5);    // 声明 box1
   Box Box2(8.5, 6.0, 2.0);    // 声明 box2

   // 在创建对象之后输出对象的总数
   cout << "Final Stage Count: " << Box::getCount() << endl;

   return 0;
}
```

