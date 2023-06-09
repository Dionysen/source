---
title: C Plus Plus - Enhancement
date: 2022-05-25 17:34:01
categories: [Programming, Language]
tags: [cpp]
comment: true
sticky: 998
---

本阶段主要针对cpp***泛型编程***和***STL***技术做详细讲解，探讨cpp更深层的使用

<!-- more -->

## 一、模板

### 1.1 模板的概念

模板就是建立**通用的模具**，大大**提高复用性**
模板的特点：

- 模板不可以直接使用，它只是一个框架
- 模板的通用并不是万能的

### 1.2 函数模板

- cpp另一种编程思想称为 ***泛型编程*** ，主要利用的技术就是模板

- cpp提供两种模板机制:**函数模板**和**类模板**

#### 1.2.1 函数模板语法

函数模板作用：
建立一个通用函数，其函数返回值类型和形参类型可以不具体制定，用一个**虚拟的类型**来代表。
**语法：**

```C++
template<typename T>
函数声明或定义
```

**解释：**
template  ---  声明创建模板
typename  --- 表面其后面的符号是一种数据类型，可以用class代替
T    ---   通用的数据类型，名称可以替换，通常为大写字母
**示例：**

```C++
//交换整型函数
void swapInt(int& a, int& b) {
 int temp = a;
 a = b;
 b = temp;
}
//交换浮点型函数
void swapDouble(double& a, double& b) {
 double temp = a;
 a = b;
 b = temp;
}
//利用模板提供通用的交换函数
template<typename T>
void mySwap(T& a, T& b)
{
 T temp = a;
 a = b;
 b = temp;
}
void test01()
{
 int a = 10;
 int b = 20;
 //swapInt(a, b);
 //利用模板实现交换
 //1、自动类型推导
 mySwap(a, b);
 //2、显示指定类型
 mySwap<int>(a, b);
 cout << "a = " << a << endl;
 cout << "b = " << b << endl;
}
int main() {
 test01();
 return 0;
}
```

- 函数模板利用关键字 template

- 使用函数模板有两种方式：自动类型推导、显示指定类型

- 模板的目的是为了提高复用性，将类型参数化

#### 1.2.2 函数模板注意事项

注意事项：

- 自动类型推导，必须推导出一致的数据类型T,才可以使用
- 模板必须要确定出T的数据类型，才可以使用
  **示例：**

```C++
//利用模板提供通用的交换函数
template<class T>
void mySwap(T& a, T& b)
{
 T temp = a;
 a = b;
 b = temp;
}
// 1、自动类型推导，必须推导出一致的数据类型T,才可以使用
void test01()
{
 int a = 10;
 int b = 20;
 char c = 'c';
 mySwap(a, b); // 正确，可以推导出一致的T
 //mySwap(a, c); // 错误，推导不出一致的T类型
}
// 2、模板必须要确定出T的数据类型，才可以使用
template<class T>
void func()
{
 cout << "func 调用" << endl;
}
void test02()
{
 //func(); //错误，模板不能独立使用，必须确定出T的类型
 func<int>(); //利用显示指定类型的方式，给T一个类型，才可以使用该模板
}
int main() {
 test01();
 test02();
 return 0;
}
```

- 使用模板时必须确定出通用数据类型T，并且能够推导出一致的类型

#### 1.2.3 函数模板案例

案例描述：

- 利用函数模板封装一个排序的函数，可以对**不同数据类型数组**进行排序
- 排序规则从大到小，排序算法为**选择排序**
- 分别利用**char数组**和**int数组**进行测试
  示例：

```C++
//交换的函数模板
template<typename T>
void mySwap(T &a, T&b)
{
 T temp = a;
 a = b;
 b = temp;
}
template<class T> // 也可以替换成typename
//利用选择排序，进行对数组从大到小的排序
void mySort(T arr[], int len)
{
 for (int i = 0; i < len; i++)
 {
  int max = i; //最大数的下标
  for (int j = i + 1; j < len; j++)
  {
   if (arr[max] < arr[j])
   {
    max = j;
   }
  }
  if (max != i) //如果最大数的下标不是i，交换两者
  {
   mySwap(arr[max], arr[i]);
  }
 }
}
template<typename T>
void printArray(T arr[], int len) {
 for (int i = 0; i < len; i++) {
  cout << arr[i] << " ";
 }
 cout << endl;
}
void test01()
{
 //测试char数组
 char charArr[] = "bdcfeagh";
 int num = sizeof(charArr) / sizeof(char);
 mySort(charArr, num);
 printArray(charArr, num);
}
void test02()
{
 //测试int数组
 int intArr[] = { 7, 5, 8, 1, 3, 9, 2, 4, 6 };
 int num = sizeof(intArr) / sizeof(int);
 mySort(intArr, num);
 printArray(intArr, num);
}
int main() {
 test01();
 test02();
 return 0;
}
```

模板可以提高代码复用，需要熟练掌握

#### 1.2.4 普通函数与函数模板的区别

**普通函数与函数模板区别：**

- 普通函数调用时可以发生自动类型转换（隐式类型转换）
- 函数模板调用时，如果利用自动类型推导，不会发生隐式类型转换
- 如果利用显示指定类型的方式，可以发生隐式类型转换
  **示例：**

```C++
//普通函数
int myAdd01(int a, int b)
{
 return a + b;
}
//函数模板
template<class T>
T myAdd02(T a, T b)
{
 return a + b;
}
//使用函数模板时，如果用自动类型推导，不会发生自动类型转换,即隐式类型转换
void test01()
{
 int a = 10;
 int b = 20;
 char c = 'c';
 cout << myAdd01(a, c) << endl; //正确，将char类型的'c'隐式转换为int类型  'c' 对应 ASCII码 99
 //myAdd02(a, c); // 报错，使用自动类型推导时，不会发生隐式类型转换
 myAdd02<int>(a, c); //正确，如果用显示指定类型，可以发生隐式类型转换
}
int main() {
 test01();
 system("pause");
 return 0;
}
```

建议使用显示指定类型的方式，调用函数模板，因为可以自己确定通用类型T

#### 1.2.5 普通函数与函数模板的调用规则

调用规则如下：

1. 如果函数模板和普通函数都可以实现，优先调用普通函数
2. 可以通过空模板参数列表来强制调用函数模板
3. 函数模板也可以发生重载
4. 如果函数模板可以产生更好的匹配,优先调用函数模板
   **示例：**

```C++
//普通函数与函数模板调用规则
void myPrint(int a, int b)
{
 cout << "调用的普通函数" << endl;
}
template<typename T>
void myPrint(T a, T b)
{
 cout << "调用的模板" << endl;
}
template<typename T>
void myPrint(T a, T b, T c)
{
 cout << "调用重载的模板" << endl;
}
void test01()
{
 //1、如果函数模板和普通函数都可以实现，优先调用普通函数
 // 注意 如果告诉编译器  普通函数是有的，但只是声明没有实现，或者不在当前文件内实现，就会报错找不到
 int a = 10;
 int b = 20;
 myPrint(a, b); //调用普通函数
 //2、可以通过空模板参数列表来强制调用函数模板
 myPrint<>(a, b); //调用函数模板
 //3、函数模板也可以发生重载
 int c = 30;
 myPrint(a, b, c); //调用重载的函数模板
 //4、 如果函数模板可以产生更好的匹配,优先调用函数模板
 char c1 = 'a';
 char c2 = 'b';
 myPrint(c1, c2); //调用函数模板
}
int main() {
 test01();
 return 0;
}
```

既然提供了函数模板，最好就不要提供普通函数，否则容易出现二义性

#### 1.2.6 模板的局限性

**局限性：**

- 模板的通用性并不是万能的
  **例如：**

```C++
 template<class T>
 void f(T a, T b)
 {
     a = b;
    }
```

在上述代码中提供的赋值操作，如果传入的a和b是一个数组，就无法实现了
再例如：

```C++
 template<class T>
 void f(T a, T b)
 {
     if(a > b) { ... }
    }
```

在上述代码中，如果T的数据类型传入的是像Person这样的自定义数据类型，也无法正常运行
因此cpp为了解决这种问题，提供模板的重载，可以为这些**特定的类型**提供**具体化的模板**
**示例：**

```C++
#include<iostream>
using namespace std;
#include <string>
class Person
{
public:
 Person(string name, int age)
 {
  this->m_Name = name;
  this->m_Age = age;
 }
 string m_Name;
 int m_Age;
};
//普通函数模板
template<class T>
bool myCompare(T& a, T& b)
{
 if (a *** b)
 {
  return true;
 }
 else
 {
  return false;
 }
}
//具体化，显示具体化的原型和定意思以template<>开头，并通过名称来指出类型
//具体化优先于常规模板
template<> bool myCompare(Person &p1, Person &p2)
{
 if ( p1.m_Name  *** p2.m_Name && p1.m_Age *** p2.m_Age)
 {
  return true;
 }
 else
 {
  return false;
 }
}
void test01()
{
 int a = 10;
 int b = 20;
 //内置数据类型可以直接使用通用的函数模板
 bool ret = myCompare(a, b);
 if (ret)
 {
  cout << "a *** b " << endl;
 }
 else
 {
  cout << "a != b " << endl;
 }
}
void test02()
{
 Person p1("Tom", 10);
 Person p2("Tom", 10);
 //自定义数据类型，不会调用普通的函数模板
 //可以创建具体化的Person数据类型的模板，用于特殊处理这个类型
 bool ret = myCompare(p1, p2);
 if (ret)
 {
  cout << "p1 *** p2 " << endl;
 }
 else
 {
  cout << "p1 != p2 " << endl;
 }
}
int main() {
 test01();
 test02();
 return 0;
}
```

- 利用具体化的模板，可以解决自定义类型的通用化

- 学习模板并不是为了写模板，而是在STL能够运用系统提供的模板

