---
title: V2rayA
date: 2023-05-25 17:34:01
categories: Wiki
tags: [VPN]
comment: true
---

On archlinux:

```bash
yay -S xray-bin
sudo pacman -S v2ray
yay -S v2raya-bin
```

Maybe you need restart your computer!

<!--more-->

And config:

| 项目                      | 配置                 |
| ------------------------- | -------------------- |
| 透明代理/系统代理         | 启用：大陆白名单模式 |
| 透明代理/系统代理实现方式 | redirect             |
| 规则端口的分流模式        | 大陆白名单模式       |
| 防止 DNS 污染             | 仅防止 DNS 劫持      |
| 特殊模式                  | supervisor           |
| TCPFastOpen               | 保持系统默认         |
