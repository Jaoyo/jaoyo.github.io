---
title: C++ 重载运算符和重载函数
date: 2025-09-07 23:00:00 +0800
categories: [编程语言, Cpp]
tags: [C++语言]
description: 
---

C++ 允许在同一作用域中的某个**函数**和**运算符**指定多个定义，分别称为**函数重载**和**运算符重载**。

重载声明是指一个与之前已经在该作用域内声明过的函数或方法具有相同名称的声明，但是它们的参数列表和定义（实现）不相同。

当您调用一个**重载函数**或**重载运算符**时，编译器通过把您所使用的参数类型与定义中的参数类型进行比较，决定选用最合适的定义。选择最合适的重载函数或重载运算符的过程，称为**重载决策**。

## 函数重载

在同一个作用域内，可以声明几个功能类似的同名函数，但是这些同名函数的形式参数（指参数的个数、类型或者顺序）必须不同。

程序示例：

```c++
#include <iostream>
using namespace std;
 
class printData 
{
   public:
      void print(int i) {
        cout << "Printing int: " << i << endl;
      }

      void print(double  f) {
        cout << "Printing float: " << f << endl;
      }

      void print(string c) {
        cout << "Printing character: " << c << endl;
      }
};

int main(void)
{
   printData pd;
 
   // Call print to print integer
   pd.print(5);
   // Call print to print float
   pd.print(500.263);
   // Call print to print character
   pd.print("Hello C++");
 
   return 0;
}
```

## 运算符重载

C++ **运算符重载**`（operator overloading）` 是一种语法糖，允许我们为自定义类型（类或结构体）重新定义已有的运算符行为，使其像内置类型一样自然使用。

* 运算符重载实质上是**函数重载**
* 可以定义为**成员函数**或**全局函数**
* 定义格式

```c++
返回类型 operator运算符(参数列表):
```

### 一元运算符重载

一元运算符只对一个操作数进行操作，可以对以下的运算符进行重载：

* 递增运算符(`++`)和递减运算符(`--`)
* 一元减运算符`-`
* 逻辑非运算符`!`

一元减运算符重载程序示例

```c++
#include <iostream>
using namespace std;
 
class Distance
{
   private:
      int feet;             // 0 到无穷
      int inches;           // 0 到 12
   public:
      // 所需的构造函数
      Distance(){
         feet = 0;
         inches = 0;
      }
      Distance(int f, int i){
         feet = f;
         inches = i;
      }
      // 显示距离的方法
      void displayDistance()
      {
         cout << "F: " << feet << " I:" << inches <<endl;
      }
      // 重载负运算符（ - ）
      Distance operator- ()  
      {
         feet = -feet;
         inches = -inches;
         return Distance(feet, inches);
      }
};
int main()
{
   Distance D1(11, 10), D2(-5, 11);
 
   -D1;                     // 取相反数
   D1.displayDistance();    // 距离 D1

   -D2;                     // 取相反数
   D2.displayDistance();    // 距离 D2

   return 0;
}
```

递增运算符重载程序实例

```c++
#include <iostream>
using namespace std;
 
class Time
{
   private:
      int hours;             // 0 到 23
      int minutes;           // 0 到 59
   public:
      // 所需的构造函数
      Time(){
         hours = 0;
         minutes = 0;
      }
      Time(int h, int m){
         hours = h;
         minutes = m;
      }
      // 显示时间的方法
      void displayTime()
      {
         cout << "H: " << hours << " M:" << minutes <<endl;
      }
      // 重载前缀递增运算符（ ++ ）
      Time operator++ ()  
      {
         ++minutes;          // 对象加 1
         if(minutes >= 60)  
         {
            ++hours;
            minutes -= 60;
         }
         return Time(hours, minutes);
      }
      // 重载后缀递增运算符（ ++ ）
      Time operator++( int )         
      {
         // 保存原始值
         Time T(hours, minutes);
         // 对象加 1
         ++minutes;                    
         if(minutes >= 60)
         {
            ++hours;
            minutes -= 60;
         }
         // 返回旧的原始值
         return T; 
      }
};
int main()
{
   Time T1(11, 59), T2(10,40);
 
   ++T1;                    // T1 加 1
   T1.displayTime();        // 显示 T1
   ++T1;                    // T1 再加 1
   T1.displayTime();        // 显示 T1
 
   T2++;                    // T2 加 1
   T2.displayTime();        // 显示 T2
   T2++;                    // T2 再加 1
   T2.displayTime();        // 显示 T2
   return 0;
}
```

### 二元运算符重载

二元运算符需要两个参数，包括加运算符`+`，减运算符`-`，乘运算符`*`，除运算符`/`

程序示例