### 1.3 类模板

#### 1.3.1 类模板语法

类模板作用：

- 建立一个通用类，类中的成员 数据类型可以不具体制定，用一个**虚拟的类型**来代表。
  **语法：**

```c++
template<typename T>
类
```

**解释：**
template  ---  声明创建模板
typename  --- 表面其后面的符号是一种数据类型，可以用class代替
T    ---   通用的数据类型，名称可以替换，通常为大写字母
**示例：**

```C++
#include <string>
//类模板
template<class NameType, class AgeType>
class Person
{
public:
 Person(NameType name, AgeType age)
 {
  this->mName = name;
  this->mAge = age;
 }
 void showPerson()
 {
  cout << "name: " << this->mName << " age: " << this->mAge << endl;
 }
public:
 NameType mName;
 AgeType mAge;
};
void test01()
{
 // 指定NameType 为string类型，AgeType 为 int类型
 Person<string, int>P1("孙悟空", 999);
 P1.showPerson();
}
int main() {
 test01();
 return 0;
}
```

类模板和函数模板语法相似，在声明模板template后面加类，此类称为类模板

#### 1.3.2 类模板与函数模板区别

类模板与函数模板区别主要有两点：

1. 类模板没有自动类型推导的使用方式
2. 类模板在模板参数列表中可以有默认参数
   **示例：**

```C++
#include <string>
//类模板
template<class NameType, class AgeType = int>
class Person
{
public:
 Person(NameType name, AgeType age)
 {
  this->mName = name;
  this->mAge = age;
 }
 void showPerson()
 {
  cout << "name: " << this->mName << " age: " << this->mAge << endl;
 }
public:
 NameType mName;
 AgeType mAge;
};
//1、类模板没有自动类型推导的使用方式
void test01()
{
 // Person p("孙悟空", 1000); // 错误 类模板使用时候，不可以用自动类型推导
 Person <string ,int>p("孙悟空", 1000); //必须使用显示指定类型的方式，使用类模板
 p.showPerson();
}
//2、类模板在模板参数列表中可以有默认参数
void test02()
{
 Person <string> p("猪八戒", 999); //类模板中的模板参数列表 可以指定默认参数
 p.showPerson();
}
int main() {
 test01();
 test02();
 return 0;
}
```

- 类模板使用只能用显示指定类型方式

- 类模板中的模板参数列表可以有默认参数

#### 1.3.3 类模板中成员函数创建时机

类模板中成员函数和普通类中成员函数创建时机是有区别的：

- 普通类中的成员函数一开始就可以创建
- 类模板中的成员函数在调用时才创建
  **示例：**

```C++
class Person1
{
public:
 void showPerson1()
 {
  cout << "Person1 show" << endl;
 }
};
class Person2
{
public:
 void showPerson2()
 {
  cout << "Person2 show" << endl;
 }
};
template<class T>
class MyClass
{
public:
 T obj;
 //类模板中的成员函数，并不是一开始就创建的，而是在模板调用时再生成
 void fun1() { obj.showPerson1(); }
 void fun2() { obj.showPerson2(); }
};
void test01()
{
 MyClass<Person1> m;
 m.fun1();
 //m.fun2();//编译会出错，说明函数调用才会去创建成员函数
}
int main() {
 test01();
 return 0;
}
```

类模板中的成员函数并不是一开始就创建的，在调用时才去创建

#### 1.3.4 类模板对象做函数参数

学习目标：

- 类模板实例化出的对象，向函数传参的方式
  一共有三种传入方式：

1. 指定传入的类型   --- 直接显示对象的数据类型
2. 参数模板化           --- 将对象中的参数变为模板进行传递
3. 整个类模板化       --- 将这个对象类型 模板化进行传递
   **示例：**

```C++
#include <string>
//类模板
template<class NameType, class AgeType = int>
class Person
{
public:
 Person(NameType name, AgeType age)
 {
  this->mName = name;
  this->mAge = age;
 }
 void showPerson()
 {
  cout << "name: " << this->mName << " age: " << this->mAge << endl;
 }
public:
 NameType mName;
 AgeType mAge;
};
//1、指定传入的类型
void printPerson1(Person<string, int> &p)
{
 p.showPerson();
}
void test01()
{
 Person <string, int >p("孙悟空", 100);
 printPerson1(p);
}
//2、参数模板化
template <class T1, class T2>
void printPerson2(Person<T1, T2>&p)
{
 p.showPerson();
 cout << "T1的类型为： " << typeid(T1).name() << endl;
 cout << "T2的类型为： " << typeid(T2).name() << endl;
}
void test02()
{
 Person <string, int >p("猪八戒", 90);
 printPerson2(p);
}
//3、整个类模板化
template<class T>
void printPerson3(T & p)
{
 cout << "T的类型为： " << typeid(T).name() << endl;
 p.showPerson();
}
void test03()
{
 Person <string, int >p("唐僧", 30);
 printPerson3(p);
}
int main() {
 test01();
 test02();
 test03();
 return 0;
}
```

- 通过类模板创建的对象，可以有三种方式向函数中进行传参

- 使用比较广泛是第一种：指定传入的类型

#### 1.3.5 类模板与继承

当类模板碰到继承时，需要注意一下几点：

- 当子类继承的父类是一个类模板时，子类在声明的时候，要指定出父类中T的类型
- 如果不指定，编译器无法给子类分配内存
- 如果想灵活指定出父类中T的类型，子类也需变为类模板
  **示例：**

```C++
template<class T>
class Base
{
 T m;
};
//class Son:public Base  //错误，cpp编译需要给子类分配内存，必须知道父类中T的类型才可以向下继承
class Son :public Base<int> //必须指定一个类型
{
};
void test01()
{
 Son c;
}
//类模板继承类模板 ,可以用T2指定父类中的T类型
template<class T1, class T2>
class Son2 :public Base<T2>
{
public:
 Son2()
 {
  cout << typeid(T1).name() << endl;
  cout << typeid(T2).name() << endl;
 }
};
void test02()
{
 Son2<int, char> child1;
}
int main() {
 test01();
 test02();
 return 0;
}
```

如果父类是类模板，子类需要指定出父类中T的数据类型

#### 1.3.6 类模板成员函数类外实现

学习目标：能够掌握类模板中的成员函数类外实现
**示例：**

```C++
#include <string>
//类模板中成员函数类外实现
template<class T1, class T2>
class Person {
public:
 //成员函数类内声明
 Person(T1 name, T2 age);
 void showPerson();
public:
 T1 m_Name;
 T2 m_Age;
};
//构造函数 类外实现
template<class T1, class T2>
Person<T1, T2>::Person(T1 name, T2 age) {
 this->m_Name = name;
 this->m_Age = age;
}
//成员函数 类外实现
template<class T1, class T2>
void Person<T1, T2>::showPerson() {
 cout << "姓名: " << this->m_Name << " 年龄:" << this->m_Age << endl;
}
void test01()
{
 Person<string, int> p("Tom", 20);
 p.showPerson();
}
int main() {
 test01();
 return 0;
}
```

类模板中成员函数类外实现时，需要加上模板参数列表

#### 1.3.7 类模板分文件编写

学习目标：

- 掌握类模板成员函数分文件编写产生的问题以及解决方式
  问题：
- 类模板中成员函数创建时机是在调用阶段，导致分文件编写时链接不到
  解决：
- 解决方式1：直接包含.cpp源文件
- 解决方式2：将声明和实现写到同一个文件中，并更改后缀名为.hpp，hpp是约定的名称，并不是强制
  **示例：**
  person.hpp中代码：

```C++
#pragma once
#include <iostream>
using namespace std;
#include <string>
template<class T1, class T2>
class Person {
public:
 Person(T1 name, T2 age);
 void showPerson();
public:
 T1 m_Name;
 T2 m_Age;
};
//构造函数 类外实现
template<class T1, class T2>
Person<T1, T2>::Person(T1 name, T2 age) {
 this->m_Name = name;
 this->m_Age = age;
}
//成员函数 类外实现
template<class T1, class T2>
void Person<T1, T2>::showPerson() {
 cout << "姓名: " << this->m_Name << " 年龄:" << this->m_Age << endl;
}
```

类模板分文件编写.cpp中代码

```C++
#include<iostream>
using namespace std;
//#include "person.h"
#include "person.cpp" //解决方式1，包含cpp源文件
//解决方式2，将声明和实现写到一起，文件后缀名改为.hpp
#include "person.hpp"
void test01()
{
 Person<string, int> p("Tom", 10);
 p.showPerson();
}
int main() {
 test01();
 return 0;
}
```

主流的解决方式是第二种，将类模板成员函数写到一起，并将后缀名改为.hpp

#### 1.3.8 类模板与友元

学习目标：

- 掌握类模板配合友元函数的类内和类外实现
  全局函数类内实现 - 直接在类内声明友元即可
  全局函数类外实现 - 需要提前让编译器知道全局函数的存在
  **示例：**

```C++
#include <string>
//2、全局函数配合友元  类外实现 - 先做函数模板声明，下方在做函数模板定义，在做友元
template<class T1, class T2> class Person;
//如果声明了函数模板，可以将实现写到后面，否则需要将实现体写到类的前面让编译器提前看到
//template<class T1, class T2> void printPerson2(Person<T1, T2> & p);
template<class T1, class T2>
void printPerson2(Person<T1, T2> & p)
{
 cout << "类外实现 ---- 姓名： " << p.m_Name << " 年龄：" << p.m_Age << endl;
}
template<class T1, class T2>
class Person
{
 //1、全局函数配合友元   类内实现
 friend void printPerson(Person<T1, T2> & p)
 {
  cout << "姓名： " << p.m_Name << " 年龄：" << p.m_Age << endl;
 }
 //全局函数配合友元  类外实现
 friend void printPerson2<>(Person<T1, T2> & p);
public:
 Person(T1 name, T2 age)
 {
  this->m_Name = name;
  this->m_Age = age;
 }
private:
 T1 m_Name;
 T2 m_Age;
};
//1、全局函数在类内实现
void test01()
{
 Person <string, int >p("Tom", 20);
 printPerson(p);
}
//2、全局函数在类外实现
void test02()
{
 Person <string, int >p("Jerry", 30);
 printPerson2(p);
}
int main() {
 //test01();
 test02();
 return 0;
}
```

