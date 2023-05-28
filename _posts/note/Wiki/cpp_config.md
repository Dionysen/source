---
title: Clangd Config CMakeLists. txt
date: 2023-05-25 17:34:01
categories: Wiki
tags: [CPP, CMake, Clangd, Linux]
comment: true
---

Vim using Coc-nvim plugin `clangd-lsp` need to read `CMakeLists.txt` so that it can auto-complete your code.
If your project builds with CMake, it can generate this file. <!-- more -->You should enable it with:

```bash
cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=1
```

`compile_commands.json` will be written to your build directory. If your build directory is `$SRC` or `$SRC/build`, clangd will find it. Otherwise, symlink or copy it to `$SRC`, the root of your source tree.

```bash
ln -s ~/myproject-build/compile_commands.json ~/myproject/
```

Generated `compile_commands.json` can support auto completion for third party libraries.
