---
title: WSL2
date: 2023-05-25 17:34:01
categories: Wiki
tags: [xpra, x11-forward]
comment: true
---

> ✅  This is a tutorial of installing  on WSL2

<!--more-->

## Install WSL2

### Start using WSL

Open `powershell` using administration rights, and input:

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

### Requirement of WSL2

For x64 system, the version of win10 must be 1903 or higher.
Using "win + R" and input `winver` to check.

### Start Virtual machinel platform

Open `powershell` using administration rights, and input:

```powershell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

### Install Linux Kernal Updating

 Download Link: <https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi>
Install.

### Setting the default version 2

Open `powershell` using administration rights, and input:

```powershell
wsl --set-default-version 2
```

Then, WSL2 is already installed.

## Update to WSLg

将win10更新到最新的版本

Open `powershell` using administration rights, and input:

```powershell
wsl --update
wsl --version
# display:
WSL version: 1.0.3.0
Kernel version: 5.15.79.1
WSLg version: 1.0.47
MSRDC version: 1.2.3575
Direct3D version: 1.606.4
DXCore version: 10.0.25131.1002-220531-1700.rs-onecore-base2-hyp
Windows version: 10.0.19045.2364
```

否则说明win10还不是最新的，继续更新

## Install Archlinux on WSL2

### Download Archlinux

Download link: <https://mirrors.tuna.tsinghua.edu.cn/archlinux/iso/latest/>
Find and Download  `archlinux-bootstrap-2020.10.01-x86_64.tar.gz` .

### Install Archlinux by LxRunOffline

#### 1. Input the command in powershell

```powershell
LxRunOffline i -n <自定义名称> -f <Arch镜像位置> -d <安装系统的位置> -r root.x86_64
```

example:

```powershell
LxRunOffline i -n ArchLinux -f C:\Users\dionysen\Downloads\archlinux-bootstrap-2020.10.01-x86_64.tar.gz -d C:\Users\dionysen\Linux -r root.x86_64
```

#### 2. Change WSL2 version in Archlinux

```powershell
wsl --set-version ArchLinux 2
```

### Configuration

#### Basic Configuration

```bash
wsl -d Archlinux
rm /etc/resolv.conf
exit
```

The terminal window will be unavailable, so you should reopen a new terminal window, then:

```bash
wsl --shutdown Archlinux
wsl -d Archlinux
cd /etc
vi pacman.conf
```

Add following code in the end of `pacman.conf`:

```bash
[archlinuxcn]
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```

And change the mirrorlist:

```bash
vi ./pacman.d/mirrorlist
```

**Remove the comment** of a Chinese source.

```bash
pacman -Syy
pacman-key --init
pacman-key --populate
pacman -S archlinuxcn-keyring
pacman -S base base-devel vim git wget

passwd # input the password of root
useradd -m -G wheel -s /bin/bash <username>
passwd <username>
vim /etc/sudoers
```

Use `/wheel` find the line `wheel ALL=(ALL) ALL` and remove the comment.

```bash
id -u <username>
exit
```

Execute the command in `powershell` to set default user of Archlinux:

```bash
lxrunoffline su -n Archlinux -v <username>
```

## Install Ubuntu20.02 in WSL2 

```powershell
wsl --list --online		# 查看可直接安装的发行版列表
# 显示如下：
PS C:\Windows\system32> wsl -l --online
The following is a list of valid distributions that can be installed.
Install using 'wsl.exe --install <Distro>'.

NAME               FRIENDLY NAME
Ubuntu             Ubuntu
Debian             Debian GNU/Linux
kali-linux         Kali Linux Rolling
SLES-12            SUSE Linux Enterprise Server v12
SLES-15            SUSE Linux Enterprise Server v15
Ubuntu-18.04       Ubuntu 18.04 LTS
Ubuntu-20.04       Ubuntu 20.04 LTS
OracleLinux_8_5    Oracle Linux 8.5
OracleLinux_7_9    Oracle Linux 7.9

# 安装ubuntu 20.04
wsl --install Ubuntu-20.04
```

然后打开终端，打开ubuntu-20.04，创建用户和密码

换源+更新

然后安装anaconda

## Install Anaconda on Ubuntu

```bash
wget https://mirrors.bfsu.edu.cn/anaconda/archive/Anaconda3-5.3.0-Linux-x86_64.sh
chmod +x Anaconda3-5.3.0-Linux-x86_64.sh
./Anaconda3-5.3.0-Linux-x86_64.sh
yes
ENTER
```

安装完成之后，检查版本：

```bash
anaconda -V
conda -V
```

### 使用anaconda

换源：

```bash
cd
vim .condarc
```

编辑`.condarc` ，添加清华源

```bash
# add to .condarc
ssl_verify: false
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch-lts: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```

更新：

 ```bash
conda update -n base -c defaults conda # 升级anaconda
 ```

```bash
conda create -n myconda python=3.7 		# 创建虚拟环境，名称为myconda（可以修改
conda info --envs 						# 查看已安装的虚拟环境
conda activate myconda					# 激活环境myconda
conda deactivate						# 关闭当前环境
```

```bash
conda list				# 查看conda的包
pip list				# 查看pip的包
# 给pip换源  （也可以直接使用命令更换阿里源：
# pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/
cd
mkdir .pip
vim .pip/pip.conf
# 添加以下内容
#-----------------------------------------
[global]
index-url = https://mirrors.bfsu.edu.cn/pypi/web/simple
format = columns
trusted-host = mirrors.bfsu.edu.cn
#-----------------------------------------
pip install jupyter 	# 安装jupyter
jupyter notebook		# 开启jupyter notebook服务

```

## 附加配置

### systemd

编辑 `/etc/wsl.conf`

```conf
[boot]
systemd=true
```

### WSL distros 的备份还原

- 备份

```powershell
wsl -l -v
# 显示为
  NAME            STATE           VERSION
* Ubuntu-20.04    Running         2

wsl -t Ubuntu-20.04
wsl --export Ubuntu-20.04 E:\SystemBackup\ubuntu-wsl2-2022.11.29.tar
```

- 还原

```powershell
wsl --import <distro-name> <install-path> <backup-file>
# e.g.
wsl --import Ubuntu c:\wsl2 d:\save\linux\wsl2.tar
```

## WSL-Ubuntu安装 [[Computer/Programming/Language/Qt|qt]]

```bash
sudo apt install qt5* qtcreator
```

创建项目时如果出现“no suitable kits”，点击“option”查看配置，如果“QT version”为“none”，则选择 `/usr/lib/qt5/bin/qmake`，保存即可。
