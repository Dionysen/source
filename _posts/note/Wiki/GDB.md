---
title: GDB
date: 2023-05-25 17:34:01
categories: Wiki
tags: [CPP, C, Debug, Linux]
comment: true
---

Compile the source file to the binary file.
Add argument `-g` to generate a GDB binary file.

<!--more-->

```bash
gcc -g source.c -o output
g++ -g source.cpp -o output
ls -a
total 52K
-rw-r--r-- 1 dionysen dionysen  450 Oct  5 22:26 binary-search.cpp
-rw-r--r-- 1 dionysen dionysen 2.5K Oct  2 14:29 linked-list.cpp
-rw-r--r-- 1 dionysen dionysen  411 Oct  2 14:41 node.cpp
-rwxr-xr-x 1 dionysen dionysen  37K Oct  5 23:04 output
```

| Command      | Full name    | Do somthing                                                                           |
| ------------ | ------------ | ------------------------------------------------------------------------------------- |
| `gdb output` |              |                                                                                       |
| `r`          | `run`        | Run current program                                                                   |
| `b`          | `break`      | Set a breakpoint at `[function]` or `[line]` (`in file`)                              |
| `c`          | `continue`   | Continue running your program (after stopping, e.g. at a breakpoint).                 |
| `n`          | `next`       | Execute next program line (after stopping); step over any function calls in the line. |
| `s`          | `step`       | Execute next program line (after stopping); step into any function calls in the line. |
| `l`          | `list`       | Type the text of the program in the vicinity of where it is presently stopped.        |
| `p`          | `print`      | Display the value of an expression.                                                   |
| `watch`      | `watch`      | Set a watchpoint in an address of expression                                          |
| `i b`        | `info break` | Check information of breakpoints.                                                     |
| `k`          | `kill`       | Kill the program being debugged.                                                      |
| `q`          | `quit`       | Exit from GDB.                                                                        |

- You can use `shell [args]` to execute a shell command.