建议全局函数做类内实现，用法简单，而且编译器可以直接识别

#### 1.3.9 类模板案例

案例描述:  实现一个通用的数组类，要求如下：

- 可以对内置数据类型以及自定义数据类型的数据进行存储
- 将数组中的数据存储到堆区
- 构造函数中可以传入数组的容量
- 提供对应的拷贝构造函数以及operator=防止浅拷贝问题
- 提供尾插法和尾删法对数组中的数据进行增加和删除
- 可以通过下标的方式访问数组中的元素
- 可以获取数组中当前元素个数和数组的容量
  **示例：**
  myArray.hpp中代码

```C++
#pragma once
#include <iostream>
using namespace std;
template<class T>
class MyArray
{
public:
 //构造函数
 MyArray(int capacity)
 {
  this->m_Capacity = capacity;
  this->m_Size = 0;
  pAddress = new T[this->m_Capacity];
 }
 //拷贝构造
 MyArray(const MyArray & arr)
 {
  this->m_Capacity = arr.m_Capacity;
  this->m_Size = arr.m_Size;
  this->pAddress = new T[this->m_Capacity];
  for (int i = 0; i < this->m_Size; i++)
  {
   //如果T为对象，而且还包含指针，必须需要重载 = 操作符，因为这个等号不是 构造 而是赋值，
   // 普通类型可以直接= 但是指针类型需要深拷贝
   this->pAddress[i] = arr.pAddress[i];
  }
 }
 //重载= 操作符  防止浅拷贝问题
 MyArray& operator=(const MyArray& myarray) {
  if (this->pAddress != NULL) {
   delete[] this->pAddress;
   this->m_Capacity = 0;
   this->m_Size = 0;
  }
  this->m_Capacity = myarray.m_Capacity;
  this->m_Size = myarray.m_Size;
  this->pAddress = new T[this->m_Capacity];
  for (int i = 0; i < this->m_Size; i++) {
   this->pAddress[i] = myarray[i];
  }
  return *this;
 }
 //重载[] 操作符  arr[0]
 T& operator [](int index)
 {
  return this->pAddress[index]; //不考虑越界，用户自己去处理
 }
 //尾插法
 void Push_back(const T & val)
 {
  if (this->m_Capacity *** this->m_Size)
  {
   return;
  }
  this->pAddress[this->m_Size] = val;
  this->m_Size++;
 }
 //尾删法
 void Pop_back()
 {
  if (this->m_Size *** 0)
  {
   return;
  }
  this->m_Size--;
 }
 //获取数组容量
 int getCapacity()
 {
  return this->m_Capacity;
 }
 //获取数组大小
 int getSize()
 {
  return this->m_Size;
 }
 //析构
 ~MyArray()
 {
  if (this->pAddress != NULL)
  {
   delete[] this->pAddress;
   this->pAddress = NULL;
   this->m_Capacity = 0;
   this->m_Size = 0;
  }
 }
private:
 T * pAddress;  //指向一个堆空间，这个空间存储真正的数据
 int m_Capacity; //容量
 int m_Size;   // 大小
};
```

类模板案例—数组类封装.cpp中

```C++
#include "myArray.hpp"
#include <string>
void printIntArray(MyArray<int>& arr) {
 for (int i = 0; i < arr.getSize(); i++) {
  cout << arr[i] << " ";
 }
 cout << endl;
}
//测试内置数据类型
void test01()
{
 MyArray<int> array1(10);
 for (int i = 0; i < 10; i++)
 {
  array1.Push_back(i);
 }
 cout << "array1打印输出：" << endl;
 printIntArray(array1);
 cout << "array1的大小：" << array1.getSize() << endl;
 cout << "array1的容量：" << array1.getCapacity() << endl;
 cout << "--------------------------" << endl;
 MyArray<int> array2(array1);
 array2.Pop_back();
 cout << "array2打印输出：" << endl;
 printIntArray(array2);
 cout << "array2的大小：" << array2.getSize() << endl;
 cout << "array2的容量：" << array2.getCapacity() << endl;
}
//测试自定义数据类型
class Person {
public:
 Person() {}
  Person(string name, int age) {
  this->m_Name = name;
  this->m_Age = age;
 }
public:
 string m_Name;
 int m_Age;
};
void printPersonArray(MyArray<Person>& personArr)
{
 for (int i = 0; i < personArr.getSize(); i++) {
  cout << "姓名：" << personArr[i].m_Name << " 年龄： " << personArr[i].m_Age << endl;
 }
}
void test02()
{
 //创建数组
 MyArray<Person> pArray(10);
 Person p1("孙悟空", 30);
 Person p2("韩信", 20);
 Person p3("妲己", 18);
 Person p4("王昭君", 15);
 Person p5("赵云", 24);
 //插入数据
 pArray.Push_back(p1);
 pArray.Push_back(p2);
 pArray.Push_back(p3);
 pArray.Push_back(p4);
 pArray.Push_back(p5);
 printPersonArray(pArray);
 cout << "pArray的大小：" << pArray.getSize() << endl;
 cout << "pArray的容量：" << pArray.getCapacity() << endl;
}
int main() {
 //test01();
 test02();
 return 0;
}
```

能够利用所学知识点实现通用的数组

## 二、STL

### 初识容器

```c++
vector<int> v;  // 创建容器
v.push_back(10);//向容器中放数据
v.begin() //返回第一个元素
v.end()  //返回最后一个元素
/* 遍历 */
//1.
for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
 cout << *it << endl;
}
//2.
for (auto it = v.begin(); it != v.end(); it ++){
    cout << *it << endl;
}
//3.
#include <algorithm>
void MyPrint(int val){
 cout << val << endl;
}
for_each(v.begin(), v.end(), MyPrint);
```

1. 存放自定义数据的时候，初始化容器数据是要使用数据类型的构造函数。访问时使用`(*it).member`

2. 存放对象指针：`vector<class *> v` ，放入数据时要放入地址：`v.push_back(&Object)`

3. 容器嵌套容器：`vector<vector<int>> v`，遍历时要用嵌套`for`循环。
   迭代器种类：

   | 种类                                                 | 功能                                                     | 支持运算                                                |
   | ---------------------------------------------------- | -------------------------------------------------------- | ------------------------------------------------------- |
   | 输入迭代器                                           | 对数据的只读访问                                         | 只读，支持`++`、`***`、`!=`                             |
   | 输出迭代器                                           | 对数据的只写访问                                         | 只写，支持`++`                                          |
   | 前向迭代器                                           | 读写操作，并能向前推进迭代器                             | 读写，支持`++`、`***`、`!=`                             |
   | 双向迭代器                                           | 读写操作，并能向前和向后操作                             | 读写，支持`++`、`--`，                                  |
   | 随机访问迭代器                                       | 读写操作，可以以跳跃的方式访问任意数据，功能最强的迭代器 | 读写，支持`++`、`--`、`[n]`、`-n`、`<`、`<=`、`>`、`>=` |
   | 常用的容器中迭代器种类为双向迭代器，和随机访问迭代器 |                                                          |                                                         |

### string

本质上是一个类
`char *`是一个指针，而`string`是一个类，内部封装了`char *`，来管理这个字符串，所以`string`是一个`char *`型的容器

| 方法    | 作用 |
| ------- | ---- |
| find    | 查找 |
| copy    | 拷贝 |
| delete  | 删除 |
| replace | 替换 |
| insert  | 插入 |

#### 构造函数

```c++
string();//创建一个空的字符串 例如: string str;
string(const char* s);//使用字符串s初始化
string(const string& str);//使用一个string对象初始化另一个string对象
string(int n, char c);//使用n个字符c初始化
```

#### 赋值

```c++
string& operator=(const char* s);//char*类型字符串 赋值给当前的字符串
string& operator=(const string &s);//把字符串s赋给当前的字符串
string& operator=(char c);//字符赋值给当前的字符串
string& assign(const char *s);//把字符串s赋给当前的字符串
string& assign(const char *s, int n);//把字符串s的前n个字符赋给当前的字符串
string& assign(const string &s);//把字符串s赋给当前字符串
string& assign(int n, char c);//用n个字符c赋给当前字符串
```

带operator=的是等号运算符重载，即可以用另一个数据和`=`给其赋值，assingn则是成员函数，需要调用函数才能赋值

#### 字符串拼接

```c++
string& operator+=(const char* str);//重载+=操作符
string& operator+=(const char c);//重载+=操作符
string& operator+=(const string& str);//重载+=操作符
string& append(const char *s);//把字符串s连接到当前字符串结尾
string& append(const char *s, int n);//把字符串s的前n个字符连接到当前字符串结尾
string& append(const string &s);//同operator+=(const string& str)
string& append(const string &s, int pos, int n); //字符串s中从pos开始的n个字符连接到字符串结尾
```

#### 查找替换

```c++
int find(const string& str, int pos = 0) const;//查找str第一次出现位置,从pos开始查找
int find(const char* s, int pos = 0) const;//查找s第一次出现位置,从pos开始查找
int find(const char* s, int pos, int n) const;//从pos位置查找s的前n个字符第一次位置
int find(const char c, int pos = 0) const;//查找字符c第一次出现位置
int rfind(const string& str, int pos = npos) const;//查找str最后一次位置,从pos开始查找
int rfind(const char* s, int pos = npos) const;//查找s最后一次出现位置,从pos开始查找
int rfind(const char* s, int pos, int n) const;//从pos查找s的前n个字符最后一次位置
int rfind(const char c, int pos = 0) const;//查找字符c最后一次出现位置
string& replace(int pos, int n, const string& str);//替换从pos开始n个字符为字符串str
string& replace(int pos, int n,const char* s);//替换从pos开始的n个字符为字符串s
```

