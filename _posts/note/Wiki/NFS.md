---
title: NFS
date: 2023-05-25 17:34:01
categories: Wiki
tags: [Protocol, Linux]
comment: true
---

## Introduction

一种在linux之间共享文件的协议

nfs把远程机器上的文件数据以挂载的形式映射在本地用户机器上，所以nfs类似于windows的共享文件夹。
nfs通过port传输数据，但端口是随机选择的，因此nfs通过rpc服务注册端口，实现告知用户nfs的端口号。
RPC服务记录每一个NFS功能对应的端口号，并且告诉客户端。（像一个中介）

<!--more-->

## Installation

```bash
sudo pacman -S nfs-utils rpcbind
```

## Configure

**C/S模式：** client/server模式
Server端：

```bash
sudo pacman -S nfs-utils rpcbind
sudo chmod -Rf 777 /home/dionysen

# configure
sudo vim /etc/exports
# add the following parameters
home/dionysen  *(insecure,rw,sync)
# 共享目录+客户端地址（可以是**主机名、通配符和ip地址**）+权限参数
```

**权限参数：**

| parameters     | 说明                                                       |
| -------------- | ---------------------------------------------------------- |
| rw             | 读写                                                       |
| ro             | 只读                                                       |
| root_squash    | 客户端以root身份访问时，映射为匿名用户nobody               |
| no_root_squash | 直接以root身份挂载（比较危险，很不常用）                   |
| all_squash     | 所有用户都映射为匿名用户很安全常用                         |
| sync           | 数据同步写入到内存和磁盘，优点是保证内存数据安全，但效率低 |
| async          | 数据先写入内存，再持久化到磁盘，效率高，但有数据丢失的隐患 |

```bash
sudo systemctl enable --now rpcbind 
ll -d /home/dionysen
# 应为:
drwxrwxrwx 1 dionysen dionysen 1.3K Nov  4 13:24 .
# 若为root root,则需修改所属:
chmod -R dionysen.dionysen /home/dionysen
```

## Usage

Client端：

```bash
# 远程挂载:
sudo mount -t nfs 82.157.246.225:/home/dionysen/hexo /mnt/hexo
```
