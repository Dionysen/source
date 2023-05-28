---
title: Xpra on Linux 的安装与使用
date: 2023-05-25 17:34:01
categories: Wiki
tags: [xpra, x11-forward]
comment: true
---



运行在浏览器上的远程桌面<!--more-->

## 安装

archlinux

```bash
sudo pacman -S xpra
```

ubuntu

```bash
sudo apt install ca-certificates
sudo wget -O "/usr/share/keyrings/xpra-2022.gpg" https://xpra.org/xpra-2022.gpg
sudo wget -O "/usr/share/keyrings/xpra-2018.gpg" https://xpra.org/xpra-2018.gpg
# For older distributions:
wget -q https://xpra.org/xpra-2022.gpg -O- | sudo apt-key add -
wget -q https://xpra.org/xpra-2018.gpg -O- | sudo apt-key add -

cd /etc/apt/sources.list.d;wget https://xpra.org/repos/jammy/xpra.list # ubuntu 22.04 doesn't work
cd /etc/apt/sources.list.d;wget https://xpra.org/repos/focal/xpra.list # ubuntu 20.04

apt update;apt install xpra
```

CentOS

```bash
sudo wget -O /etc/yum.repos.d/xpra.repo https://xpra.org/repos/CentOS/xpra.repo
sudo yum install -y xpra
```

### 使用

- 可以直接打开远程主机的程序

```bash
xpra start ssh:user@host --exit-with-children --start-child=<command>
```

- 可以开启服务监听，在远程浏览器上打开

```bash
xpra start --bind-tcp=0.0.0.0:4000
```

#### 使用 systemd 设置 html5 服务开机自启

编辑服务配置文件

```bash
sudo vim /etc/systemd/system/xpra@.service
```

为

```bash
[Unit]
Description=xpra-html5-server

[Service]
Type=simple
User=%i
EnvironmentFile=/etc/conf.d/xpra
ExecStart=/usr/bin/xpra --no-daemon start ${%i} --bind-tcp=0.0.0.0:4000

[Install]
WantedBy=multi-user.target
```

Now create the configuration, adding a line for each username you want to have an xpra display:

```bash
sudo vim /etc/conf.d/xpra
# 添加
dionysen=:7
```

允许开机自启：

```bash
sudo systemctl enable --now xpra@dionysen

# 检查服务运行情况
sudo systemctl status xpra@dionysen
```

- 可能需要安装一个桌面环境（可以使用 [[Wiki/LinuxNote#Tmoe 脚本|Tmoe]] 最小安装）
