---
title: 云服务器安装Archlinux
date: 2023-05-25 17:34:01
categories: Wiki
tags: [Archlinux, Linux, Server]
comment: true
---

<!--more-->

### 准备工作（在已有的服务器上操作）

```bash
cd /
sudo wget https://mirrors.tuna.tsinghua.edu.cn/archlinux/iso/latest/archlinux-2022.12.01-x86_64.iso

mv arch* arch.iso  # 重命名为arch.iso

#编辑GRUB配置文件，加入 arch.iso 启动项（部分系统的该文件路径为 /boot/grub2/grub.cfg ）
 #编辑 /boot/grub/grub.cfg，在与下面结构类似的第一个 menuentry 前，添加下面的内容。（搜索“menuentry（空格）”的第一个匹配项）
vim /boot/grub/grub.cfg
 #配置600秒的GRUB等待时长，“vda1”项根据主机“fdisk -l”命令查看，视情况更改
 #花括号内的缩进为一个Tab键
set timeout=600
menuentry "Archlinux Live (x86_64)" {
    insmod iso9660
    set isofile=/arch.iso
    loopback lo0 ${isofile}
    linux (lo0)/arch/boot/x86_64/vmlinuz archisolabel=ARCH_202002 img_dev=/dev/vda1 img_loop=${isofile} earlymodules=loop
    initrd (lo0)/arch/boot/x86_64/archiso.img
}
```

### 重启进入 vnc 界面配置 ssh

```bash
 #如果提示“insmod”无法识别，进入原系统在GRUB配置文件中，使用Tab键重新缩进
 #配置 arch live 环境
 #设置密码
 passwd
 #自动分配IP
 dhcpcd
 #开启 ssh 服务
 systemctl start sshd
 #使用 ssh 连接，摆脱不好用的 VNC 界面
 #用户名 root，密码为 passwd 所设置的
 #重设磁盘 vda1 的读写权限
 mount -o rw,remount /dev/vda1
 #进入 vda1 挂载目录 /run/archiso/img_dev
 cd /run/archiso/img_dev
 #删除原系统文件（除了arch.iso）
 rm -rf [b-z]*
 #重新挂载 vda1 至 /mnt
 mount /dev/vda1 /mnt
```

### 正常安装 Arch Linux

跳过分区步骤，==此处万万不可随意重启==，因为已经没有系统了，也没有 GRUB 了

- 编辑软件源

```bash
 #编辑镜像源，将“China”字样的镜像源复制到镜像首，如“tuna”
 #使用文本编辑器“VIM”，打开镜像文件
 vim /etc/pacman.d/mirrorlist
     #在该文件中搜索“China”，vim使用符号“/”作为搜索标志，回车后使用“n”/“N”切换搜索“下一个”/“上一个”
     /China(回车)
     #停留在字样“tuna”/“aliyun”处，将其复制下来，vim使用“2yy”表示“复制2行”
     2yy
     #跳转到第6行
     6gg
     #粘贴
     p
     #保存退出
     :wq
```

- 安装基础软件包

```bash
 #使用 pacstrap 脚本，安装 base 软件包和 Linux 内核以及常规硬件的固件，此处我选择长期支持版内核
 pacstrap /mnt base linux-lts linux-firmware
 #使用 pacstrap 脚本，安装常用软件
 pacstrap /mnt base-devel grub openssh intel-ucode vim man dhcpcd
```

- 配置系统

```bash
#生成 fstab 文件
 genfstab -U /mnt >> /mnt/etc/fstab
 #将环境变更至新系统下
 arch-chroot /mnt
 #设置时区（软链接）
 ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
 #同步时钟
 hwclock --systohc
 #本地化（语言）
 vim /etc/locale.gen
     #移除某些行头的注释符“#”，可通过搜索“en_US”实现
     en_US.UTF-8 UTF-8
     #保存退出
     :wq
 #生成 local 信息
 locale-gen
 #创建 locale.conf
 vim /etc/locale.conf
     #编辑 LANG 变量
     LANG=en_US.UTF-8
     #保存退出
     :wq
 #创建网络相关文件
 vim /etc/hostname
     #写入你想要用的主机名
     myhostname
 vim /etc/hosts
     127.0.0.1   localhost
     ::1         localhost
     127.0.1.1   tencent.localdomain   tencent
```

- 设置用户

```bash
 #设置 root 账户密码
 passwd
 #创建新用户
 useradd -m -G wheel arch        # -m        创建家目录
                                 # -G        用户所属的组
                                 # arch      示例用户名
 #设置 arch 用户密码
 passwd arch
 #修改(arch)用户权限
 vim /etc/sudoers        # 编辑sudoer file
                         # 去掉“%wheel ALL=(ALL) ALL”前面的注释，保存退出
```

- 配置 GRUB

```bash
 #生成 grub 相关文件
 grub-install --target=i386-pc /dev/vda
 #生成 grub.cfg
 grub-mkconfig > /boot/grub/grub.cfg
```

- 配置网络

```bash
 #使能 dhcpcd
 systemctl enable dhcpcd
 #使能 sshd
 systemctl enable sshd
 #退出当前用户
 exit
 #重启
 reboot
```
