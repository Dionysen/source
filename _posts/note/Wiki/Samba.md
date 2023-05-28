---
title: Samba
data: 2022-11-20 01:28
tags: 
  - Samba Protocol
  - Windows
categories: Wiki
comment: true
---



- SMB是微软指定的协议，用于局域网文件共享，SMB全称是服务消息块
- SMB移植到linux后，就诞生了samba软件
- 可以在两台linux下，也可以在linux与windows之间

<!--more-->

## Installation

```bash
sudo pacman -S samba
```

## Usage

```bash
sudo vim /etc/samba/smb.conf
```

在配置文件中写入：

```bash
[global]
        workgroup = SAMBA
        Security = user
        passdb backend = tdbsam
        printing = cups
        printcap name = cups
        load printers = yes
        cups options = raw
[homes]
        comment = Home Directories
        valid users = %S, %D%w%S
        browseable = No
        readonly = No
        inherit acls = Yes
[printers]
        comment = All Printers
        path = /var/tmp
        printable = Yes
        create mask = 0600
        browseable = No

[print$]
        comment = Printer Driver
        path = /var/lib/samba/drivers
        write list = @printadmin root
        force group = @printadmin
        create mask = 0664
        directory mask = 0775
[dionysen]
        comment = dionysen
        path = /home/dionysen
        public = no
        writable = yes
        guest ok = yes
```

使用`pdbedit`命令创建samba专用的用户和密码，创建时必须保证这个用户在linux系统中存在

```bash
pdbedit -a -u dionysen  #create a user

sudo systemctl restart smb
sudo systemctl status smb

sudo netstat -tunlp | grep smb

tcp        0      0 0.0.0.0:445             0.0.0.0:*               LISTEN      189537/smbd
tcp        0      0 0.0.0.0:139             0.0.0.0:*               LISTEN      189537/smbd
tcp6       0      0 :::445                  :::*                    LISTEN      189537/smbd
tcp6       0      0 :::139                  :::*                    LISTEN      189537/smbd
```

打开防火墙(由于只能在局域网使用，已放弃)

改用[[FTP]]!
