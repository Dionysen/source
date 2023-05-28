---
title: SCP protocol
date: 2023-05-25 17:34:01
categories: Wiki
tags: [SCP, SSH]
comment: true
---

`scp` copies files between hosts on a network.

<!--more-->

# scp

It uses ***ssh*** for data transfer, and uses the same authentication and provides the same security as a login session.

`scp` will ask for passwords or passphrases if they are needed for authentication.

The **source** and **target** may be specified as a local pathname, a remote host with optional path in the form `[user@]host:[path]`, or a URI in the form `scp://[user@]host[:port][/path]`.

> Local file names can be made explicit using absolute or relative pathnames to avoid `scp` treating file names containing '`:`' as host speacifiers.

When copying between two remote hosts, if the URI format is used, a **port** cannot be specified on the **target** if the `-R` option is used.

| Options | Implication                                                                                                                                            |
| ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `-3`    | Copies between two remote hosts are transferred through the local host.  Without this option the data is copied directly between the two remote hosts. |
| `-4`    | Forces `scp` to use IPv4 addresses only                                                                                                                |
| `-6`    | Forces `scp` to use IPv6 addresses only                                                                                                                |
| `-A`    | Allows forwarding of ssh-agent(1) to the remote system.  The default is not to forward an authentication agent.                                        |
| `-C`    | Compression enable                                                                                                                                     |
| `-l`    | Limits the used bandwidth, specified in Kbit/s                                                                                                         |
