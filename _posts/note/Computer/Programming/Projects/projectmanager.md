---
title: Project Manager
date: 2023-05-25 17:34:01
categories: [Programming, Project]
tags: [CPP]
comment: true
sticky: 99
---

**需求：**

- 可以创建以 CMake ＋ make 为构建工具的 C++项目
- 可以添加或删除 C++ 类，自动生成 `.h` 和 `.cpp` 文件，并补全必要代码
- 可以使用命令进行构建和运行项目
- 可以读取配置文件信息，如果没有，会初始化创建一个配置文件，配置文件信息包括：项目的路径，该路径中的所有项目，指定当前项目

<!-- more -->

### 命令行参数及作用

| long arg          | arg    | do                         |
| ----------------- | ------ | -------------------------- |
| `--list`          | `-l`   | show the whole inforamtion |
| `--createproject` | `-c`   | create project             |
| `--delproject`    | `-d`   | delete a prject            |
| `--addclass`      | `-a`   | add class                  |
| `--delclass`      | `none` | delete a class             |
| `--build`         | `-b`   | build without run          |
| `--run`           | `-r`   | build and run              |
| `--setproject`    | `-s`   | set the current project    |
| `--setpath`       | `none` | set the project path       |
| `--help`          | `-h`   | show help information      |

### 项目地址

<https://gitee.com/sential/projectmanager>

### 安装脚本

```bash
#!/bin/bash
cd
rm -rf ./projectmanager
git clone git@gitee.com:sential/projectmanager.git
cd projectmanager/build
rm -rf ./*
cmake ..
make
sudo cp ./pm /usr/bin/pm
```

### 过程中所用技巧（需整理到cpp中

#### 命令行参数

使用`getopt()`函数，原型为：`int getopt(int argc, char *const *argv, const char *shortopts)`

或如果需要长参数，使用getopt_long()函数，原型为： `int getopt_long(int argc, char *const *argv, const char *shortopts, const struct option *longopts, int *longind)`

使用方法是先创建三个参数，分别为命令行参数`opt`用以判定参数到底是哪一个，参数索引`option_index`，和长参数结构体数组本身`long_options[]`

其中`long_options[]`的`option`类型的结构的，原型为：

```cpp
struct option {
    const char *name;       // name is the name of the long option.
    int         has_arg;    //has_arg is: no_argument (or 0) if the option does not take an argument;
                            //required_argument (or 1) if the option requires an argument; 
                            //or optional_argument (or 2) if the option takes an optional argument.
    int        *flag;       //flag specifies how results are returned for a long option. If flag is NULL, then getopt_long() returns val. (For example, the calling program may set val to the equivalent short option character.) Otherwise, getopt_long() returns 0, and flag points to a variable which is set to val if the option is found, but left unchanged if the option is not found.
    int         val;        //val is the value to return, or to load into the variable pointed to by flag.
    // The last element of the array has to be filled with zeros.
};
```

**创建参数变量：**

```cpp
 int opt, option_index = 0;
    option long_options[] = {
        {"create", 1, nullptr, 'c'},  {"addclass", 1, nullptr, 'a'},
        {"setproj", 1, 0, 's'},       {"delproj", 1, nullptr, 'd'},
        {"list", 0, nullptr, 'l'},    {"build", 0, nullptr, 'b'},
        {"run", 0, nullptr, 'r'},     {"help", 0, nullptr, 'h'},
        {"setpath", 1, nullptr, 'S'}, {"delclass", 1, nullptr, 'D'},
    };
```

然后创建一个获取参数的循环，使用函数`getopt_long`不断的获取参数，用`switch`判断参数是哪个，然后执行相应的动作

```cpp
 while ((opt = getopt_long(argc, argv, "lbra:c:hd:s:D:S:", long_options,
                              &option_index)) != -1) {
        switch (opt) {
        case 'l':
            pm.list();
            break;
        case 'c':
            pm.setCurrentProject(optarg);
            pm.createProject();
            break;
        case 'a':
            pm.addClass(pm.currentProject, optarg);
            break;
        case 'b':
            pm.buildWithoutRun();
            break;
        case 'r':
            pm.run();
            break;
        case 'd':
            pm.delProject(optarg);
            break;
        case 'h':
            pm.showHelp();
            break;
        case 's':
            pm.setCurrentProject(optarg);
            break;
        case 'S':
            pm.setDefaultPath(optarg);
            break;
        case 'D':
            pm.delClass(optarg);
            break;
        }
    }
```

#### 打印彩色字符

| code    | 效果                                         |
| ------- | -------------------------------------------- |
| \033[1m | 让输出的字符高亮显式                         |
| \033[3m | 输出斜体字                                   |
| \033[4m | 给输出的字符加上下划线                       |
| \033[5m | 让输出的字符闪烁显式                         |
| \033[7m | 设置反显效果，即把背景色和字体颜色反过来显示 |
| 30      | 表示黑色                                     |
| 31      | 表示红色                                     |
| 32      | 表示绿色                                     |
| 33      | 表示黄色                                     |
| 34      | 表示蓝色                                     |
| 35      | 表示紫色                                     |
| 36      | 表示浅蓝色                                   |
| 37      | 表示灰色                                     |
| 40      | 表示背景为黑色                               |
| 41      | 表示背景为红色                               |
| 42      | 表示背景为绿色                               |
| 43      | 表示背景为黄色                               |
| 44      | 表示背景为蓝色                               |
| 45      | 表示背景为紫色                               |
| 46      | 表示背景为浅蓝色                             |
| 47      | 表示背景为灰白色                             |
| 0       | 清除所有格式                                 |

#### Use bash by String

```cpp
std::string cmd =
    "cd " + projectPath + "; mv -f " + projectName + " ./.dion_trash";
system(cmd.c_str());
```
