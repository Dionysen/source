---
title: C Language
date: 2022-05-25 17:34:01
categories: [Programming, Language]
tags: [c]
comment: true
sticky: 9999
---

<img src="https://cdn.jsdelivr.net/gh/Dionysen/BlogCDN@main/img/code_by_rasmusir-d4a4dj2222.jpg" alt="code_by_rasmusir-d4a4dj2222" style="zoom: 50%;" />

> 📚  Notes and *personal understanding* of the process of learning **C language**, used to find.

<!-- more -->

## Introduction

> Our daily life has become inseparable from computers. Whether you are using computers or not, you are using computers consciously or unconsciously, or using the services that computers provide for you. When we are in the use of computer, we are all in the use of computer has some software, so we will go to find the APP, if you searched all the APPs on the market, is there no have the functionality of the APP you want, then you have to write their own one, if you want to do something special, you can't find the right software, will still have to write their own one.
> Learning programming is not about writing software for yourself. It is about learning programming to understand how computers work, what they can or are good at doing, what they can't or aren't good at doing, and how computers solve problems.

<p align="right"> —— Weng Kai</p>



## Get started

### Framework

```c
#include "stdio.h"
int main()
{

    return 0;
}
```

Any programs programed by C language must have this framework.

### Output function

You can understand it as function in math, which is a mapping relationship. But they are different.

`printf` is a function, whoes function is output a string by formating `printf("......\n")` .

For example, the `Hello, world!` :

```c
#include "stdio.h"
int main()
{
    printf("Hello,world!\n"); // \n make it wrapping
    return 0;
}
```

`printf`  can print not only a string, but also the value of the variable, but you need to format the variable.

## Variables and constants

> The computer carries on the computation, then participates in the computation is the number, participates in the computation in the C language the number is called the quantity, the quantity divides into the variable and the constant.
> Use decimal for expression of daily life, because is advantageous for the calculation of the human brain, a computer internal use binary, for convenience of computer calculation, and the computer expression, as a result of **bytes** in computer internal frequency is higher, if you can use a simple way to express its inner meaning accurately, Will bring us a lot of convenience, so often use hexadecimal expression.
> But the number itself remains the same no matter which way it is counted.

### Constants

As the name implies, an invariant quantity that, once initialized, cannot be changed.

### Variables

As the name implies, a variable quantity that, once defined, can be assigned any value to change its size.

**The way of difination：**

```c
int i;
int j = 1;
char k;
float h = 1.2;
double g = 2.0;
```

For example, `i` is the variable itself, `int` is an **integer variable**, whose value can only be an integer, while `double` is a **double-precision floating point number**, which can represent a decimal.

Different variable types have different value types and value ranges.

Character variables:

Use to store **character constants**. A character variable can hold **only** **one** character constant. The type specifier is `char`.

```c
#include<stdio.h>
int main()
{
    char x,y,z;
    x = 'b';
    y = 'o';
    z = 'y';
    printf("%c%c%c\n",x,y,z);
return 0;
}
```

The result is:

```bash
boy
```

The literal value of a character variable is independent of the character constant it holds, analogous to an integer variable.

Character variables can also store integer data, which is universal. You can change `%c` to `%d` during input and output.

## Output and input of a variable

### Output

As mentioned above, `printf` can print a string, and can print the value of a variable, as shown in the following example:

```c
#include "stdio.h"
int main()
{
    int i = 1;
    printf("i = %d\n",i);
    i = 2;
    printf("After assignment，i = %d\n",i);
    return 0;
}
```

Notice that printf prints the value of the variable with a `%d` inside the double quotes, which is the way the variable is formatted.

 `%d` indicates that the output variable is an integer.

| Variable Types     | Formatting Symbols |
|:------------------ |:------------------ |
| int                | %d                 |
| unsigned           | %u                 |
| long long          | %ld                |
| unsigned long long | %lu                |
| float              | %f                 |
| double             | %lf                |

You can use **scientific notation** when you output, and use `%e` for formatting symbol.

```c
printf("%.nf",sum);
```

