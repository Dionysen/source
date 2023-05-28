---
title: Linux Note
date: 2023-05-25 17:34:01
categories: Wiki
tags: [Termux, Linux, WSL2, Windows]
comment: true
sticky: 999
---

![R-C2](https://cdn.jsdelivr.net/gh/Dionysen/BlogCDN@main/img/R-C2.png)

> 📚  Some of knowledge encountered in the process of learning Linux.

<!--more-->

## Linux General Issues

### Linux 添加环境变量

添加路径到 `.bashrc` , `/etc/bashrc`, `.bash_profile`, `/etc/profile`, `/etc/environment`

```bash
export PATH=$PATH:/path/to/PATH
```

### Tmoe 脚本

```bash
curl -LO https://l.tmoe.me/2.awk
awk -f 2.awk
```

### Linux 更改家目录文件名的语言

```bash
export LANG=en_US
xdg-user-dirs-gtk-update
# 在弹出的对话框中选择更新文件名
# 然后再改回
export LANG=zh_CN
```

### Qt 最新版完整安装

使用在线安装器

```bash
wget https://mirrors.tuna.tsinghua.edu.cn/qt/archive/online_installers/4.5/qt-unified-linux-x64-4.5.0-online.run
chmod +x qt-unified-linux-x64-4.5.0-online.run
./qt-unified-linux-x64-4.5.0-online.run # 需要图形界面
```

### Linux 文件权限

常见的有 `644`、`755`、`777`
| 444 | r--r--r-- |
| --- | --------- |
| 600 | rw------- |
| 644 | rw-r--r-- |
| 666 | rw-rw-rw- |
| 700 | rwx------ |
| 744 | rwxr--r-- |
| 755 | rwxr-xr-x |
| 777 | rwxrwxrwx |

| 数字                                        | 权限             | 字母         |
|:-----------------------------------------:|:--------------:|:----------:|
| 4                                         | 读              | r          |
| 2                                         | 写              | w          |
| 1                                         | 执行             | x          |
| 0                                         | 无              | 无          |
| 以 `755` 为例                                |                |            |
| 权限代码                                      | 7              | 5          |
| ------------                              | -------------- | ---------- |
| 权限对应用户                                    | 文件所有者          | 组用户        |
| 计算                                        | 4+2+1          | 4+1        |
| 权限                                        | 可读可写可执行        | 可读可执行      |
| 若用 `chmod 4755 filename` 可使此程序具有 root 的权限 |                |            |

### Run Multiple Processes on Linux Terminal

```bash
hexo g -w & hexo s
```

Different from `hexo g -w ; hexo s`, `&` implicate that the former and the latter will run at the same time. The command  in the front of `;` priors to the command  in the back of `;`.

### 给 shell 脚本文件添加可执行权限

```bash
chmod +x shell.sh
```

### Linux 查看磁盘空间

```bash
df -hl
```

### Linux修改中文

```bash
vim /etc/locale.gen
# Uncommit zh_CN and en_US
locale-gen

vim /etc/locale.conf
# LANG="zh_CN-UTF-8"

vim .bashrc
# Add:
# export LANG=zh_CN.UTF-8
# export LANGUAGE=zh_CN:en_US

sudo pacman -S noto-fonts-cjk
# Install font
```

## Archlinux

### Backup and Restore (using pigz)

#### Backup

```bash
sudo pacman -Syyu # Update system  
sudo pacman -S pigz #Install pigz  
cd /
sudo tar --use-compress-program=pigz -cvpf /run/media/icarus/MHD/Systembackup/archlinux-backup@`date +%Y-%m+%d`.tgz --exclude=/proc --exclude=/lost+found --exclude=/mnt --exclude=/sys --exclude=/tmp --exclude=/run/media --exclude=/home / #Backup  

sudo tar -cvpzf /run/media/icarus/MHD/Systembackup/archlinux-backup-pureKDE.tgz --exclude=/proc --exclude=/lost+found --exclude=/mnt --exclude=/sys --exclude=/run/media --[[#Clean up Trash in Archlinux]]exclude=/tmp /  #Don't use pigz, and network is not necessary
```

#### Restore

```bash
# Boot by Live CD 
iwctl                             
device list                     # Find wlan0  
station wlan0 scan              # Scan WIFI  
station wlan0 get-networks      # List network 
station wlan0 connect WIFI1  # Connect a network  
exit                            # Exit after successing  
ping www.bing.com     # Test network  

sudo vim /etc/pacman.d/mirrorlist # Add "Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch"  
sudo pacman -S pigz    # Install pigz  
lsblk        # View disk 
mkdir /RE       # Create a partition of backup files
mount /dev/sda1 /RE    # Mount the disk where backup files are stored
mount /dev/sdb3 /mnt    # Mount system root directory of Archlinux to /mnt  

rm -rf /mnt/*      # Clean old system  
tar --use-compress-program=pigz -xvpf /RE/Systembackup/archlinux-backup-pureKDE.tgz  -C /mnt          # Restore system 
ls /mnt       # View the restore 
umount -R /mnt      # Unmount /mnt  
reboot        # Reboot
```

It is worth noting that **`fstab`** and **GRUB boot sequence** needs to be regenerated!

### Add Windows Boot Manager to GRUB

```bash
sudo pacman -S grub-customizer
```

Add a boot menu in grub-customizer, then modify the configuration:

```bash
menuentry 'Windows 10' {  
 set root='(hd1,3)'  
 search --no-floppy --fs-uuid --set 0527-0342  
 chainloader (${root})/EFI/Microsoft/Boot/bootmgfw.efi  
}
```

Use `blkid` view the `uuid` of EFI partition:

```bash
sudo blkid
```

### Install . deb Package in Archlinux

```bash
yay -S debtap  
sudo debtap -u  
debtap </application>.deb  
sudo pacman -U </package-name>
```

### Can't Connect Bluetooth Keyboard in Archlinux (GUI)

Using `Cli` :

```bash
sudo pacman -S bluez bluez-utils
# Install bluez
sudo bluetoothctl
power on
agent on
default-agent
scan on
pair <MAC address of keyboard>
trust <MAC address of keyboard>
connect <MAC address>
```

## Linux Terminal Proxy Setting

```bash
sudo pacman -S proxychains-gn  # Install proxychains  
vim /etc/proxychains.conf       # Edit proxychains.conf
```

Add **socks proxy** at `proxylist` in `Proxychains` :

```bash
socks4 127.0.0.1 1080 
```

or

```bash
socks5 127.0.0.1 1080
```

Add `proxychains` before the command that needs proxy.
s of keyboard>

### Clean up Trash in Archlinux

```bash
sudo pacman -R ${pacman -Qdtq}  # Clean up useless dependence
sudo pacman -Scc    # Clean up caches
```

### Install XMind Cracked for Linux

Install `xmind-vana-10.3.1-1-x86_64.pkg.tar.zst`， and edit `/etc/profile`，add

```bash
export VANA_LICENSE_MODE=true
export VANA_LICENSE_TO="sui bian xie"
```

Save and login out your system, then enjoy.

## Ubuntu

[[Wiki/Source List#ubuntu|镜像源]]，

### ubuntu 最小安装 gnome

```bash
sudo apt-get --no-install-recommends install ubuntu-gnome-desktop fonts-ubuntu yaru-theme-gtk gnome-tweaks fonts-noto fonts-noto-mono fonts-noto-cjk fonts-noto-color-emoji
```

## Window Related

### Windows Access [WSL2](WSL2) Files

Input `\\wsl$` in address bar of `explorer`.

### Win11 Restore Right-click Menu

```powershell
# To win10
reg.exe add "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /f /ve

# To win11
reg.exe delete "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /va /f
```

地址栏输入 `chrome://flags` 可以开启隐藏功能

## RealVNC 注册码

`Version`: `6.11`

```
7SA9N-9JF3P-E8CW2-BH9JU-PMVQA
```

## shell脚本

### 判断文件是否存在

```bash
-e filename # 如果 filename存在，则为真 
-d filename # 如果 filename为目录，则为真 
-f filename # 如果 filename为常规文件，则为真 
-L filename # 如果 filename为符号链接，则为真 
-r filename # 如果 filename可读，则为真 
-w filename # 如果 filename可写，则为真 
-x filename # 如果 filename可执行，则为真 
-s filename # 如果文件长度不为0，则为真 
-h filename # 如果文件是软链接，则为真
```

例如：

```bash
#shell判断文件夹是否存在

#如果文件夹不存在，创建文件夹
if [ ! -d "/myfolder" ]; then
  mkdir /myfolder
fi
```