ﬁnd查找是从左往后，rﬁnd从右往左
ﬁnd找到字符串后返回查找的第一个字符位置，找不到返回-1
replace在替换时，要指定从哪个位置起，多少个字符，替换成什么样的字符串

#### 字符串的比较

```c++
int compare(const string &s) const;//与字符串s比较
int compare(const char *s) const;//与字符串s比较
//比较ASCII码
//= 返回0
//> 返回1
//< 返回-1
```

#### 单个字符存取

```c++
char& operator[](int n);//通过[]方式取字符
char& at(int n);//通过at方法获取字符
```

#### 插入和删除

```c++
string& insert(int pos, const char* s);//插入字符串
string& insert(int pos, const string& str);//插入字符串
string& insert(int pos, int n, char c);//在指定位置插入n个字符c
string& erase(int pos, int n = npos);//删除从Pos开始的n个字符
```

#### 字符串子串

```c++
string substr(int pos = 0, int n = npos) const;
//返回由pos开始的n个字符组成的字符串
```

### vector

单端数组，可以动态扩展，但扩展时不是直接接上，而是寻找更大的空间拷贝过去，释放原来的空间

#### 构造函数

```c++
vector<T> v;//采用模板实现类实现，默认构造函数
vector(v.begin(), v.end());//将v[begin(), end())区间中的元素拷贝给本身。
vector(n, element);//构造函数将n个elem拷贝给本身。
vector(const vector &vec);//拷贝构造函数。
```

#### 赋值

```c++
vector& operator=(const vector &vec); //重载等号操作符
assign(begin, end);//将[begin, end)区间中的数据拷贝赋值给本身。
assign(n, element);//将n个elem拷贝赋值给本身。
```

#### 容量和大小

```c++
empty(); //判断容器是否为空
capacity(); //容器的容量
size(); //返回容器中元素的个数
resize(int num); //重新指定容器的长度为num，若容器变长，则以默认值填充新位置。
//如果容器变短，则末尾超出容器长度的元素被删除。
resize(int num, elem); //重新指定容器的长度为num，若容器变长，则以elem值填充新位置。
//如果容器变短，则末尾超出容器长度的元素被删除
```

#### 插入和删除

```c++
push_back(ele);//尾部插入元素ele
pop_back();//删除最后一个元素
insert(const_iterator pos, ele);//迭代器指向位置pos插入元素ele
insert(const_iterator pos, int count,ele); //迭代器指向位置pos插入count个元素ele
erase(const_iterator pos);
//删除迭代器指向的元素
erase(const_iterator start, const_iterator end); //删除迭代器从start到end之间的元素
clear();//删除容器中所有元素
```

#### 数据存取

```c++
at(int idx);//返回索引idx所指的数据
operator[];//返回索引idx所指的数据
front();//返回容器中第一个数据元素
back();//返回容器中最后一个数据元素
```

#### 两个容器交换

```c++
swap(vec);
// 将vec与本身的元素互换
```

#### 预留空间

```c++
reserve(int len); //容器预留len个元素长度，预留位置不初始化，元素不可访问。
```

目的是减少动态扩展时的扩展次数

### deque

双端数组，头尾均可插入或删除，头部插入比vector快，但访问元素的速度没有vector快
deque是一片连续的内存空间

#### 构造函数

```c++
deque<T> deqT;//默认构造形式
deque(beg, end);//构造函数将[beg, end)区间中的元素拷贝给本身。
deque(n, elem);//构造函数将n个elem拷贝给本身。
deque(const deque &deq);//拷贝构造函数
```

#### 赋值

```c++
deque& operator=(const deque &deq);//重载等号操作符
assign(beg, end);//将[beg, end)区间中的数据拷贝赋值给本身。
assign(n, elem);//将n个elem拷贝赋值给本身。
```

#### 容量和大小

```c++
deque.empty(); //判断容器是否为空
deque.size(); //返回容器中元素的个数
deque.resize(num); //重新指定容器的长度为num,若容器变长，则以默认值填充新位置。
//如果容器变短，则末尾超出容器长度的元素被删除。
deque.resize(num, elem); //重新指定容器的长度为num,若容器变长，则以elem值填充新位置。
//如果容器变短，则末尾超出容器长度的元素被删除。
```

#### 插入和删除

```c++
push_back(elem);//在容器尾部添加一个数据
push_front(elem);//在容器头部插入一个数据
pop_back();//删除容器最后一个数据
pop_front();//删除容器第一个数据
insert(pos,elem); //在pos位置插入一个elem元素的拷贝，返回新数据的位置。
insert(pos,n,elem); //在pos位置插入n个elem数据，无返回值。
insert(pos,beg,end); //在pos位置插入[beg,end)区间的数据，无返回值。
clear(); //清空容器的所有数据
erase(beg,end); //删除[beg,end)区间的数据，返回下一个数据的位置。
erase(pos); //删除pos位置的数据，返回下一个数据的位置。
```

数据存取：

```c++
at(int idx);//返回索引idx所指的数据
operator[];//返回索引idx所指的数据
front();//返回容器中第一个数据元素
back();//返回容器中最后一个数据元素
```

#### 排序

```c++
sort(iterator beg, iterator end)
//对beg和end区间内元素进行排序
```

### stack

先进后出，只有顶部的元素才可以使用，因此无法遍历，push入栈，pop出栈

#### 常用接口

```c++
//构造函数：
stack<T> stk;//stack采用模板类实现， stack对象的默认构造形式
stack(const stack &stk);//拷贝构造函数
//赋值操作：
stack& operator=(const stack &stk);
//数据存取：
push(elem);//向栈顶添加元素
pop();//从栈顶移除第一个元素
top();//返回栈顶元素
//大小操作：
empty();//判断堆栈是否为空
size();//返回栈的大小
```

### queue

先进先出，一端进一端出，只有头尾可以被使用，因此也不能遍历

#### 常用接口

```c++
//构造函数：
queue<T> que;//queue采用模板类实现，queue对象的默认构造形式
queue(const queue &que);//拷贝构造函数
//赋值操作：
queue& operator=(const queue &que);
//重载等号操作符
//数据存取：
push(elem);//往队尾添加元素
pop();//从队头移除第一个元素
back();//返回最后一个元素
front();//返回第一个元素
//大小操作：
empty();//判断堆栈是否为空
size();//返回栈的大小
```

### list

链表，一系列指针链组成，是一个双向循环链表，储存不是连续的内存空间，list的迭代器只支持前移和后移，属于双向迭代器
采用动态储存分配，不会造成内存浪费或溢出，插入删除只需要修改指针，灵活，但空间和时间消耗大
**重要性质：** 插入和删除都不会造成原有的迭代器失效，`vector`是不可以的
另外`list`和`vector`是最常用的两个容器，各有优缺点

#### 常用接口

```c++
//构造函数
list<T> lst;//list采用采用模板类实现,对象的默认构造形式：
list(beg,end);//构造函数将[beg, end)区间中的元素拷贝给本身。
list(n,elem);//构造函数将n个elem拷贝给本身。
list(const list &lst);//拷贝构造函数。
//赋值和交换
assign(beg, end);//将[beg, end)区间中的数据拷贝赋值给本身。
assign(n, elem);//将n个elem拷贝赋值给本身。
list& operator=(const list &lst);
swap(lst);
//重载等号操作符
//将lst与本身的元素互换。
//大小
size(); //返回容器中元素的个数
empty(); //判断容器是否为空
resize(num); //重新指定容器的长度为num，若容器变长，则以默认值填充新位置。
//如果容器变短，则末尾超出容器长度的元素被删除。
resize(num, elem); //重新指定容器的长度为num，若容器变长，则以elem值填充新位置。
//如果容器变短，则末尾超出容器长度的元素被删除。
//插入和删除
push_back(elem);//在容器尾部加入一个元素
pop_back();//删除容器中最后一个元素
push_front(elem);//在容器开头插入一个元素
pop_front();//从容器开头移除第一个元素
insert(pos,elem);//在pos位置插elem元素的拷贝，返回新数据的位置。insert(pos,n,elem);//在pos位置插入n个elem数据，无返回值。
insert(pos,beg,end);//在pos位置插入[beg,end)区间的数据，无返回值。
clear();//移除容器的所有数据
erase(beg,end);//删除[beg,end)区间的数据，返回下一个数据的位置。
erase(pos);//删除pos位置的数据，返回下一个数据的位置。
remove(elem);//删除容器中所有与elem值匹配的元素。
//存取
front();//返回第一个元素。
back();//返回最后一个元素。
//反转和排序
reverse();//反转链表
sort();//链表排序
```

### set和multiset

所有元素在插入时就被自动排列
属关联式容器，底层结构是***二叉树***实现的
`set`不允许有重复的元素，`multiset`可以有重复的元素；`set`插入数据后会返回结果，表示是否插入成功，而`multiset`一定可以成功，所以不会检测数据

#### 常用接口

```c++
//构造：
set<T> st;//默认构造函数：
set(const set &st);//拷贝构造函数
//赋值：
set& operator=(const set &st);
//重载等号操作符
//大小和交换
size();
//返回容器中元素的数目
empty();//判断容器是否为空
swap(st);//交换两个集合容器
//插入和删除
insert(elem);//在容器中插入元素。
clear();//清除所有元素
erase(pos);//删除pos迭代器所指的元素，返回下一个元素的迭代器。
erase(beg, end);//删除区间[beg,end)的所有元素 ，返回下一个元素的迭代器。
erase(elem);//删除容器中值为elem的元素。
//查找和统计
find(key);//查找key是否存在,若存在，返回该键的元素的迭代器；若不存在，返回set.end();
count(key);//统计key的元素个数
```

