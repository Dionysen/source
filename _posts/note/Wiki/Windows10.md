---
title: Window相关技巧
date: 2023-05-25 17:34:01
categories: Wiki
tags: [Window 10, Window 11, Font]
comment: true
---

> Windows系统的一些技巧

<!--more-->

## win10 修改系统字体

下载FontCreator，用其打开需要替换的字体，选择字体->属性：

<img src="https://cdn.jsdelivr.net/gh/Dionysen/BlogCDN@main/img/image-20230529192930581.png" alt="image-20230529192930581" style="zoom:50%;" />

然后将名字修改成`Microsoft Yahei`，然后进入PE系统，替换系统中的雅黑字体（msyh.ttc、msyhl.ttc、msyhbd.ttc三个文件）。

<img src="https://cdn.jsdelivr.net/gh/Dionysen/BlogCDN@main/img/image-20230529193004835.png" alt="image-20230529193004835" style="zoom:50%;" />

### 修改输入法候选字的字体

Win+r输入`regedit`打开注册表编辑器，找到如下：

```
HKEY_CURRENT_USER\Software\Microsoft\InputMethod\CandidateWindow\CHS\1
```

`FontStyle`和`FontStyleTSF3`修改成想要的字体，如：

<img src="https://cdn.jsdelivr.net/gh/Dionysen/BlogCDN@main/img/image-20230529192738123.png" alt="image-20230529192738123" style="zoom:67%;" />