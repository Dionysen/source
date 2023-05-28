---
title: JAVA
date: 2022-05-25 17:34:01
categories: [Programming, Language]
tags: [Java]
comment: true
sticky: 99
---



<img src="https://cdn.staticaly.com/gh/Dionysen/BlogCDN@master/img/v2-c6b46c466b7408580ce7469080d6aeed_720w.jpg"  />

> 📚  学习 Java 过程中的笔记和个人理解。

<!-- more -->

## 包裹类型

Java的系统类库中有一些包裹类型，其封装了一些比较好用的函数。

```java
package com.company;

//import javax.swing.*;
import java.util.Scanner;

public class Main {

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        final int SIZE = 3;
        Integer a;

        System.out.println(Integer.MIN_VALUE);
        System.out.println(Integer.MAX_VALUE);
        System.out.println(Character.isDigit('a'));
        System.out.println(Character.isDigit('4'));
        System.out.println(Character.toLowerCase('I'));

    }
}
```

输出结果为：

```
-2147483648
2147483647
false
true
i
```

## Math 类型

**abs**

```java
package com.company;

import java.util.Scanner;

public class Main {

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        System.out.println(Math.abs(-12));
    }
}
```

输出结果为放入数字的绝对值：

```
12
```

**round**

```java
package com.company;

import java.util.Scanner;

public class Main {

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        System.out.println(Math.round(10.349));
    }
}
```

输出结果为将浮点数的小数部分四舍五入后得到的结果：

```
10
```

**random**

```java
package com.company;

import java.util.Scanner;

public class Main {

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        System.out.println((int)(Math.random()*100));
    }
}
```

这是随机产生一个范围为0到1的浮点数，可以通过以上代码得到随机范围内的整数。
**pow**

```java
package com.company;

import java.util.Scanner;

public class Main {

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        System.out.println(Math.pow(2, 20));
    }
}
```

pow是计算某个数的某次方，计算的类型是浮点数：

```
1048576.0
```

## 输入字符串

==in.next()== ：读入一个单词，结束的标志是空格（包括空格，tab，回车）。

==in.nextLine()== ：读入一整行。

## 字符串操作

**比较两个字符串是否相等：** `s1.equals(s2)`

**比较两个字符串的大小：** `s1.comnpareTo(s2)` ，输出结果是数字，负数，正数，或者零。其本质是将两个字符串对应的Unicode码分别相加再将二者相减。

**访问字符串中的单个字符：** `s.charAt(index)` ，返回在index上的单个字符，index的范围是0到length()-1，第一个字符的index是0,与数组相同。但是不能用for-each循环来遍历字符串。

**得到子串：** `s.substring(n)` ，得到从n 号位置到末尾的全部内容；`s.substring(b,e)` 得到从b 号位置到e 号位置之前的内容。

**寻找字符：** `s.indexOf(c)` ，找到字符c 所在的位置，-1 表示不存在；`s.indexOf(c,n)` ，从n号位置开始寻找c ；`s.indexOf(t)` ，找到字符串t 所在的位置；`s.lastIndexOf(c)` , `s.lastIndexOf(c,n)` , `s.lastIndexOf(t)` ，从==右边==开始找。返回值都是所找到的位置。

当需要寻找的字符存在数量不止一个时，可用以下代码将其全部找出：

```java
package com.company;

import java.util.Scanner;

public class Main {

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        String s = new String("12455323412198467182371263142867351923107241");
        for (int i = -1; i < s.length(); )
        {
            i = s.indexOf('2', i+1);
            if (i == -1)
                break;
            System.out.println(i);
        }
    }
}
```

输出结果为：

```
1
6
10
19
23
28
36
41
```

## 成员函数与成员变量

成员函数调用自身的成员函数时会自动带上`this` 。

Java中本地变量未被赋予初始值则不能被使用。

构造函数没有返回类型。

构造函数可以重载。