set的默认排序规则为从小到大，利用仿函数可以改变规则

```c++
#include <iostream>
#include <set>
using namespace std;
class myCompare
{
public:
bool operator()(int v1, int v2){
 return v1 > v2;
}
};
void test(){
 set<int> set_1;
 set_1.insert(1);
 set_1.insert(2);
 set_1.insert(3);
 set_1.insert(4);
 set_1.insert(5);
 set_1.insert(6);
 for (auto it = set_1.begin(); it != set_1.end(); it++){
  std::cout << *it << " " << "\n" ;
 }
 set<int, myCompare> set_2;
 set_2.insert(2);
 set_2.insert(3);
 set_2.insert(1);
 set_2.insert(5);
 set_2.insert(4);
 set_2.insert(6);
 for (auto it = set_2.begin(); it != set_2.end(); it++){
  std::cout << *it << " " << "\n" ;
 }
}
int main(int argc, char const *argv[])
{
    test();
 return 0;
}
```

当使用自定义数据类型时，set必须指定排序规则才可以插入数据

### pair

成对出现的数据可以使用pair

#### 常用接口

```c++
//创建方式
pair<type, type> p ( value1, value2 );
pair<type, type> p = make_pair( value1, value2 );
```

### map和multimap

所有的元素都是`pair`，其中第一个元素是`key`，第二个是`value`，所有元素都会根据元素的`key`进行自动排序
属于关联式容器，底层结构是用二叉树实现
可以通过`key`值快速找到`value`
`map`不能有重复的`key`，`multimap`可以有重复的`key`

#### 常用接口

```c++
//构造：
map<T1, T2> mp;//map默认构造函数:
map(const map &mp);//拷贝构造函数
//赋值：
map& operator=(const map &mp);
//大小和交换
size();//返回容器中元素的数目
empty();//判断容器是否为空
swap(st);//交换两个集合容器
//插入和删除
insert(elem);//在容器中插入元素。
clear();//清除所有元素
erase(pos);//删除pos迭代器所指的元素，返回下一个元素的迭代器。
erase(beg, end);//删除区间[beg,end)的所有元素 ，返回下一个元素的迭代器。
erase(key);//删除容器中值为key的元素。
//查找和统计
find(key);//查找key是否存在,若存在，返回该键的元素的迭代器；若不存在，返回set.end();
count(key);//统计key的元素个数
```

也可以利用仿函数指定排序规则

### 函数对象

重载函数调用操作符的类，其对象称为函数对象
函数对象使用重载时，行为类似函数调用，所以也叫仿函数
是一个类而非函数
函数对象可以有参数也可以有返回值
但函数对象超出普通函数的概念，有自己的状态
函数对象可以作为参数进行传递

```c++
#include <iostream>
using namespace std;
class add{
public:
    add(){
        count = 0;
    }
    int operator()(int v1, int v2){
        count ++;
        return v1 + v2;
    }
    int count = 0;
};
void test(){
    add add;
    add.operator()(1,2);
    add.operator()(1,3);
    add.operator()(1,2);
    add.operator()(1,2);
    cout << add.count << endl;
}
int main(){
    test();
    return 0;
}
```

The keys of cpp comparing to c language is Object-oriented and Generic programming.

### count

类似于find,可查找字符串中某个字符出现的次数。

```c++
string s = "abcdefgaaadsasafas";
int numOfA = s.count('a');
```

当`map`类的数据使用`count`的时候，传入的参数应是`key`而非`value`。

## 三、函数对象

### 4.1 函数对象

#### 4.1.1 函数对象概念

**概念：**

- 重载**函数调用操作符**的类，其对象常称为**函数对象**
- **函数对象**使用重载的()时，行为类似函数调用，也叫**仿函数**
  **本质：**
  函数对象(仿函数)是一个**类**，不是一个函数

#### 4.1.2  函数对象使用

**特点：**

- 函数对象在使用时，可以像普通函数那样调用, 可以有参数，可以有返回值
- 函数对象超出普通函数的概念，函数对象可以有自己的状态
- 函数对象可以作为参数传递
  **示例:**

```C++
#include <string>
//1、函数对象在使用时，可以像普通函数那样调用, 可以有参数，可以有返回值
class MyAdd
{
public :
 int operator()(int v1,int v2)
 {
  return v1 + v2;
 }
};
void test01()
{
 MyAdd myAdd;
 cout << myAdd(10, 10) << endl;
}
//2、函数对象可以有自己的状态
class MyPrint
{
public:
 MyPrint()
 {
  count = 0;
 }
 void operator()(string test)
 {
  cout << test << endl;
  count++; //统计使用次数
 }
 int count; //内部自己的状态
};
void test02()
{
 MyPrint myPrint;
 myPrint("hello world");
 myPrint("hello world");
 myPrint("hello world");
 cout << "myPrint调用次数为： " << myPrint.count << endl;
}
//3、函数对象可以作为参数传递
void doPrint(MyPrint &mp , string test)
{
 mp(test);
}
void test03()
{
 MyPrint myPrint;
 doPrint(myPrint, "Hello cpp");
}
int main() {
 //test01();
 //test02();
 test03();
 return 0;
}
```

总结：

- 仿函数写法非常灵活，可以作为参数进行传递。

### 4.2  谓词

#### 4.2.1 谓词概念

**概念：**

- 返回bool类型的仿函数称为**谓词**
- 如果operator()接受一个参数，那么叫做一元谓词
- 如果operator()接受两个参数，那么叫做二元谓词

#### 4.2.2 一元谓词

**示例：**

```C++
#include <vector>
#include <algorithm>
//1.一元谓词
struct GreaterFive{
 bool operator()(int val) {
  return val > 5;
 }
};
void test01() {
 vector<int> v;
 for (int i = 0; i < 10; i++)
 {
  v.push_back(i);
 }
 vector<int>::iterator it = find_if(v.begin(), v.end(), GreaterFive());
 if (it *** v.end()) {
  cout << "没找到!" << endl;
 }
 else {
  cout << "找到:" << *it << endl;
 }
}
int main() {
 test01();
 return 0;
}
```

- 参数只有一个的谓词，称为一元谓词

#### 4.2.3 二元谓词

**示例：**

```C++
#include <vector>
#include <algorithm>
//二元谓词
class MyCompare
{
public:
 bool operator()(int num1, int num2)
 {
  return num1 > num2;
 }
};
void test01()
{
 vector<int> v;
 v.push_back(10);
 v.push_back(40);
 v.push_back(20);
 v.push_back(30);
 v.push_back(50);
 //默认从小到大
 sort(v.begin(), v.end());
 for (vector<int>::iterator it = v.begin(); it != v.end(); it++)
 {
  cout << *it << " ";
 }
 cout << endl;
 cout << "----------------------------" << endl;
 //使用函数对象改变算法策略，排序从大到小
 sort(v.begin(), v.end(), MyCompare());
 for (vector<int>::iterator it = v.begin(); it != v.end(); it++)
 {
  cout << *it << " ";
 }
 cout << endl;
}
int main() {
 test01();
 return 0;
}
```

- 参数只有两个的谓词，称为二元谓词

### 4.3 内建函数对象

#### 4.3.1 内建函数对象意义

**概念：**

- STL内建了一些函数对象
  **分类:**
- 算术仿函数
- 关系仿函数
- 逻辑仿函数
  **用法：**
- 这些仿函数所产生的对象，用法和一般函数完全相同
- 使用内建函数对象，需要引入头文件 `#include <functional>`

#### 4.3.2 算术仿函数

**功能描述：**

- 实现四则运算
- 其中negate是一元运算，其他都是二元运算
  **仿函数原型：**
- `template<class T> T plus<T>`                //加法仿函数
- `template<class T> T minus<T>`              //减法仿函数
- `template<class T> T multiplies<T>`    //乘法仿函数
- `template<class T> T divides<T>`         //除法仿函数
- `template<class T> T modulus<T>`         //取模仿函数
- `template<class T> T negate<T>`           //取反仿函数
  **示例：**

```C++
#include <functional>
//negate
void test01()
{
 negate<int> n;
 cout << n(50) << endl;
}
//plus
void test02()
{
 plus<int> p;
 cout << p(10, 20) << endl;
}
int main() {
 test01();
 test02();
 system("pause");
 return 0;
}
```

- 使用内建函数对象时，需要引入头文件 `#include <functional>`

#### 4.3.3 关系仿函数

**功能描述：**

- 实现关系对比
  **仿函数原型：**

- `template<class T> bool equal_to<T>`                    //等于

- `template<class T> bool not_equal_to<T>`            //不等于

- `template<class T> bool greater<T>`                      //大于

- `template<class T> bool greater_equal<T>`          //大于等于

- `template<class T> bool less<T>`                           //小于

- `template<class T> bool less_equal<T>`               //小于等于
  **示例：**

```C++
#include <functional>
#include <vector>
#include <algorithm>
class MyCompare
{
public:
 bool operator()(int v1,int v2)
 {
  return v1 > v2;
 }
};
void test01()
{
 vector<int> v;
 v.push_back(10);
 v.push_back(30);
 v.push_back(50);
 v.push_back(40);
 v.push_back(20);
 for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
  cout << *it << " ";
 }
 cout << endl;
 //自己实现仿函数
 //sort(v.begin(), v.end(), MyCompare());
 //STL内建仿函数  大于仿函数
 sort(v.begin(), v.end(), greater<int>());
 for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
  cout << *it << " ";
 }
 cout << endl;
}
int main() {
 test01();
 return 0;
}
```

关系仿函数中最常用的就是`greater<>`大于

#### 4.3.4 逻辑仿函数