This line of code can **preserve n decimal places.**

The following are **escape characters**:

| Symbols | Words     | 中文含义   |
|:-------:|:---------:|:------:|
| \b      | backspace | 回退一格   |
| \t      | tab       | 下一个制表位 |
| \n      | new line  | 换行     |
| \r      | return    | 回车     |
| \f      | f         | 换页     |

### Input

Similarly, the function `scanf` can read the input according to a certain format.

```c
#include <stdio.h>
int main( ) {

   char str[100];
   int i;

   printf( "Enter a value :");
   scanf("%s %d", str, &i);

   printf( "\nYou entered: %s %d ", str, i);
   printf("\n");
   return 0;
}
```

`scanf()` stops reading a string as soon as it encounters a space, so "this is test" is three strings for `scanf()`.

## Floating point numbers

In mathematics, the numbers on the number line are continuous, and between any two different points, an infinite number can be found, but this is difficult to achieve in computers, so floating point numbers emerged.

Floating point numbers are used to represent fractional numbers between whole numbers, but their accuracy is not infinite, nor is their expressability infinite, so a random decimal ***may not be able to*** be expressed by a computer.

## Operation

### Operator

| Mathematics    | Add | Substract | Multiply | Divide | Remainder |
|:--------------:|:---:|:---------:|:--------:|:------:|:---------:|
| **C Language** | +   | -         | *        | /      | %         |

### Relational operator

| Relation | Equal | Not Equal | Greater | Greater or Equal | Less-than | Less-than or Equal |
|:--------:|:-----:|:---------:|:-------:|:----------------:|:---------:|:------------------:|
| Operator | ==    | !=        | >       | >=               | <         | <=                 |

The relational operator evaluates only zeros and ones.

### Special operator

**`count ++`** and **`++ count`** both mean to add one, but `a = count ++;` means to assign the value of `count` to `a` and then add one, whereas `a = ++count;` means to add one to the value of `count` and then assign the result to `a`. So you end up adding one to `count` in both cases, but the value of `a` differ by `1`.

**`count --`** and **`-- count`** in the same way.

**`,`** is comma operator that generally has only one purpose: to add multiple conditions to an `if` statement.

### Conditional operator

```c
count = (<#condition#>)? <#yes#>:<#no#>;
```

It is equivalent to an `if` statement.

> Nesting is not recommended.

### Logical operator

| Logic  | and | or   | not |
|:------:|:---:|:----:|:---:|
| Symbol | &&  | \|\| | !   |

The result of logical operation is only `0` or `1`.

## Several statements

### `if`

```c
  if (<#condition#>) {
        <#statements#>
    }
```

To judge and to act when the conditions are true.

```c
    if (<#condition#>) {
        <#statements#>
    }
    else if (#condition#) {
        <#statements#>
    }
 else if (#condition#) {
        <#statements#>
    }
  ……
    else {
        <#statements#>
    }
```

We can add `else`, so we can do something if the condition doesn't work.

The `else` always matches the nearest `if`.

### `while`

```c
    while (<#condition#>) {
        <#statements#>
    }
```

The loop continues until the condition fails.

### `dowhile`

```c
    do {
        <#statements#>
    } while (<#condition#>);
```

The loop continues until the condition fails.

The difference with a `while` loop is that a `dowhile` does something and then evaluates the condition, whereas a `while` evaluates the condition and then loops. **`While` might not do a loop at all, if the condition is not satisfied in the first place.**

### `switch`

```c
    switch (<#expression#>) {
        case <#constant#>:
            <#statements#>
            break;
       case <#constant#>:
            <#statements#>
            break;
        ......
        default:
            break;
    }
```

`switch` is judgment statement, the `<#expression#>`  is constant expression that must be a ***integral type*** or ***enum-type***.

The essence of such a statement is the program evaluates this expression and then compares it to each `case` at a time. The action after the `case` is executed when equal.

There are an infinite number of cases, each followed by a value to be compared with and a colon.

The variables to be compared must be of the same type.

When all the `case` is false, the program will do the action after `default` . So there can be nothing after `defalut`.

