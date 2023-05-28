---
title: C Plus Plus - Skill
date: 2022-05-25 17:34:01
categories: [Programming, Language]
tags: [cpp]
comment: true
sticky: 997
---

C++ 技巧

<!-- more -->

## virtual函数

虚函数的调用取决于指向或者引用的对象的类型，而不是指针或者引用自身的类型。

静态函数不可以声明为虚函数，同时也不能被`const` 和 `volatile`关键字修饰。

构造函数不可以声明为虚函数。同时除了`inline|explicit`之外，构造函数不允许使用其它任何关键字。

## 多态

本质上是，利用继承和虚函数实现多种不同的类调用同一个函数，此函数在不同的子类中有不同的实现，但函数名一样。

## 模板

### 模板函数

提供一种操作，但支持多种数据类型，可有效减少代码量，适用于多种数据进行同类型操作

```c++
template <typename T>
void swap(T &a, T &b){
    T temp = a;
    a = b;
    b = temp;
}
int a = 10;
int b = 20;
swap(a, b); //自动推导数据类型
swap<int>(a, b);  //指定数据类型
```

声明中typename与class没有差别
自动推导数据类型时，必须推导出一致的数据类型才能使用
在没有确定类型参数的情况下，模板无法自动推导，此时模板没有确定的数据类型，那么模板无法使用，必须手动指定数据类型
普通函数调用时可以发生自动类型转换（隐式类型转换），但模板函数自动推导类型时无法隐式类型转换，只有指定类型后才可以
从某种程度上说，函数模板在指定类型后，与普通函数相差不大

### 对用规则

函数模板与普通函数都可以实现的情况下，优先使用普通函数
可以通过空模板参数来强制调用函数模板
函数模板可以重载

### 具体化函数模板

函数模板不能直接传入自定义类型或数组，可以通过具体化函数模板来解决

```c++
class person{
    int age;
    double height;
}
template<class T>
bool compare(T &a, T &b){
    if (a *** b) return true;
    return false;
}
template<> bool compare(person &a, person &b){
    if (a.age *** b.age && a.height *** b.height) return true;
    return false;
}
person a;
person b;
a.age = 10;
a.height = 175;
b.age = 24;
b.height = 168;
compare(a, b);
```

“学习模板不是为了写模板，而是为了熟练运用STL提供的模板”

### 类模板

创建一个数据类模板，此模板可以使用多个未定的数据类型，给其赋值时需要指定数据类型

```c++
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
```

类模板无法自动推导类型，但可以有默认参数（即默认的数据类型）
类模板中的成员函数在调用时才生成
当类模板的对象作为函数的参数时，可以指定传入对象的数据类型，也可以将参数模板化，或将整个类模板化

### 类模板与继承

当子类继承的父类是一个类模板时，子类在声明的时候，要指定出父类中T的类型
如果不指定，编译器无法给子类分配内存
如果想灵活指定出父类中T的类型，子类也需变为类模板

## 文件读写

正常读取：

```c++
#include <fstream> //包含头文件
ifstream ifs;//创建输入文件流
ifs.open(<File path>, ios::in);//指定文件路径和读取方式
if (!ifs.is_open()){ //判断文件是否成功打开
    <expression>
    ifs.close()//关闭打开的文件
} else{
    // 以下为读取文件的例子，依次读取即可，只是中间空格不知去哪里了
    int id;
    string name;
    int departmentId;
    int index = 0;
    while (ifs >> id && ifs >> name && ifs >> departmentId) {
        worker *worker = NULL;
        if (departmentId *** 1) {
            worker = new employee(id, name, departmentId);
        } else if (departmentId *** 2) {
            worker = new manager(id, name, departmentId);
        } else {
            worker = new boss(id, name, departmentId);
        }
        this->pWorkerArray[index] = worker;
        index++;
}
```

判断文件是否为空：

```c++
#include <fstream> //包含头文件
ifstream ifs;//创建输入文件流
char ch;
ifs >> ch;
if (ifs.eof()) { //判断ifs是否为文件的结尾，如果是，说明文件为空，如果不是说名文件不为空
    cout << "File is empty" << endl;
 <expression>
    ifs.close();
 return;
}
```

写入文件：