**功能描述：**

- 实现逻辑运算
  **函数原型：**

- `template<class T> bool logical_and<T>`              //逻辑与

- `template<class T> bool logical_or<T>`                //逻辑或

- `template<class T> bool logical_not<T>`              //逻辑非
  **示例：**

```C++
#include <vector>
#include <functional>
#include <algorithm>
void test01()
{
 vector<bool> v;
 v.push_back(true);
 v.push_back(false);
 v.push_back(true);
 v.push_back(false);
 for (vector<bool>::iterator it = v.begin();it!= v.end();it++)
 {
  cout << *it << " ";
 }
 cout << endl;
 //逻辑非  将v容器搬运到v2中，并执行逻辑非运算
 vector<bool> v2;
 v2.resize(v.size());
 transform(v.begin(), v.end(),  v2.begin(), logical_not<bool>());
 for (vector<bool>::iterator it = v2.begin(); it != v2.end(); it++)
 {
  cout << *it << " ";
 }
 cout << endl;
}
int main() {
 test01();
 return 0;
}
```

- 逻辑仿函数实际应用较少，了解即可

## 四、STL常用算法

# Algorithm

**概述**:

- 算法主要是由头文件`<algorithm>` `<functional>` `<numeric>`组成。
- `<algorithm>`是所有STL头文件中最大的一个，范围涉及到比较、 交换、查找、遍历操作、复制、修改等等
- `<numeric>`体积很小，只包括几个在序列上面进行简单数学运算的模板函数
- `<functional>`定义了一些模板类,用以声明函数对象。

### 5.1 常用遍历算法

**学习目标：**

- 掌握常用的遍历算法
  **算法简介：**
- `for_each`     //遍历容器
- `transform`   //搬运容器到另一个容器中

#### 5.1.1 for_each

**功能描述：**

- 实现遍历容器
  **函数原型：**
- `for_each(iterator beg, iterator end, _func);`
  // 遍历算法 遍历容器元素
  // beg 开始迭代器
  // end 结束迭代器
  // \_func 函数或者函数对象
  **示例：**

```C++
#include <algorithm>
#include <vector>
//普通函数
void print01(int val)
{
 cout << val << " ";
}
//函数对象
class print02
{
 public:
 void operator()(int val)
 {
  cout << val << " ";
 }
};
//for_each算法基本用法
void test01() {
 vector<int> v;
 for (int i = 0; i < 10; i++)
 {
  v.push_back(i);
 }
 //遍历算法
 for_each(v.begin(), v.end(), print01);
 cout << endl;
 for_each(v.begin(), v.end(), print02());
 cout << endl;
}
int main() {
 test01();
 return 0;
}
```

- `for_each`在实际开发中是最常用遍历算法，需要熟练掌握

#### 5.1.2 transform

**功能描述：**

- 搬运容器到另一个容器中
  **函数原型：**
- `transform(iterator beg1, iterator end1, iterator beg2, _func);`
  //beg1 源容器开始迭代器
  //end1 源容器结束迭代器
  //beg2 目标容器开始迭代器
  //\_func 函数或者函数对象
  **示例：**

```C++
#include<vector>
#include<algorithm>
//常用遍历算法  搬运 transform
class TransForm
{
public:
 int operator()(int val)
 {
  return val;
 }
};
class MyPrint
{
public:
 void operator()(int val)
 {
  cout << val << " ";
 }
};
void test01()
{
 vector<int>v;
 for (int i = 0; i < 10; i++)
 {
  v.push_back(i);
 }
 vector<int>vTarget; //目标容器
 vTarget.resize(v.size()); // 目标容器需要提前开辟空间
 transform(v.begin(), v.end(), vTarget.begin(), TransForm());
 for_each(vTarget.begin(), vTarget.end(), MyPrint());
}
int main() {
 test01();
 return 0;
}
```

- 搬运的目标容器必须要提前开辟空间，否则无法正常搬运
  也可以用来转换大小写：

```cpp
transform(str.begin(),str.end(),str.begin(),::tolower); transform(str.begin(),str.end(),str.begin(),::toupper);
```

### 5.2 常用查找算法

学习目标：

- 掌握常用的查找算法
  **算法简介：**
- `find`                     //查找元素
- `find_if`               //按条件查找元素
- `adjacent_find`    //查找相邻重复元素
- `binary_search`    //二分查找法
- `count`                   //统计元素个数
- `count_if`             //按条件统计元素个数

#### 5.2.1 find

**功能描述：**

- 查找指定元素，找到返回指定元素的迭代器，找不到返回结束迭代器end()
  **函数原型：**

- `find(iterator beg, iterator end, value);`
  // 按值查找元素，找到返回指定位置迭代器，找不到返回结束迭代器位置
  // beg 开始迭代器
  // end 结束迭代器
  // value 查找的元素
  **示例：**

```C++
#include <algorithm>
#include <vector>
#include <string>
void test01() {
 vector<int> v;
 for (int i = 0; i < 10; i++) {
  v.push_back(i + 1);
 }
 //查找容器中是否有 5 这个元素
 vector<int>::iterator it = find(v.begin(), v.end(), 5);
 if (it *** v.end())
 {
  cout << "没有找到!" << endl;
 }
 else
 {
  cout << "找到:" << *it << endl;
 }
}
class Person {
public:
 Person(string name, int age)
 {
  this->m_Name = name;
  this->m_Age = age;
 }
 //重载***
 bool operator***(const Person& p)
 {
  if (this->m_Name *** p.m_Name && this->m_Age *** p.m_Age)
  {
   return true;
  }
  return false;
 }
public:
 string m_Name;
 int m_Age;
};
void test02() {
 vector<Person> v;
 //创建数据
 Person p1("aaa", 10);
 Person p2("bbb", 20);
 Person p3("ccc", 30);
 Person p4("ddd", 40);
 v.push_back(p1);
 v.push_back(p2);
 v.push_back(p3);
 v.push_back(p4);
 vector<Person>::iterator it = find(v.begin(), v.end(), p2);
 if (it *** v.end())
 {
  cout << "没有找到!" << endl;
 }
 else
 {
  cout << "找到姓名:" << it->m_Name << " 年龄: " << it->m_Age << endl;
 }
}
```

- 利用find可以在容器中找指定的元素，返回值是**迭代器**

#### 5.2.2 find_if

**功能描述：**

- 按条件查找元素
  **函数原型：**

- `find_if(iterator beg, iterator end, _Pred);`
  // 按值查找元素，找到返回指定位置迭代器，找不到返回结束迭代器位置
  // beg 开始迭代器
  // end 结束迭代器
  // \_Pred 函数或者谓词（返回bool类型的仿函数）
  **示例：**

```C++
#include <algorithm>
#include <vector>
#include <string>
//内置数据类型
class GreaterFive
{
public:
 bool operator()(int val)
 {
  return val > 5;
 }
};
void test01() {
 vector<int> v;
 for (int i = 0; i < 10; i++) {
  v.push_back(i + 1);
 }
 vector<int>::iterator it = find_if(v.begin(), v.end(), GreaterFive());
 if (it *** v.end()) {
  cout << "没有找到!" << endl;
 }
 else {
  cout << "找到大于5的数字:" << *it << endl;
 }
}
//自定义数据类型
class Person {
public:
 Person(string name, int age)
 {
  this->m_Name = name;
  this->m_Age = age;
 }
public:
 string m_Name;
 int m_Age;
};
class Greater20
{
public:
 bool operator()(Person &p)
 {
  return p.m_Age > 20;
 }
};
void test02() {
 vector<Person> v;
 //创建数据
 Person p1("aaa", 10);
 Person p2("bbb", 20);
 Person p3("ccc", 30);
 Person p4("ddd", 40);
 v.push_back(p1);
 v.push_back(p2);
 v.push_back(p3);
 v.push_back(p4);
 vector<Person>::iterator it = find_if(v.begin(), v.end(), Greater20());
 if (it *** v.end())
 {
  cout << "没有找到!" << endl;
 }
 else
 {
  cout << "找到姓名:" << it->m_Name << " 年龄: " << it->m_Age << endl;
 }
}
int main() {
 //test01();
 test02();
 return 0;
}
```

- `find_if`按条件查找使查找更加灵活，提供的仿函数可以改变不同的策略

#### 5.2.3 adjacent_find

**功能描述：**

- 查找相邻重复元素
  **函数原型：**

- `adjacent_find(iterator beg, iterator end);`
  // 查找相邻重复元素,返回相邻元素的第一个位置的迭代器
  // beg 开始迭代器
  // end 结束迭代器
  **示例：**

```C++
#include <algorithm>
#include <vector>
void test01()
{
 vector<int> v;
 v.push_back(1);
 v.push_back(2);
 v.push_back(5);
 v.push_back(2);
 v.push_back(4);
 v.push_back(4);
 v.push_back(3);
 //查找相邻重复元素
 vector<int>::iterator it = adjacent_find(v.begin(), v.end());
 if (it *** v.end()) {
  cout << "找不到!" << endl;
 }
 else {
  cout << "找到相邻重复元素为:" << *it << endl;
 }
}
```

- 面试题中如果出现查找相邻重复元素，记得用STL中的`adjacent_find`算法

#### 5.2.4 binary_search

**功能描述：**

- 查找指定元素是否存在
  **函数原型：**

- `bool binary_search(iterator beg, iterator end, value);`
  // 查找指定的元素，查到 返回true  否则false
  // 注意: 在**无序序列中不可用**
  // beg 开始迭代器
  // end 结束迭代器
  // value 查找的元素
  **示例：**

```C++
#include <algorithm>
#include <vector>
void test01()
{
 vector<int>v;
 for (int i = 0; i < 10; i++)
 {
  v.push_back(i);
 }
 //二分查找
 bool ret = binary_search(v.begin(), v.end(),2);
 if (ret)
 {
  cout << "找到了" << endl;
 }
 else
 {
  cout << "未找到" << endl;
 }
}
int main() {
 test01();
 return 0;
}
```

