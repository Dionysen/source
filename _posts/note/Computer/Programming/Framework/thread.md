---
title: 多线程基础
date: 2023-05-25 17:34:01
categories: [Programming, Framework]
tags: [CPP]
comment: true
sticky: 99
---



> **多线程**（英语：**multithreading**），是指从软件或者硬件上实现多个线程并发执行的技术。具有多线程能力的计算机因有硬件支持而能够在同一时间执行多于一个线程，进而提升整体处理性能。具有这种能力的系统包括对称多处理机、多核心处理器以及芯片级多处理（Chip-level multithreading）或同时多线程（Simultaneous multithreading）处理器。			——WIki-Pedia

<!--more-->

查看进程

```bash
ps -ef | grep <name>
```

查看线程

```bash
ps -xH | grep <name>
```

查看端口

```bash
netstat -ant | grep 8080
```

```cpp
pthread_exit(0);//使用pthread_exit(0);或者return (void*)0;结束当前线程，而非return;
// 使用exit(0)会结束当前进程（进程中所有的线程都会终止
```

多线程参数传递可以使用强制类型转换：

```cpp
int i = -10;
void *ptr = (void*)(long)i;
int j = (int)(long)ptr; //此时j == i == -10
```

#### 线程资源的回收

线程有两种状态，jonable和unjoinable，如果为前者，子线程主函数终止时（自己退出或pthread_exit(0)），资源不会被释放，这种线程称为僵尸线程。

创建线程时，默认为joinable.

资源回收有四种方法：

1. 在主线程中调用pthread_join(),但一般不用，因为会发生阻塞
2. 创建线程前，调用`pthread_attr_setdetachstate`设置属性

```cpp
pthread_attr_t attr;
pthread_attr_init(&attr);
pthread_attr_setdetachstate(&attr, PTHREAD_CREATE_DETACHED);
pthread_create(&pthid, &attr, mainFund, NULL);
```

3. 创建之后，设置为detached状态：`pthread_detach(pthid);`
4. 在线程主函数中调用`pthread_detach`：`pthread_detach(pthread_self());`

可以使用`pthread_join(pthid, (void**)&i)`来获取线程结束后的返回值。在线程主函数中使用`return (void *) number`来设置线程的返回值，可以告诉主线程子线程的情况。 

#### 线程的取消

线程的取消：**int** **pthread_cancel(pthread_t** thread**);** 被取消的线程返回值为`-1`

可以在子线程的主函数中使用函数设置是否可以被取消：

```cpp
pthread_setcancelstate(PTHREAD_CANCEL_ENABLE, NULL);
//or
pthread_setcancelstate(PTHREAD_CANCEL_DISABLE, NULL);
//也可以保留旧的状态：
int oldState;
pthread_setcancelstate(PTHREAD_CANCEL_DISABLE, &oldState);

//线程创建时默认为可以取消
```

子线程中调用`pthread_setcanceltype`改变取消的方式：

```cpp
pthread_testcancel(); // 设置取消点
pthread_setcanceltype(PTHREAD_CANCEL_ASYNCHRONOUS, NULL); // 立即取消
pthread_setcanceltype(PTHREAD_CANCEL_DEFERRED, NULL); // 延时取消，延时到取消点再取消
```

#### 线程清理

子线程退出时需要释放资源、锁和回滚事务等；

善后的代码一般写在清理函数中，清理函数必须成对出现：

```cpp
void cleanfunc(void *arg) {/*Do something*/}

//在子线程中
pthread_cleanup_push(cleanfunc, NULL);
pthread_cleanup_pop(1);
```

线程的信号处理函数只能有一个，所有线程收到信号都会调用这一个。

### 线程池

C11封装了线程库`thread`.

构造函数：

```cpp
template<typename _Callable , typename... _Args, typename  = _Require<__not_same<_Callable>>> std::thread::thread (_Callable && __f, _Args &&... __args) [inline],  [explicit]
```

线程同步：多个线程协商如何合理的使用资源。

递归互斥锁允许线程多次申请加锁，可以解决同一线程多次加锁造成的死锁的问题。