For example:

```c
#include <stdio.h>
int main(){
    int a;
    printf("Input integer number:");
    scanf("%d",&a);
    switch(a){
        case 1: printf("Monday\n"); break;
        case 2: printf("Tuesday\n"); break;
        case 3: printf("Wednesday\n"); break;
        case 4: printf("Thursday\n"); break;
        case 5: printf("Friday\n"); break;
        case 6: printf("Saturday\n"); break;
        case 7: printf("Sunday\n"); break;
        default:printf("error\n"); break;
    }
    return 0;
}
```

### `for`

```c
    for (<#initialization#>; <#condition#>; <#increment#>) {
        <#statements#>
    }
```

`for` loop applies to loops with a defined number of cycles, such as **traverse**.

There are three sections in parenthesis, separated with semicolons, which are respectively **initialization**, **conditions** for loop to proceed and **actions** to be performed in each cycle.

## Miscellaneous

| Key words              | Implication          |
| ---------------------- | -------------------- |
| `Inf`                  | Infinity             |
| `-Inf`                 | Negative infinity    |
| `nan`                  | Invalid number       |
| `fabs(<#expression#>)` | Absolute value       |
| `break`                | Jump out of the loop |
| `continue`             | End the cycle        |

## Function and customizing function

At the beginning of C language program, the implication of `#include <stdio.h>` is including **a function library** named `stdio.h` and then the program can call functions in the library. Both the `printf` and the `scanf` used in the previous paragraph are functions of the library.

In practice, we often encounter repeated operations, we can copy this code to complete the repeated action, but **code copy is a poor quality of the program**, because the ***maintenance*** may need to change too many places.

You can solve this problem by customizing functions:

```c
  <#type#> (<#type#>,<#type#>,……){
    <#statement#>
    return 0; //Depends on the function type，Also visable as：return;
  }
```

- A function can have multiple `return`  or none. However, multiple `return` are not recommended for easy modification.

- Each function has its own **variable space**, namely `{}` (block), which is independent of each other. **Local variables** are limited by the block they are in. If the inside of a block has the same name as the outside of a block, ***the inside of a block takes precedence***.

- When a function is called, it can only pass **values** to functions, **not variables** to functions. That is, after passing a variable to a function, the function will read the value of the variable for operation, ***but will not change the value of the variable***.

- The first line of a function with a semicolon placed before the entire program code is called a **function prototype declaration**. The purpose is to tell the compiler what type the function is before it encounters it.

## Array

### Defination

> type of variables + character + [number of variables]

For example:

```c
int a[10];
```

An array is a container that, once created, cannot be resized, is internally ordered, and can appear on both sides of an assignment symbol.

The index of an array is counted from `0`.

You can think of it as a **sequence** in *mathematics*.

### Use

**Integration initialization** is easy to use:

```c
int a[3] = {1,3,5,};
int a[13] = {[0]=2,[3]=5,6,7,[9]=0,}; //（C99 only）
```

If you don't know how many cells there are in an array, you can use `sizeof(a)/sizeof(a[0])` to represent the number of cells in the array, so that you don't need to change the number of cells in the array.

### Multidimensional array

A multidimensional array is actually a multidimensional matrix, and the footer increases accordingly.

Initialization:

```c
int a[][5] = {
 {0,1,2,3,4},
 {2,3,4,5,6},
}
```

The number of **columns** must be given and the number of **rows** can be counted by the compiler itself.

## Pointer

### Address

Each variable has an address in the computer where it is stored. The value of a variable can change, but its address is **constant**. The following code can be used to view the address of a variable.

```c
int i = 1;
printf("%d\n",&i);
```

`&` is the address to access the variable;

`*` is the variable on the access address.

### Defination

A pointer is a variable, but it cannot be used independently. It must point to a variable. In this case, the value of the pointer variable is the address of the variable to which it points.

### Use

```c
int *p = &i;
```

In this case, `p` is a pointer to the address of variable `i`. So the value of `p` is the address of `i`, and the value of `i` can be **accessed** (read and write) by `*p`.