**总结：** 二分查找法查找效率很高，值得注意的是***查找的容器中元素必须的有序序列***

#### 5.2.5 count

**功能描述：**

- 统计元素个数
  **函数原型：**

- `count(iterator beg, iterator end, value);`
  // 统计元素出现次数
  // beg 开始迭代器
  // end 结束迭代器
  // value 统计的元素
  **示例：**

```C++
#include <algorithm>
#include <vector>
//内置数据类型
void test01()
{
 vector<int> v;
 v.push_back(1);
 v.push_back(2);
 v.push_back(4);
 v.push_back(5);
 v.push_back(3);
 v.push_back(4);
 v.push_back(4);
 int num = count(v.begin(), v.end(), 4);
 cout << "4的个数为： " << num << endl;
}
//自定义数据类型
class Person
{
public:
 Person(string name, int age)
 {
  this->m_Name = name;
  this->m_Age = age;
 }
 bool operator***(const Person & p)
 {
  if (this->m_Age *** p.m_Age)
  {
   return true;
  }
  else
  {
   return false;
  }
 }
 string m_Name;
 int m_Age;
};
void test02()
{
 vector<Person> v;
 Person p1("刘备", 35);
 Person p2("关羽", 35);
 Person p3("张飞", 35);
 Person p4("赵云", 30);
 Person p5("曹操", 25);
 v.push_back(p1);
 v.push_back(p2);
 v.push_back(p3);
 v.push_back(p4);
 v.push_back(p5);
    Person p("诸葛亮",35);
 int num = count(v.begin(), v.end(), p);
 cout << "num = " << num << endl;
}
int main() {
 //test01();
 test02();
 return 0;
}
```

- 统计自定义数据类型时候，需要配合重载 `operator***`

#### 5.2.6 count_if

**功能描述：**

- 按条件统计元素个数
  **函数原型：**

- `count_if(iterator beg, iterator end, _Pred);`
  // 按条件统计元素出现次数
  // beg 开始迭代器
  // end 结束迭代器
  // \_Pred 谓词
  **示例：**

```C++
#include <algorithm>
#include <vector>
class Greater4
{
public:
 bool operator()(int val)
 {
  return val >= 4;
 }
};
//内置数据类型
void test01()
{
 vector<int> v;
 v.push_back(1);
 v.push_back(2);
 v.push_back(4);
 v.push_back(5);
 v.push_back(3);
 v.push_back(4);
 v.push_back(4);
 int num = count_if(v.begin(), v.end(), Greater4());
 cout << "大于4的个数为： " << num << endl;
}
//自定义数据类型
class Person
{
public:
 Person(string name, int age)
 {
  this->m_Name = name;
  this->m_Age = age;
 }
 string m_Name;
 int m_Age;
};
class AgeLess35
{
public:
 bool operator()(const Person &p)
 {
  return p.m_Age < 35;
 }
};
void test02()
{
 vector<Person> v;
 Person p1("刘备", 35);
 Person p2("关羽", 35);
 Person p3("张飞", 35);
 Person p4("赵云", 30);
 Person p5("曹操", 25);
 v.push_back(p1);
 v.push_back(p2);
 v.push_back(p3);
 v.push_back(p4);
 v.push_back(p5);
 int num = count_if(v.begin(), v.end(), AgeLess35());
 cout << "小于35岁的个数：" << num << endl;
}
int main() {
 //test01();
 test02();
 return 0;
}
```

- 按值统计用`count`，按条件统计用`count_if`

### 5.3 常用排序算法

**学习目标：**

- 掌握常用的排序算法
  **算法简介：**
- `sort`             //对容器内元素进行排序
- `random_shuffle`   //洗牌   指定范围内的元素随机调整次序
- `merge`           // 容器元素合并，并存储到另一容器中
- `reverse`       // 反转指定范围的元素

#### 5.3.1 sort

**功能描述：**

- 对容器内元素进行排序
  **函数原型：**

- `sort(iterator beg, iterator end, _Pred);`
  // 按值查找元素，找到返回指定位置迭代器，找不到返回结束迭代器位置
  //  beg    开始迭代器
  //  end    结束迭代器
  // \_Pred  谓词
  **示例：**

```c++
#include <algorithm>
#include <vector>
void myPrint(int val)
{
 cout << val << " ";
}
void test01() {
 vector<int> v;
 v.push_back(10);
 v.push_back(30);
 v.push_back(50);
 v.push_back(20);
 v.push_back(40);
 //sort默认从小到大排序
 sort(v.begin(), v.end());
 for_each(v.begin(), v.end(), myPrint);
 cout << endl;
 //从大到小排序
 sort(v.begin(), v.end(), greater<int>());
 for_each(v.begin(), v.end(), myPrint);
 cout << endl;
}
int main() {
 test01();
 return 0;
}
```

- `sort`属于开发中最常用的算法之一，需熟练掌握

#### 5.3.2 random_shuffle

**功能描述：**

- 洗牌   指定范围内的元素随机调整次序
  **函数原型：**

- `random_shuffle(iterator beg, iterator end);`
  // 指定范围内的元素随机调整次序
  // beg 开始迭代器
  // end 结束迭代器
  **示例：**

```c++
#include <algorithm>
#include <vector>
#include <ctime>
class myPrint
{
public:
 void operator()(int val)
 {
  cout << val << " ";
 }
};
void test01()
{
 srand((unsigned int)time(NULL));
 vector<int> v;
 for(int i = 0 ; i < 10;i++)
 {
  v.push_back(i);
 }
 for_each(v.begin(), v.end(), myPrint());
 cout << endl;
 //打乱顺序
 random_shuffle(v.begin(), v.end());
 for_each(v.begin(), v.end(), myPrint());
 cout << endl;
}
int main() {
 test01();
 return 0;
}
```

- `random_shuffle`洗牌算法比较实用，使用时记得加随机数种子

#### 5.3.3 merge

**功能描述：**

- 两个容器元素合并，并存储到另一容器中
  **函数原型：**

- `merge(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);`
  // 容器元素合并，并存储到另一容器中
  // 注意: 两个容器必须是**有序的**
  // beg1   容器1开始迭代器
  // end1   容器1结束迭代器
  // beg2   容器2开始迭代器
  // end2   容器2结束迭代器
  // dest    目标容器开始迭代器
  **示例：**

```c++
#include <algorithm>
#include <vector>
class myPrint
{
public:
 void operator()(int val)
 {
  cout << val << " ";
 }
};
void test01()
{
 vector<int> v1;
 vector<int> v2;
 for (int i = 0; i < 10 ; i++)
    {
  v1.push_back(i);
  v2.push_back(i + 1);
 }
 vector<int> vtarget;
 //目标容器需要提前开辟空间
 vtarget.resize(v1.size() + v2.size());
 //合并  需要两个有序序列
 merge(v1.begin(), v1.end(), v2.begin(), v2.end(), vtarget.begin());
 for_each(vtarget.begin(), vtarget.end(), myPrint());
 cout << endl;
}
int main() {
 test01();
 return 0;
}
```

- `merge`合并的两个容器必须的有序序列

#### 5.3.4 reverse

**功能描述：**

- 将容器内元素进行反转
  **函数原型：**

- `reverse(iterator beg, iterator end);`
  // 反转指定范围的元素
  // beg 开始迭代器
  // end 结束迭代器
  **示例：**

```c++
#include <algorithm>
#include <vector>
class myPrint
{
public:
 void operator()(int val)
 {
  cout << val << " ";
 }
};
void test01()
{
 vector<int> v;
 v.push_back(10);
 v.push_back(30);
 v.push_back(50);
 v.push_back(20);
 v.push_back(40);
 cout << "反转前： " << endl;
 for_each(v.begin(), v.end(), myPrint());
 cout << endl;
 cout << "反转后： " << endl;
 reverse(v.begin(), v.end());
 for_each(v.begin(), v.end(), myPrint());
 cout << endl;
}
int main() {
 test01();
 return 0;
}
```

- `reverse`反转区间内元素，面试题可能涉及到

### 5.4 常用拷贝和替换算法

**学习目标：**

- 掌握常用的拷贝和替换算法
  **算法简介：**
- `copy`                      // 容器内指定范围的元素拷贝到另一容器中
- `replace`                // 将容器内指定范围的旧元素修改为新元素
- `replace_if`          // 容器内指定范围满足条件的元素替换为新元素
- `swap`                     // 互换两个容器的元素

#### 5.4.1 copy

**功能描述：**

- 容器内指定范围的元素拷贝到另一容器中
  **函数原型：**

- `copy(iterator beg, iterator end, iterator dest);`
  // 按值查找元素，找到返回指定位置迭代器，找不到返回结束迭代器位置
  // beg  开始迭代器
  // end  结束迭代器
  // dest 目标起始迭代器
  **示例：**

```c++
#include <algorithm>
#include <vector>
class myPrint
{
public:
 void operator()(int val)
 {
  cout << val << " ";
 }
};
void test01()
{
 vector<int> v1;
 for (int i = 0; i < 10; i++) {
  v1.push_back(i + 1);
 }
 vector<int> v2;
 v2.resize(v1.size());
 copy(v1.begin(), v1.end(), v2.begin());
 for_each(v2.begin(), v2.end(), myPrint());
 cout << endl;
int main() {
 test01();
 return 0;
}
```

- 利用`copy`算法在拷贝时，目标容器记得提前开辟空间

#### 5.4.2 replace

**功能描述：**

- 将容器内指定范围的旧元素修改为新元素
  **函数原型：**

- `replace(iterator beg, iterator end, oldvalue, newvalue);`
  // 将区间内旧元素 替换成 新元素
  // beg 开始迭代器
  // end 结束迭代器
  // oldvalue 旧元素
  // newvalue 新元素
  **示例：**

