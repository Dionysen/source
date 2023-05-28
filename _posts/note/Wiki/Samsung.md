---
title: Samsung
data: 2022-11-20 01:28
tags: 
  - Mobile phone
  - root
categories: Wiki
comment: true
---

三星玩机经验<!--more-->

- 卸载不需要的系统软件：刷完欧版系统，没有过多预装软件，可以不用卸载，不用的可以长按禁用
- [x] **知乎收藏夹**
- [x] 配色模式

***三星解bl锁之前一定要退出账号***
刷完海外版固件，oem依然是解开的状态，但是有一个kg锁需要打开（prenormal是锁着的状态，checking是解开的状态），方式是进行系统更新检查，检查可能提醒无法进行更新，但无所谓，重启进入刷机模式就能看到已经解锁，此时可以刷magisk修补包了

试试安装busybux（已试，确实不行）

### Dex模式下使用谷歌输入法

条件：已Root
使用termux，输入命令：

```bash
sudo ime set com.google.android.inputmethod.latin/com.android.inputmethod.latin.LatinIME
```