```c++
#include <fstream> //包含头文件
ofstream ofs;
ofs.open(FILENAME, ios::out);
for (int i = 0; i < this->workerNum; ++i) {
    ofs << this->pWorkerArray[i]->id << " "
        << this->pWorkerArray[i]->name << " "
        << this->pWorkerArray[i]->departmentId << endl;
}
ofs.close();
```

## 退出程序接口

```c++
exit(0)
```

## linux下的“按任意键继续”

This is a library `conio.h` for linux. Just copy file and paste file `conio.h` on `/usr/include/` but don't forget before you want copy paste on `/usr/include/` you must open folder as `ADMINISTRATOR` first !!

```bash
git clone https://github.com/zoelabbb/conio.h.git
cd conio.h
sudo make install
```

重启IDE

```c++
#include <conio.h>
void toContinue() {
    cout << "Press any key to continue ..." << endl;
    getch();
} //如果在while循环中，可能要用两个getch()才能正常工作
```

## string一个用法（把字符串当做字符数组）

```c++
void speechManager::createPlayer() {
    string nameSeed;
    nameSeed = "ABCDEFGHIJKL";
    for (int i = 0; i < nameSeed.size(); i++) {
        string name = "Player - ";
        name += nameSeed[i];
        player tempPlayer;
        tempPlayer.name = name;
        for (double &j: tempPlayer.score) j = 0; //看上去高级，可读性降低
        this->v1.push_back(i + 10001);
        this->players.insert(make_pair(i + 10001, tempPlayer));
    }
}
```

## 遍历容器(使用auto)

```c++
for (auto it = vector.begin(); it != vector.end(); it ++){
 <expression>
}
```

# 特性

## `decltype`

Deduce the type of statements when compiling. It can be used to define a variable.

```c++
int a = 0;
decltype(a) b = 1; // This means the type of "b" is "int"
decltype(a + b) c = a + b; // c: int
```

## `using`

The modern cpp use more `using` to define aliases of variables.

```c++
using byte = unsigned char;
using array_t = double[10]; // "array_t" is an array with "double" type and length is ten
array_t a;
a[0] = 1;                   // It can be used as a common array
using func_t = double(double);
func_t* f = sin;
std::cout << f(3.1415926 / 4);  // Calculate sin(PI/4)
```

## namespace

When a project was completed by many coders, naming conflicts in so many identifiers may be occurs.
Key word `namespace` can define a namespace with a name or not. **The same indentifiers can exist in different namespaces.**
Using `::` access to a namespace.
Don't use `;` at the end of namespace.

## Increment and Decrement

```c++
int i = 1;
a = i++; // a = 1, i = 2
int i = 1;
a = ++i; // a = 2, i = 2
```

> Both `i++` and `++i` will make `i` plus 1, but when using them in a `class`, `++i` is more efficienct.

## Built-in operation function

```c++
#include <functional>
#include <iostream>
int main(int, char **) {
  std::cout << std::plus<int>()(5, 8) << std::endl;
  std::cout << std::minus<int>()(8, 5) << std::endl;
  std::cout << std::multiplies<int>()(5, 8) << std::endl;
  std::cout << std::divides<int>()(8, 2) << std::endl;
  std::cout << std::modulus<int>()(8, 6) << std::endl;
  std::cout << std::negate<int>()(5) << std::endl;
  return 0;
}
```

## Type convertion

There are **three ways** to convert:

```c++
(DestinationType)sourceData;
DestinationType(sourceData);
static_cast<DestinationType>(sourceData);
```

## `if` / `switch` with initialization

> Limit the scope of variables as much as possible.
> cpp 17 introduced `if` / `switch` statements that allow variable initialization.

```c++
if (auto x{ std::cin.get() }; x >= 48 && x <= 57){
    std::cout << x << " is a digit." << std::endl;
} else{
    std::cout << x << " is not a digit." << std::endl;
}
```

## Range-based `for`

```c++
int a[]{
    1, 2, 3, 4, 5, 6,
};
for (auto &i : a)   // Write by reference
    i *= 10;
for (auto i : a)    // Read by value
    std::cout << i << "\t";
std::cout << std::endl;
for (auto i : {12, 25, 67, 43, 89, 54}) // Access the list directly
    std::cout << i << "\t";
std::cout << std::endl;
```