```c++
#include <algorithm>
#include <vector>
class myPrint
{
public:
 void operator()(int val)
 {
  cout << val << " ";
 }
};
void test01()
{
 vector<int> v;
 v.push_back(20);
 v.push_back(30);
 v.push_back(20);
 v.push_back(40);
 v.push_back(50);
 v.push_back(10);
 v.push_back(20);
 cout << "替换前：" << endl;
 for_each(v.begin(), v.end(), myPrint());
 cout << endl;
 //将容器中的20 替换成 2000
 cout << "替换后：" << endl;
 replace(v.begin(), v.end(), 20,2000);
 for_each(v.begin(), v.end(), myPrint());
 cout << endl;
}
int main() {
 test01();
 return 0;
}
```

- `replace`会替换区间内满足条件的元素

#### 5.4.3 replace_if

**功能描述:**

- 将区间内满足条件的元素，替换成指定元素
  **函数原型：**

- `replace_if(iterator beg, iterator end, _pred, newvalue);`
  // 按条件替换元素，满足条件的替换成指定元素
  // beg 开始迭代器
  // end 结束迭代器
  // \_pred 谓词
  // newvalue 替换的新元素
  **示例：**

```c++
#include <algorithm>
#include <vector>
class myPrint
{
public:
 void operator()(int val)
 {
  cout << val << " ";
 }
};
class ReplaceGreater30
{
public:
 bool operator()(int val)
 {
  return val >= 30;
 }
};
void test01()
{
 vector<int> v;
 v.push_back(20);
 v.push_back(30);
 v.push_back(20);
 v.push_back(40);
 v.push_back(50);
 v.push_back(10);
 v.push_back(20);
 cout << "替换前：" << endl;
 for_each(v.begin(), v.end(), myPrint());
 cout << endl;
 //将容器中大于等于的30 替换成 3000
 cout << "替换后：" << endl;
 replace_if(v.begin(), v.end(), ReplaceGreater30(), 3000);
 for_each(v.begin(), v.end(), myPrint());
 cout << endl;
}
int main() {
 test01();
 return 0;
}
```

- `replace_if`按条件查找，可以利用仿函数灵活筛选满足的条件

#### 5.4.4 swap

**功能描述：**

- 互换两个容器的元素
  **函数原型：**

- `swap(container c1, container c2);`
  // 互换两个容器的元素
  // c1容器1
  // c2容器2
  **示例：**

```c++
#include <algorithm>
#include <vector>
class myPrint
{
public:
 void operator()(int val)
 {
  cout << val << " ";
 }
};
void test01()
{
 vector<int> v1;
 vector<int> v2;
 for (int i = 0; i < 10; i++) {
  v1.push_back(i);
  v2.push_back(i+100);
 }
 cout << "交换前： " << endl;
 for_each(v1.begin(), v1.end(), myPrint());
 cout << endl;
 for_each(v2.begin(), v2.end(), myPrint());
 cout << endl;
 cout << "交换后： " << endl;
 swap(v1, v2);
 for_each(v1.begin(), v1.end(), myPrint());
 cout << endl;
 for_each(v2.begin(), v2.end(), myPrint());
 cout << endl;
}
int main() {
 test01();
 return 0;
}
```

- `swap`交换容器时，注意交换的容器要同种类型

### 5.5 常用算术生成算法

**学习目标：**

- 掌握常用的算术生成算法
  **注意：**

- 算术生成算法属于小型算法，使用时包含的头文件为 `#include <numeric>`
  **算法简介：**

- `accumulate`      // 计算容器元素累计总和

- `fill`                 // 向容器中添加元素

#### 5.5.1 accumulate

**功能描述：**

- 计算区间内 容器元素累计总和
  **函数原型：**

- `accumulate(iterator beg, iterator end, value);`
  // 计算容器元素累计总和
  // beg 开始迭代器
  // end 结束迭代器
  // value 起始值
  **示例：**

```c++
#include <numeric>
#include <vector>
void test01()
{
 vector<int> v;
 for (int i = 0; i <= 100; i++) {
  v.push_back(i);
 }
 int total = accumulate(v.begin(), v.end(), 0);
 cout << "total = " << total << endl;
}
int main() {
 test01();
 return 0;
}
```

- `accumulate`使用时头文件注意是 `numeric`，这个算法很实用

#### 5.5.2 fill

**功能描述：**

- 向容器中填充指定的元素
  **函数原型：**

- `fill(iterator beg, iterator end, value);`
  // 向容器中填充元素
  // beg 开始迭代器
  // end 结束迭代器
  // value 填充的值
  **示例：**

```c++
#include <numeric>
#include <vector>
#include <algorithm>
class myPrint
{
public:
 void operator()(int val)
 {
  cout << val << " ";
 }
};
void test01()
{
 vector<int> v;
 v.resize(10);
 //填充
 fill(v.begin(), v.end(), 100);
 for_each(v.begin(), v.end(), myPrint());
 cout << endl;
}
int main() {
 test01();
 return 0;
}
```

- 利用`fill`可以将容器区间内元素填充为指定的值

### 5.6 常用集合算法

**学习目标：**

- 掌握常用的集合算法
  **算法简介：**
- `set_intersection`          // 求两个容器的交集
- `set_union`                       // 求两个容器的并集
- `set_difference`              // 求两个容器的差集

#### 5.6.1 set_intersection

**功能描述：**

- 求两个容器的交集
  **函数原型：**

- `set_intersection(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);`
  // 求两个集合的交集
  // **注意：两个集合必须是有序序列**
  // beg1 容器1开始迭代器
  // end1 容器1结束迭代器
  // beg2 容器2开始迭代器
  // end2 容器2结束迭代器
  // dest 目标容器开始迭代器
  **示例：**

```C++
#include <vector>
#include <algorithm>
class myPrint
{
public:
 void operator()(int val)
 {
  cout << val << " ";
 }
};
void test01()
{
 vector<int> v1;
 vector<int> v2;
 for (int i = 0; i < 10; i++)
    {
  v1.push_back(i);
  v2.push_back(i+5);
 }
 vector<int> vTarget;
 //取两个里面较小的值给目标容器开辟空间
 vTarget.resize(min(v1.size(), v2.size()));
 //返回目标容器的最后一个元素的迭代器地址
 vector<int>::iterator itEnd =
        set_intersection(v1.begin(), v1.end(), v2.begin(), v2.end(), vTarget.begin());
 for_each(vTarget.begin(), itEnd, myPrint());
 cout << endl;
}
int main() {
 test01();
 return 0;
}
```

**总结：**

- 求交集的两个集合必须的有序序列
- 目标容器开辟空间需要从**两个容器中取小值**
- set_intersection返回值既是交集中最后一个元素的位置

#### 5.6.2 set_union

**功能描述：**

- 求两个集合的并集
  **函数原型：**

- `set_union(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);`
  // 求两个集合的并集
  // **注意:两个集合必须是有序序列**
  // beg1 容器1开始迭代器
  // end1 容器1结束迭代器
  // beg2 容器2开始迭代器
  // end2 容器2结束迭代器
  // dest 目标容器开始迭代器
  **示例：**

```C++
#include <vector>
#include <algorithm>
class myPrint
{
public:
 void operator()(int val)
 {
  cout << val << " ";
 }
};
void test01()
{
 vector<int> v1;
 vector<int> v2;
 for (int i = 0; i < 10; i++) {
  v1.push_back(i);
  v2.push_back(i+5);
 }
 vector<int> vTarget;
 //取两个容器的和给目标容器开辟空间
 vTarget.resize(v1.size() + v2.size());
 //返回目标容器的最后一个元素的迭代器地址
 vector<int>::iterator itEnd =
        set_union(v1.begin(), v1.end(), v2.begin(), v2.end(), vTarget.begin());
 for_each(vTarget.begin(), itEnd, myPrint());
 cout << endl;
}
int main() {
 test01();
 return 0;
}
```

**总结：**

- 求并集的两个集合必须的有序序列
- 目标容器开辟空间需要**两个容器相加**
- set_union返回值既是并集中最后一个元素的位置

#### 5.6.3  set_difference

**功能描述：**

- 求两个集合的差集
  **函数原型：**

- `set_difference(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);`
  // 求两个集合的差集
  // **注意:两个集合必须是有序序列**
  // beg1 容器1开始迭代器
  // end1 容器1结束迭代器
  // beg2 容器2开始迭代器
  // end2 容器2结束迭代器
  // dest 目标容器开始迭代器
  **示例：**

```C++
#include <vector>
#include <algorithm>
class myPrint
{
public:
 void operator()(int val)
 {
  cout << val << " ";
 }
};
void test01()
{
 vector<int> v1;
 vector<int> v2;
 for (int i = 0; i < 10; i++) {
  v1.push_back(i);
  v2.push_back(i+5);
 }
 vector<int> vTarget;
 //取两个里面较大的值给目标容器开辟空间
 vTarget.resize( max(v1.size() , v2.size()));
 //返回目标容器的最后一个元素的迭代器地址
 cout << "v1与v2的差集为： " << endl;
 vector<int>::iterator itEnd =
        set_difference(v1.begin(), v1.end(), v2.begin(), v2.end(), vTarget.begin());
 for_each(vTarget.begin(), itEnd, myPrint());
 cout << endl;
 cout << "v2与v1的差集为： " << endl;
 itEnd = set_difference(v2.begin(), v2.end(), v1.begin(), v1.end(), vTarget.begin());
 for_each(vTarget.begin(), itEnd, myPrint());
 cout << endl;
}
int main() {
 test01();
 return 0;
}
```

- 求差集的两个集合必须的有序序列
- 目标容器开辟空间需要从**两个容器取较大值**
- set_difference返回值既是差集中最后一个元素的位置