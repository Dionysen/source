---
title: Termux
date: 2022-05-25 17:34:01
categories: Wiki
tags: [Termux, Linux]
comment: true
---



安卓端的极客工具

能做许多你以为做不到的事情

<!--more-->

## Android 使用 Termux+Proot 在 Wayland 上运行 xfce4 或 KDE

### 部署

#### 安装Termux

#### 下载`termux-x11.deb` 和`termux-x11.apk`

#### 打开termux，切换镜像源

```bash
pkg in vim
vim /data/data/com.termux/files/usr/etc/apt/sources.list
# 添加以下镜像源
deb https://mirrors.ustc.edu.cn/termux/apt/termux-main stable main
# 执行
pkg update
```

#### 安装必要依赖和软件

```bash
pkg in x11-repo
pkg in xwayland
dpkg -i ./termux-x11.deb
```

安装 `termux-x11.apk`

1. 重启termux

```bash
pkg in proot-distro
proot-distro install archlinux

# 安装完成后：
proot-distro login archlinux
vi /etc/pacman.d/mirrorlist
# 添加
Server = https://mirrors.ustc.edu.cn/archlinuxarm/$arch/$repo

pacman -Syyu
pacman -S xfce4 # 安装xfce4桌面环境
```

#### 完成后，全部退出，打开termux

```bash
pkg in screen
screen -S termux-x11
termux-x11
# 此时会弹出termux-x11的窗口，切换回termux
# 按Ctrl+a+d,然后以共享tmp的方式登陆proot－archlinux

proot-distro login archlinux --shared-tmp
# 在archlinux中
export DISPLAY=:0
dbus-launch --exit-with-session startxfce4
```

### 若报错且无法显示图像

终端显示：

```bash
proot-distro login --user dionysen archlinux --shared-tmp                                                         ok | took 8s | at 01:03:12
[3] 11100
/usr/bin/startxfce4: X server already running on display :0
Environment variable $XAUTHORITY not set, ignoring.
Failed to import environment: Process org.freedesktop.systemd1 exited with status 1
```

****
需要在`~/.xinitrc`中添加`exec startxfce4`
如果xfce-session处于`suspend`的状态，使用`job -l`查看，使用`kill %3`杀死`[3]`进程。

### archlinux在xfce4中设置中文的方法

编辑`/etc/locale.gen`，注释掉`zh_CN.UTF-8` 前的`#`：

```bash
locale-gen
sudo vim /etc/locale.conf
```

添加`LANG="zh_CN.UTF-8"` 。

### Sandbox

可以在`/etc/environment`中添加参数`export MOZ_FAKE_NO_SANDBOX=1`.

### Termux-x11无法全屏显示

使用adb调试强制使其全屏：

1. 使用电脑adb调试
2. 使用无线adb调试
使用无线调试需要另一部手机，安装`termux`

```bash
pkg in android-tools
```

在被调试的手机上执行：

```bash
 # 打开被调试设备的adb调试和无线调试，点进去找到配对ip地址及密码
 adb pair <IP address>:<Port>
 adb connect <IP address>:<Port>
 # 有的设备pair与connect的端口可能不一样
 
 # 连接之后使用以下命令开启全屏
adb -s <IP address>:<Port> shell settings put global policy_control immersive.status=com.termux.x11

# 恢复默认设置
adb -s <IP address>:<Port> shell settings put global policy_control null
```

值得注意的是，这其实相当于一个环境变量，每次设置都会覆盖上一次的设置，因此如果要设置多个应用全屏，需要将多个应用用逗号隔开：

```bash
adb -s <IP address>:<Port> shell settings put global policy_control immersive.status=com.termux.x11,com.termux
```

## Termux Backup and Restore

```bash
termux-setup-storage  
cd /data/data/com.termux/files  
tar -zcf /sdcard/termux-backup.tar.gz home usr  # Backup  
termux-setup-storage  
cd /data/data/com.termux/files  
tar -zxf /sdcard/termux-backup.tar.gz --recursive-unlink --preserve-permissions # Restore
```

### Termux 备份说明

#### 2022-12-05

Temux：zsh+p10k
tmoe＋proot 容器： Kali，软件包含 Clion＋Wps＋vscode＋obsdian
proot-distro ：正常安装了 code-server

## Termux 安装 Code-Server

需要使用 proot-distro，因为 termux 原生安装 code-server 会导致许多插件无法安装。
先[[Wiki/Source List#termux|换源]]，然后执行命令：

```bash
apt in proot-distro
proot-distro install archlinux

# 安装完成后：
proot-distro login archlinux
vi /etc/pacman.d/mirrorlist
# 添加
Server = https://mirrors.ustc.edu.cn/archlinuxarm/$arch/$repo

# 安装依赖
pacman -Syyu
sudo pacman -S fakeroot

# 安装nvm，并用nvm安装所需求的特定版本nodejs
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.2/install.sh | bash
nvm install v16.18.1
nvm use v16.18.1

# 安装code-server
curl -fsSL https://code-server.dev/install.sh | sh
```

由于没有 systemd，可以使用脚本将 code-server 放在后台自动启动：

```bash
touch /home/icarus/.config/code-server/code-server.log
sudo vim /etc/profile
# add
nohup code-server  > /home/icarus/.config/code-server/code-server.log 2>&1 &
```