## Function and Reference

### Function

```c++
double my_sqrt(double x) {
    std::cout << "entering " << __func__ << std::endl;
    double xnew, xold{x / 2.0};
    for (;;) {
        xnew = (xold + x / xold) / 2.0;
        if (fabs(xnew - xold) < 1e-8) {
            break;
        }
        xold = xnew;
    }
    return xnew;
};
```

Result:

```bash
entering main
entering my_sqrt
1.414213
```

> To iterate is human, to recurse divine.
> To recieve multiple data, using `initializer_list` to send parameters to function.
> e.g.

```c++
double sum(std::initializer_list<double> ld) {
    double s{0};
    for (auto i : ld)
        s += i;
    return s;
}
//in main function
    std::cout << sum({1, 2, 3, 4}) << std::endl;
    std::cout << sum({1, 2, 3, 4, 5}) << std::endl;
```

The result:

```bash
10
15
```

## Inline function

Insert the `inline` keyword in front of the function defination, which is recommanded.
When the resouces of codes in a function are less than calling the function, using inline function can increase spending saving.
Inlining is at the cost of code bloat(copying), and only saves the overhead of function calls, thereby improving the execution efficiency of functions.
Inline should not be used in the following situation:

- If the code in the function body is **relatively long**, making inlining will lead to higher memory consumption costs.
- If there is a **loop** int the function body, the time to execute the code in the function body is greater than the overhead of the function call.

## Default parameter

Provide the default parameters.
When calling the function, it can automatically use the default parameters without your actual parameters.

## Function Template

Function templates implement parameterization of data types.
It can use **unknown data types** as parameters. As long as the type of data meets the requirements defined by the function template, the function template can be called with these types of data as arguments.

```c++
template <typename T> T my_min(T a, T b) { return a < b ? a : b; }
template <typename T> T my_max(T a, T b) { return a < b ? b : a; }
double sum(std::initializer_list<double> ld) {
    double s{0};
    for (auto i : ld)
        s += i;
    return s;
}
template <typename T> std::pair<T, T> my_min_max(T a, T b) {
    T t_min = my_min(a, b);
    T t_max = my_max(a, b);
    return std::make_pair(t_min, t_max);
}
template <typename T> void print(std::pair<T, T> p) {
    std::cout << typeid(T).name() << ": \t";
    std::cout << "(min: " << p.first << ", max: " << p.second << ")\n";
}
int main(int, char **) {
    print(my_min_max('a', 'b'));
    print(my_min_max(20, 10));
    print(my_min_max(1.5, 2.5));
    return 0;
    }
```

## Lambda Function

```c++
int add(int x, int y) { return x + y; }
//in main function
    int a{1}, b{2};
    std::cout << add(a, b) << std::endl;
    auto f{[](int x, int y) { return x + y; }};
    std::cout << f(a, b) << std::endl;
    auto f2{[=]() { return a + b; }};
    std::cout << f2() << std::endl;
    auto f3{[&](int x) {
        a *= x;
        b *= x;
    }};
    f3(10);
    std::cout << a << std::endl << b << std::endl;
```

Lambda function is a function that can capture the variables in a scope **autometically**.
`[]` means the capture list.
There are several commonly used forms of capture lists:

|    Forms    |                           Meaning                            |
| :---------: | :----------------------------------------------------------: |
|    `[x]`    |          Capture the variable `x` by value passing           |
|    `[=]`    |  Capture all variables in the parent scope by value passing  |
|   `[&x]`    |        Capture the variable `x` by reference passsing        |
|    `[&]`    | Capture all variables in the parent scope by reference passing |
| `[=,&x,&y]` | Capture the variables `x` and `y` by reference passing and  the rest by value passing |
|   `[&,x]`   | Capture the variable `x` by value passing and the rest by reference passing |

> Lambda function can have the real parameters.

## Reference

### Left-valued reference

> The reference is a key point and difficulty.
> A reference is an alias of the referenced object. The two are essentially the same object, but they are displayed as different names and types.

```cpp
T& r = t;
```

Where `T` is a data type, `&` is an operator, `r` is reference name and `t` is a variable of type `T`.