```c++
#include <iostream>
using namespace std;
 
class Box
{
   double length;      // 长度
   double breadth;     // 宽度
   double height;      // 高度
public:
 
   double getVolume(void)
   {
      return length * breadth * height;
   }
   void setLength( double len )
   {
       length = len;
   }
 
   void setBreadth( double bre )
   {
       breadth = bre;
   }
 
   void setHeight( double hei )
   {
       height = hei;
   }
   // 重载 + 运算符，用于把两个 Box 对象相加
   Box operator+(const Box& b)
   {
      // this指向对象本身，可以省略
      Box box;
      box.length = this->length + b.length;
      box.breadth = this->breadth + b.breadth;
      box.height = this->height + b.height;
      return box;
   }
};
// 程序的主函数
int main( )
{
   Box Box1;                // 声明 Box1，类型为 Box
   Box Box2;                // 声明 Box2，类型为 Box
   Box Box3;                // 声明 Box3，类型为 Box
   double volume = 0.0;     // 把体积存储在该变量中
 
   // Box1 详述
   Box1.setLength(6.0); 
   Box1.setBreadth(7.0); 
   Box1.setHeight(5.0);
 
   // Box2 详述
   Box2.setLength(12.0); 
   Box2.setBreadth(13.0); 
   Box2.setHeight(10.0);
 
   // Box1 的体积
   volume = Box1.getVolume();
   cout << "Volume of Box1 : " << volume <<endl;
 
   // Box2 的体积
   volume = Box2.getVolume();
   cout << "Volume of Box2 : " << volume <<endl;
 
   // 把两个对象相加，得到 Box3
   Box3 = Box1 + Box2;
 
   // Box3 的体积
   volume = Box3.getVolume();
   cout << "Volume of Box3 : " << volume <<endl;
 
   return 0;
}
```

### 关系运算符重载

C++ 语言支持各种关系运算符（ `<` 、 `>` 、 `<=` 、 `>=` 、 `==` 等等），它们可用于比较 C++ 内置的数据类型。

```c++
#include <iostream>
using namespace std;
 
class Distance
{
   private:
      int feet;             // 0 到无穷
      int inches;           // 0 到 12
   public:
      // 所需的构造函数
      Distance(){
         feet = 0;
         inches = 0;
      }
      Distance(int f, int i){
         feet = f;
         inches = i;
      }
      // 显示距离的方法
      void displayDistance()
      {
         cout << "F: " << feet << " I:" << inches <<endl;
      }
      // 重载负运算符（ - ）
      Distance operator- ()  
      {
         feet = -feet;
         inches = -inches;
         return Distance(feet, inches);
      }
      // 重载小于运算符（ < ）
      bool operator <(const Distance& d)
      {
         if(feet < d.feet)
         {
            return true;
         }
         if(feet == d.feet && inches < d.inches)
         {
            return true;
         }
         return false;
      }
};
int main()
{
   Distance D1(11, 10), D2(5, 11);
 
   if( D1 < D2 )
   {
      cout << "D1 is less than D2 " << endl;
   }
   else
   {
      cout << "D2 is less than D1 " << endl;
   }
   return 0;
}
```

### 输入输出运算符重载

C++ 能够使用流提取运算符 `>>` 和流插入运算符 `<<` 来输入和输出内置的数据类型。您可以重载流提取运算符和流插入运算符来操作对象等用户自定义的数据类型。

> 我们需要把运算符重载函数声明为类的友元函数，这样我们就能不用创建对象而直接调用函数。
{: .prompt-warn}

* 重载输入运算符`>>`

```c++
istream & operator>>(istream &in, complex &A){
    in >> A.m_real >> A.m_imag;
    return in;
}
```

上面的代码，以全局函数的形式重载`>>`，使它可以读取两个输入到`A`对象的成员中。

`istream` 表示输入流，`cin` 是 `istream` 类的对象，只不过这个对象是在标准库中定义的。之所以返回 `istream` 类对象的引用，是为了能够连续读取复数，让代码书写更加漂亮。

如果运算符重载函数中用到了私有成员，则必须在类中将该函数声明为友元函数。

* 重载输出运算符`<<`

```c++
ostream & operator<<(ostream &out, complex &A){
    out << A.m_real <<" + "<< A.m_imag <<" i ";
    return out;
}
```

同样的方式可以重载输出运算符，可以将对象A的两个成员进行输出。

`ostream` 表示输出流，`cout` 是 `ostream` 类的对象。由于采用了引用的方式进行参数传递，并且也返回了对象的引用，所以重载后的运算符可以实现连续输出。

综合程序示例：

```c++
#include <iostream>

class complex {
    private:
        double m_real;
        double m_imag;
    public:
        complex() {
            m_real=0;m_imag=0;
        }
        
        complex(double a, double b) {
            m_real=a;m_imag=b;
        }
        
        void printf() {
            std::cout << "m_real is " << m_real << " m_imag is " << m_imag << std::endl;
        }
        
        friend std::istream & operator >> (std::istream &in, complex &a);
        friend std::ostream & operator << (std::ostream &out, complex &a);
};

std::istream & operator >> (std::istream & in, complex &a) {
    in >> a.m_real >> a.m_imag;
    return in;
}

std::ostream & operator << (std::ostream &out, complex &a) {
    out << "m_real = " << a.m_real << " m_imag = " << a.m_imag << std::endl;
    return out;
}

int main()
{  
    complex x1(3,5.4), x2(2.45,6.6), x3;

    std::cin >> x3;

    x1.printf();
    std::cout << x2;
    std::cout << x3;

    return 0;
}
```