```java
package diyizhou;

import java.util.Scanner;

public class Main {

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        Fraction a = new Fraction(in.nextInt(), in.nextInt());
        Fraction b = new Fraction(in.nextInt(),in.nextInt());
        a.print();
        b.print();
        a.plus(b).print();
        a.multiply(b).plus(new Fraction(5,6)).print();
        a.print();
        b.print();
        System.out.println(a.toDouble());
        in.close();
    }
}

class Fraction {

    int denominator = 0;
    int numberator = 0;

    Fraction(int numberator, int denominator)
    {
        if (denominator > 0){
            this.denominator = denominator;
            this.numberator = numberator;
        }
        else{
            System.out.println("分母必须为大于零的数！");
            System.exit(0);
        }
    }
    double toDouble()
    {
        return numberator*1.0/denominator;
    }
    Fraction plus(Fraction r)
    {
        if (r.denominator != this.denominator)
        {
            return new Fraction(this.numberator*r.denominator + this.denominator*r.numberator, this.denominator*r.denominator);
        }
        else
        {
            return new Fraction(this.numberator + r.numberator, this.denominator);
        }
    }
    Fraction multiply(Fraction r)
    {
        return new Fraction(this.numberator*r.numberator, this.denominator*r.denominator);
    }
    void print() {
        int min;
        if (this.denominator != 0 && this.numberator == this.denominator) {
            System.out.println("1");
            return;
        }
        else if (numberator == 0)
        {
            System.out.println("0");
            return;
        }
        else
        {
            min = Math.min(this.numberator, this.denominator);
        }
        if (min > 1){
            for (int i = 2; i <= min; i++) {
                if (this.numberator % i == 0 && this.denominator % i == 0)
                {
                    this.numberator /= i;
                    this.denominator /= i;
                    i = 2;
                }
            }
        }
        System.out.println(this.numberator + "/" + this.denominator);
    }
}
```

## 对象的交互

通过同一个类创建了两个对象，那么两个对象应该是相互独立的，为了使两个对象有交互作用，就必须有“第三只手”来操作，那便是使一个新的类包含这两个对象，也就是说在新类中创建两个需要使用的对象，通过新类来在操作两个对象的交互。

```java
package clock;

public class Display {

    private int value = 0;
    private int limit = 0;
    Display(int limit){
        this.limit = limit;
    }

    public void increase()
    {
        value ++;
        if (value == limit)
        {
            value = 0;
        }
    }

    public int getValue(){
        return value;
    }

    public static void main(String[] args) {
     Display h = new Display(24);

        for (;;)
        {
            h.increase();
            System.out.println(h.getValue());
        }
    }
}
```

```java
package clock;

public class Clock {
    private Display hour = new Display(24);
    private Display minute = new Display(60);

    public void start ()
    {
        while (true){
            minute.increase();
            if ( minute.getValue() == 0)
            {
                hour.increase();
            }
            System.out.printf("%02d:%02d\n", hour.getValue(), minute.getValue());
        }
    }
    public static void main(String[] args)
    {
        Clock clock = new Clock();
        clock.start();
    }
}
```

## 访问权限

成员变量应当是私有的，用以保护成员变量不被外界随意的修改，让其按照类的设计者的意图运作。

==private== ：只类内可以访问，但同一个类的不同对象也能访问，类内指的是类的成员函数和定义初始化。

==public== ： 随意访问。

==friendly== ： 未加访问权限，则默认是friendly，意思是只允许在**同一个包**的其他类访问。

public的类可以在别的编译单元中使用。

public的类必须在自己的文件中，即文件名与类名必须相同。

一个编译单元中只能有一个类是public，这是为了让每一个编译单元都有单一的公共接口，用public类来表现，该接口可以按要求包含众多支持包访问权限的类。

## Static

==static== ：JAVA中的类变量与C++中的静态成员变量相似，类变量属于类而不属于任何一个单一的对象，同一个类的类变量改变之后，无论以那种方式访问类变量，其值都是一样的。

* 函数前的static意为此函数不属于任何对象，而是属于这个类。static函数中可以直接调用或使用对象其他的static函数，但对于非static函数，只能通过对象调用。但是在static函数中调用的static函数里也不能直接访问非static成员。
* 类函数中没有this, 因为this是用于标识究竟是哪个对象在调用或访问。

## 继承与多态

Java中的对象变量都是多态的，能保存不止一种类型的对象。比如声明类型的对象或者其子类的对象。
事实上，多态的本质是为了解决类型嵌套的问题。
==向上造型==： 当把子类的对象赋给父类的变量的时候，就发生了向上造型。

## 造型

子类的对象可以赋值给父类的变量，但父类的对象不能赋值给子类的变量。
造型时并没有转换类型，而是指针重新指向另一个类型。
