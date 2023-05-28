---
title: OpenGL开发环境搭建
date: 2023-05-25 17:34:01
categories: Wiki
tags: [OpenGL, Linux, Windows]
comment: true
---

<!--more-->

# OpenGL

### Archlinux配置OpenGL开发环境

- GLFW

安装`glfw`库

```bash
sudo pacman -S glfw-x11
```

- GLAD

<img src="https://raw.githubusercontent.com/Dionysen/BlogCDN/main/img/gladv.png" style="zoom:80%;" />

在此网站选择需要的版本[https://glad.dav1d.de](https://glad.dav1d.de/)，点击`GRNERATE`，下载生成的zip文件，解压后将其放到项目文件夹中。

文件目录为：

```bash
├── CMakeLists.txt
├── glad
│   ├── include
│   │   ├── glad
│   │   │   └── glad.h
│   │   └── KHR
│   │       └── khrplatform.h
│   └── src
│       └── glad.c
└── main.cpp
```

`CMakeLists.txt`可以写成如下：

```cmake
cmake_minimum_required(VERSION 3.14)
project(OpenglTest)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON) # Make sure clang can find .h file

set(CMAKE_CXX_STANDARD 11)
set(SOURCE_FILES main.cpp glad/src/glad.c) # src files

include_directories(glad/include) # include files

add_executable(OpenglTest ${SOURCE_FILES})

target_link_libraries(OpenglTest glfw) # link the glfw library
```

### Visual Studio on Windows 配置 OpenGL 开发环境

- GLFW（手动编译，没有必要）

下载CMake（x64）：https://github.com/Kitware/CMake/releases/download/v3.26.4/cmake-3.26.4-windows-x86_64.msi

下载GLFW源码：https://github.com/glfw/glfw/releases/download/3.3.8/glfw-3.3.8.zip，并解压

打开CMake-GUI，设置如下：

 <img src="https://cdn.jsdelivr.net/gh/Dionysen/BlogCDN@main/img/image-20230525010644603.png" style="zoom:80%;" />

点击Configure，选择自己需要的VS版本和架构：

<img src="https://cdn.jsdelivr.net/gh/Dionysen/BlogCDN@main/img/image-20230525010811588.png" style="zoom:80%;" />

点击Generate，会源文件中生成一个`build`文件夹，用 VS 打开其中的`GLFW.sln`，生成，然后将生成的`dll`文件放置好

- GLFW

[直接下载Windows版本的预编译文件](https://www.glfw.org/download.html)，其中有`includes`和`dll`文件，链接到项目即可使用

- GLAD

与Linux版本相同，均为将源代码文件包含到项目中