1. `t` cannot be constant or right value expression.
2. `r` must be initialized when defined.
   This is excatly where the reference is diferent from the pointer, that is, **there is no empty reference**.
   References cannot exist independently, but must be attached to the referenced variable, so it does not take up memory space.
   If the reference is a named variable, such a reference is called left-valued reference (in cpp98).

```cp
    int a = 2;
    int *p = &a;
    int &r = a;
    std::cout << "a:\t" << a << std::endl;
    std::cout << "*p:\t" << *p << std::endl;
    std::cout << "r:\t" << r << std::endl;
    std::cout << "The value of p:   \t" << p << std::endl;
    std::cout << "The address of p:\t" << &p << std::endl;
    std::cout << "The address of a:\t" << &a << std::endl
              << "The address of r:\t" << &r << std::endl;
    a = 3;
    std::cout << "a:\t" << a << std::endl;
    std::cout << "*p:\t" << *p << std::endl;
    std::cout << "r:\t" << r << std::endl;
    std::cout << "The value of p:   \t" << p << std::endl;
    std::cout << "The address of p:\t" << &p << std::endl;
    std::cout << "The address of a:\t" << &a << std::endl
              << "The address of r:\t" << &r << std::endl;
    r = 4;
    std::cout << "a:\t" << a << std::endl;
    std::cout << "*p:\t" << *p << std::endl;
    std::cout << "r:\t" << r << std::endl;
    std::cout << "The value of p:   \t" << p << std::endl;
    std::cout << "The address of p:\t" << &p << std::endl;
    std::cout << "The address of a:\t" << &a << std::endl
              << "The address of r:\t" << &r << std::endl;
```

The result:

```bash
a:      2
*p:     2
r:      2
The value of p:         0x7fff945b3184
The address of p:       0x7fff945b3178
The address of a:       0x7fff945b3184
The address of r:       0x7fff945b3184
a:      3
*p:     3
r:      3
The value of p:         0x7fff945b3184
The address of p:       0x7fff945b3178
The address of a:       0x7fff945b3184
The address of r:       0x7fff945b3184
a:      4
*p:     4
r:      4
The value of p:         0x7fff945b3184
The address of p:       0x7fff945b3178
The address of a:       0x7fff945b3184
The address of r:       0x7fff945b3184
```

It can be seen that the left value reference is only an **alias** of the variable, so **all operations on the lvalue reference are equivalent to the operation on the variable itself.**
Supplementary explanation:

1. Any variable can be reference, such as pointer.

```c++
int m = 3;
int* p = &m;
int*& rp = p;
```

Reference is not variable, so you cannot reference a reference! And at the same time, **pointers cannot point to a reference.**

2. Pointers can be `nullptr` or `void` type, but references cannot.

```c++
int m = 3;
void* p = &m;
p = nullptr;
void& r = m;        // error!
int&r = nullptr;    // error!
```

3. Cannot build an array of reference.
4. Constant lvalue reference can be initialized by **lvalue**, **constant lvalue** and **rvalue**.

```c++
int x = 1;
const int N = 10;
int& rn = N;        // error!
const int& rx = x;  // Reference lvalue
const int& rN = N;  // Reference constant lvalue
const bool& rB = true; // Reference constant rvalue
```

> The left reference of the constant is usually used as a formal parameter of the function. At this time, the corresponding actual parameter cannot be modified inside the function through this reference to achieve the purpose of protecting the actual parameter.

### Rvalue reference, `move()` and Move semantics

```c++
template <typename T> void my_swap(T &a, T &b) {
    T t = std::move(a);
    a = std::move(b);
    b = std::move(t);
}
```

Actually it is the `swap()` in cpp STL.
The cpp STL also provides array exchange in the form of overloading.

```cpp
template <class T, std::size_t N> void my_swap(T (&a)[N], T (&b)[N]) {
    if (&a != &b) {
        T *first1 = a;
        T *last1 = first1 + N;
        T *first2 = b;
        for (; first1 != last1; ++first1, ++first2) {
            my_swap(*first1, *first2);
        }
    }
}
```

## 易错点（待学习

const
++运算符
struct与class的异同
初始化列表