### 赋值运算符重载

程序示例：

```c++
#include <iostream>
using namespace std;
 
class Distance
{
   private:
      int feet;             // 0 到无穷
      int inches;           // 0 到 12
   public:
      // 所需的构造函数
      Distance(){
         feet = 0;
         inches = 0;
      }
      Distance(int f, int i){
         feet = f;
         inches = i;
      }
      void operator=(const Distance &D )
      { 
         feet = D.feet;
         inches = D.inches;
      }
      // 显示距离的方法
      void displayDistance()
      {
         cout << "F: " << feet <<  " I:" <<  inches << endl;
      }
      
};
int main()
{
   Distance D1(11, 10), D2(5, 11);

   cout << "First Distance : "; 
   D1.displayDistance();
   cout << "Second Distance :"; 
   D2.displayDistance();

   // 使用赋值运算符
   D1 = D2;
   cout << "First Distance :"; 
   D1.displayDistance();

   return 0;
}
```

### 函数调用运算符 `()` 重载

```c++
#include <iostream>
using namespace std;
 
class Distance
{
   private:
      int feet;             // 0 到无穷
      int inches;           // 0 到 12
   public:
      // 所需的构造函数
      Distance(){
         feet = 0;
         inches = 0;
      }
      Distance(int f, int i){
         feet = f;
         inches = i;
      }
      // 重载函数调用运算符
      Distance operator()(int a, int b, int c)
      {
         Distance D;
         // 进行随机计算
         D.feet = a + c + 10;
         D.inches = b + c + 100 ;
         return D;
      }
      // 显示距离的方法
      void displayDistance()
      {
         cout << "F: " << feet <<  " I:" <<  inches << endl;
      }
      
};
int main()
{
   Distance D1(11, 10), D2;

   cout << "First Distance : "; 
   D1.displayDistance();

   D2 = D1(10, 10, 10); // invoke operator()
   cout << "Second Distance :"; 
   D2.displayDistance();

   return 0;
}
```

### 下标运算符 `[]` 重载

```c++
#include <iostream>
using namespace std;
const int SIZE = 10;

class safearay
{
   private:
      int arr[SIZE];
   public:
      safearay() 
      {
         register int i;
         for(i = 0; i < SIZE; i++)
         {
           arr[i] = i;
         }
      }
      int& operator[](int i)
      {
          if( i > SIZE )
          {
              cout << "索引超过最大值" <<endl; 
              // 返回第一个元素
              return arr[0];
          }
          return arr[i];
      }
};
int main()
{
   safearay A;

   cout << "A[2] 的值为 : " << A[2] <<endl;
   cout << "A[5] 的值为 : " << A[5]<<endl;
   cout << "A[12] 的值为 : " << A[12]<<endl;

   return 0;
}
```

### 类成员访问运算符 `->` 重载

类成员访问运算符（ `->`）可以被重载，但它较为麻烦。它被定义用于为一个类赋予"指针"行为。运算符 `->` 必须是一个成员函数。如果使用了 `->` 运算符，返回类型必须是指针或者是类的对象。

运算符 `->` 通常与指针引用运算符 `*` 结合使用，用于实现"智能指针"的功能。这些指针是行为与正常指针相似的对象，唯一不同的是，当您通过指针访问对象时，它们会执行其他的任务。比如，当指针销毁时，或者当指针指向另一个对象时，会自动删除对象。

```c++
#include <iostream>
#include <vector>
using namespace std;

// 假设一个实际的类
class Obj {
   static int i, j;
public:
   void f() const { cout << i++ << endl; }
   void g() const { cout << j++ << endl; }
};

// 静态成员定义
int Obj::i = 10;
int Obj::j = 12;

// 为上面的类实现一个容器
class ObjContainer {
   vector<Obj*> a;
public:
   void add(Obj* obj)
   { 
      a.push_back(obj);  // 调用向量的标准方法
   }
   friend class SmartPointer;
};

// 实现智能指针，用于访问类 Obj 的成员
class SmartPointer {
   ObjContainer oc;
   int index;
public:
   SmartPointer(ObjContainer& objc)
   { 
       oc = objc;
       index = 0;
   }
   // 返回值表示列表结束
   bool operator++() // 前缀版本
   { 
     if(index >= oc.a.size()) return false;
     if(oc.a[++index] == 0) return false;
     return true;
   }
   bool operator++(int) // 后缀版本
   { 
      return operator++();
   }
   // 重载运算符 ->
   Obj* operator->() const 
   {
     if(!oc.a[index])
     {
        cout << "Zero value";
        return (Obj*)0;
     }
     return oc.a[index];
   }
};

int main() {
   const int sz = 10;
   Obj o[sz];
   ObjContainer oc;
   for(int i = 0; i < sz; i++)
   {
       oc.add(&o[i]);
   }
   SmartPointer sp(oc); // 创建一个迭代器
   do {
      sp->f(); // 智能指针调用
      sp->g();
   } while(sp++);
   return 0;
}
```

