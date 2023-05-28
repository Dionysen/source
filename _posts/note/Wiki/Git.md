---
title: Git
date: 2023-05-25 17:34:01
categories: Wiki
tags: [Github, Linux]
comment: true
---

分布式**版本控制系统**，适合个人中小企业使用

<!--more-->

## Installation

```bash
sudo pacman -S git
```

## Usage

配置git 的用户名和邮箱：

```bash
git config --global user.name "dionysen"
git config --global user.email "solongnight@outlook.com"
```

Initiate git repository on the local:

```bash
git init  
```

or set the file path:

```bash
git init path/to/repo
```

A repository was created, but it is empty.
You can add some files to the repository:

```bash
git add [filename]  // e.g. "git add ."
```

Then you add this files to the stages and you need to commit this to the repository.

```bash
git commit -a -m "Changed some files"
```

`-a` does not commit any new files.
`-m` means that you should give the commit message.
Add a remote repository:

```bash
git remote add origin git@gitee.com:sential/source.git
```

Push the local repository to the remote repository:

```bash
git push origin master
```

- 若要在一个新的设备上使用远程仓库，首先将此仓库克隆到本地：

```bash
git clone git@gitee.com:sential/source.git

# 值得注意的是gitee的仓库公钥管理方式导致必须使用ssh克隆，否则难以实现无密码修改远程仓库

# 官方提示：使用SSH公钥可以让你在你的电脑和 Gitee 通讯的时候使用安全连接（Git的Remote要使用SSH地址）
```

然后按照 `gitee` 上的提示添加个人公钥:

```bash
ssh-keygen -t ed25519 -C "xxxxx@xxxxx.com"  
# Generating public/private ed25519 key pair...

cat ~/.ssh/id_ed25519.pub
# ssh-ed25519 AAAAB3NzaC1yc2EAAAADAQABAAABAQC6eNtGpNGwstc....

ssh -T git@gitee.com
```

每次编辑时要执行。

```bash
git pull origin master

# 然后开始编辑
# 完成后执行：

git add .
git commit -a -m "Changed some files"
git push origin master
```

或者每次编辑完成后，在另一处`pull`一次，那样不用每次编辑前都要再拉去一下了。
写两个脚本自动拉取和提交。

- 当没有拉取最新版本的远程仓库同时又修改了本地仓库时，拉取会提示错误，需要选择合并或者放弃某一端，如果放弃本地仓库，执行以下命令：

```bash
git reset --hard
git pull origin master
```