The `*` at definition is not the same as the `*` at access, and the first is only used to distinguish whether a pointer variable or a normal variable is being defined.

Here is an example of using a pointer to complete a call to exchange the values of two variables.

```c
#include <stdio.h>
void exchange(int *a,int *b)
{
    int i = *a;
    *a = *b;
    *b = i;
}
int main()
{
    int a = 5;
    int b = 6;
    exc(&a, &b);
    printf("a = %d,b = %d\n",a,b);

    return 0;
}
```

This is a clever use of the function, we know that the **function cannot input variable parameters**, so this code defines the address of the pointer to the variable, **the function input pointer variable is also the address of the variable**, inside the function by adding a pointer to access the variable, and then achieve the purpose of the function to modify the variable.

> In addition, pointers are often used when a function needs to return multiple values.

#### Arrays are special Pointers

```c
#include <stdio.h>
int main()
{
    int a[] = {1,2,3,};
    printf("%p\n",a);
    return 0;
}
```

The result of this code is:

```c
0x7ffcac420c3c
```

We can see that the array variable `a` is itself an address, so when we want to use a pointer to array `a`, we should write `int *p = a`, without `&`. But the array unit is variable, therefore `int *p = &a[0]`, at the same time, `a == &a[0]`,`&a[x] == &a[0] + 4x = a + 4x`(when `a` is integer).

An array is a pointer to a constant and therefore cannot be assigned.

#### Pointer to a constant (const)

```c
int i;
int const *p = &i; // 1
int *const p = &i; // 2
```

For the above code, you can think of the following code:

```c
int i;
int const (*p) = &i; // 1
int *(const p) = &i; // 2
```

1 indicates that the variable at the address pointed to by the pointer `p` cannot be modified by the pointer.

2 indicates that the address (variable's address certainly) pointed to by `p` cannot be changed.

> The first whole to the right of const cannot be modified

```c
const int a[] = {1,2,3,};
```

The above code indicates that **each cell** is `const`, so ***it can be used to protect an array when a function argument is entered***.

#### Address of a pointer

```c
int a[];
int *p = a;
```

`*p = a = a[0],`

`*(p+1) = a[1],`

`*(p+2) = a[2],`

`……,`

`*(p+n) = a[n]`.

## Allocating Memory Space

The malloc function applies space in **bytes** from memory, returns `void *`, converts the desired type, and finally frees the memory. Format, such as:

```c
int a[n] = (int *)malloc(n * sizeof(int));
```

If the application fails, `0` or `NULL` is returned.

When you no longer use the requested memory space, you should use the `free` function to free the memory space:

```c
free(a[n]);
```

## String

### Overview

```c
Char word[] = {'H','D','e','!'};
```

Such an array is an **array of characters**, but it is not a **string**, ***because it cannot be evaluated as a string***.

```
Char word[] = {'H','D','e','!','\0'};
```

Followed by `\0`, then word is a string.

`0` = `'\0'` != `'0'`

**`0` marks the end of the string, but this `0` is not part of the string.**

Strings exist as Arrays and are accessed as arrays or Pointers, but more often as Pointers.

`string.h` has a number of functions that handle strings.

String variables:

```c
char *str = "hello";
char word[] = "hello";
char line[10] = "hello";
```

> "hello"

The compiler will turn this into an array of characters somewhere, and the length of the array is 6, because the compiler will put a `0` after it to make it a string.

Literals of strings can be used to initialize character arrays.

```c
#include "stdio.h"
int main()
{
    int i = 0;
    char *s = "hello world";
//    s[0] = 'B';
    char *s2= "hello world";
    char s3[] = {"hello world"};
//    s3[0] = 'b';

    printf("&i = %p\n", &i);
    printf("s = %p\n", s);
    printf("s2 = %p\n", s2);
    printf("Here is s[0] = %c\n", s[0]);
    printf("Here is s3[0] = %c\n",s3[0]);
    return 0;
}
```

The above code runs as follows:

```c
&i = 0x7fffa827bcf4
s = 0x55c7d8f64004
s2 = 0x55c7d8f64004
Here is s[0] = h
Here is s3[0] = h
```

You can see that the local variable `i` is far from where the pointer `s` is pointing. The address of the variable `i` is very far back, and the pointer is very far forward. Near the front are important parts of the computer that are not allowed to be modified, such as plus `s[0] = 'B'` ', and the result is:

```
1869 segmentation fault  ./a.out
```

The program attempted to reassign the initialized string `s`. The error "**segmentation fault**" was reported, meaning that the program was attempting to rewrite the value at 0x55c7d8f64004, which posed a threat to the computer and was not allowed.

In fact, for `char *s = "hello world";` pointer `s` is initialized to point to a string constant, which should actually be `const char *s = "hello world";`, but for *historical reasons*, compilers accept writing without const, and try to modify the string to which `s` refers, with serious consequences.

`s3` is an array, and the strings inside are local variables that can be modified, plus `S3[0] = 'b'`:

```
&i = 0x7fff28d5dd64
s = 0x559da8ac6004
s2 = 0x559da8ac6004
Here is s[0] = h
Here is s3[0] = b
```

**There are two ways to define a string: pointer or array.**

Arrays know the address of strings, Pointers don't.

A `char *` or `int *` is not necessarily a string. It is meant to be a pointer to a character or an array of characters.

 A `char *` or `int *` is a string only if the array of characters to which the pointer points has a zero at the end.

```c
char *t = "title";
char *s;
s = t;
```

For the above code, we have two Pointers, `t` and `s`. First, `t` points to the string "title", and then we assign a value to `s`. The result is that `s` also points to the same string, instead of creating a new string.

### Input and output of string

For `printf` and `scanf`, you can use **`%s`** to input and output strings.

Each `%s` in `scanf` is read until a SPACE or ENTER, which is not safe because you do not know exactly how many characters to read, therefore, the following code is used:

```c
char s[8];
scanf("%7s",s);
```

### Array of strings

```c
char a[][10];
char *a[];
```

The first line refers to `a` as a two-dimensional array, and the second line refers to `a` as a pointer array. Each unit in the array is a pointer to a string.

Each element of the character array holds one character, plus the `0` at the end. An array of length `n` can hold `n-1` characters.

**Multiple loops are required to input and output all elements of multiple dimensions, whether string arrays or integer arrays.**

An array is a matrix that can be used to store variables or character variables. All input and output need to be looped, but there are two types of input and output for character arrays.

#### Input and output single characters in format `%c`

```c
#include <stdio.h>
int main()
{

    int x,i,j;
    char a[][20] = {
        "",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
        "Sunday",
    };
    printf("Please input the month：\n");
    scanf("%d",&x);
    if (x > 0 && x < 8) {
        for (i = x ; i < x + 1; i ++) {
            for (j = 0; j < 20; j ++) {
               printf("%c",a[i][j]);
          }
       }       
    }
    else printf("Error");
    printf("\n");
    return 0;
}
```

#### Input and output whole array with format `%s`

```c
#include <stdio.h>
int main()
{
    int x,i,j;
    char a[][20] = {
        "",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
        "Sunday",
    };
    printf("Please input the month：\n");
    scanf("%d",&x);
    if (x > 0 && x < 8) {
        printf("%s",a[x]);
    }
    else printf("Error");
    printf("\n");
    return 0;
}
```

> Note that input characters with `%s` that encounter a **SPACE, RNTER, and TAB** end the string input, so C provides the input function `gets()` and the output function `puts()` that are best for strings.

**`gets(char *s[])`** function is to enter a string from the keyboard that can contain Spaces and end with a ENTER newline character.

**`puts(char *s[])`** or `puts(string s)`function prints a string from the character array to the screen and converts the end-of-string flag to a newline character.

In addition, when `%s` prints a string, it keeps one dimension, and the compiler automatically inputs or outputs all strings in that dimension. Gets is the same as puts.

### Input and output of character data

#### `Putchar(parameter)`

Paremeters can be **numerical values**, character constants, **character variables**, and **arithmetic or character expressions**, but the final output is a character.

#### `getchar()`

Type a character from the keyboard.

### String function

#### `strlen`

```c
#include<stdio.h>
#include <string.h>
int main(int argc, char const *argv[])
{   
    char line[] = "hello";
    printf("%lu\n", strlen(line));
    printf("%lu\n", sizeof(line));
    return 0;
}
```

The running result is:

```
5
6
```

So `strlen` is string Length, which returns the length of the string.

#### `strcmp`

The function is to compare the size of two strings, and the result of the comparison is expressed by the value returned. `0` means they are equal, `1` means the former greater, and `-1` means the latter greater.

#### `strcpy`

It means string copy，the format is：

```c
char *strcpy(char *restrict dst, const *restrict src);
```

Its function is to copy the `src` string to `dst`.
`restrict` means that `src` and `dst` cannot overlap.
The source is in the back, and the copying destination is in the front.
Return `dst` so that the function itself can be evaluated.
General usage:

```c
char *dst = (char)malloc(strlen(src)+1);
strcpy (dst,src)
```

#### `strcat`

The function is to link one string to another string.

> **Another version:**
> 
> ```c
> char *strncpy(char *restrict dst, const *restrict src,size_t n);
> char *strncat(char *restrict s1, const *restrict s2,size_t n);
> int strncmp(const char *s1, const char *s2, size_t n);
> ```
> 
> The first two are to limit the length of the copied string, eliminating security issues that are neither out of bounds.
> 
> The last one is to compare the first `n` characters.

#### `strchr`

To find a character in a string.

```c
#include<stdio.h>
#include <string.h>
#include <stdlib.h>

int main(int argc, char const *argv[])
{
    char s[] = "hello,world!";
    char *p = strchr(s,'l');
    printf("%s\n",p);
    char c = *p;
    *p = '\0';
    printf("%c\n",c);
    char *p3 = (void*)malloc(strlen(s)+1);
    strcpy(p3, s);
    printf("%s\n",p3);
    free(p3);
    char *p1 = strchr((p+1), 'l');
    char *p2 = strchr((p1+1), 'l');
    printf("%s\n",p1);
    printf("%s\n",p2);

    return 0;
}
```

The running results are as follows:

```
llo,world!
l
he
lo,world!
ld!
```

## Enumeration

```c
enum type {num_0,num_1,num_2,……,num_n};
```

You can use the name in curly braces, where `num_0` through `num_n` represents the constants `0` through `n`.

For instance:

```c
enum colors {red,yellow,green};
// Here，red == 0, yellow == 1, green == 2 
```

```c
enum type {num_0,num_1,num_2,……,num_n, number of type};
```

That's just right. **The last number of type is exactly the number of type.** It's a little trick.

## Data structure

```c
#include <stdio.h>
struct date {
    int day;
    int month;
    int year;
};
//Structure type Declaration
struct date {
 int day;
 int month;
 int year;
} today;
//This is another form

int main()
{
    struct date today;

    today.day = 25;
    today.month = 3;
    today.year = 2021;
   today = (struct date){25,3,2021};

    printf("Today is %i-%i-%i.\n",
           today.year,today.month,today.day);

    return 0;
}
```

This means that you declare a data structure type, and when you use it, you define a variable that contains all the variables in the data structure.

The structure members of a data structure do not have to be of the same variable type, and an array can only be of one type.

Data structures can perform **structure operations** .

Assigning values to or between structure variables is a one-to-one correspondence; the former requires curly braces.

The name of the structure variable is not the address of the structure variable, so you need to define the pointer using `&`.

A data structure can be entered as a function parameter, but unlike an array, the entire structure is passed into the function as the value of the parameter, **creating a new structure variable** inside the function and copying the value of the caller's structure.

You can also return a `struct`.

## Custom data types

A `typedef` can give an **alias** to a data type.

```c
typedef long int64_t;
typedef struct ADate {
 int month;
 int day;
 int year;
} Date;

int64_t i = 100000000000;
Date d = {9, 1, 2005, };
```

## Union

带参数的宏一定要有括号，结尾不能加分号。
