---
title: Code-Server
date: 2023-05-25 17:34:01
categories: Wiki
tags: [CPP, CMake, Clangd, Linux]
comment: true
---



<img src="https://cdn.jsdelivr.net/gh/Dionysen/BlogCDN@main/img/image-20230526020746624.png" alt="image-20230526020746624" style="zoom:80%;" />

<!-- more -->

## 安装和配置

### 开启 `https`

下载 **SSL 证书**，解压到一个地方
在 `.config/code-server/config.yaml` 中加入：

```yaml
cert: /path/to/*.crt
cert-key: /path/to/*.key
```

使用`systemd`重启服务即可

### 修改字体 （目前最新版 `code-server` 不能用，实测 v4.7.1 可以）

目前只能通过特殊手段修改为 `Fira Code` ：

```bash
git clone https://github.com/tuanpham-dev/code-server-font-patch.git
cd code-server-font-patch

# Run this command (change path-to-code-server to your code-server path, leave it empty if you install code-server from installer or code-server is in /usr/lib/code-server):
sudo ./patch.sh [path-to-code-server]
```

You may need to set font family in code-server settings:

```css
"editor.fontFamily": "'Fira Code', Consolas, 'Courier New', monospace",
"terminal.integrated.fontFamily": "'Fira Code', Consolas, 'Courier New', monospace",
```

### Install Packages

|   Package    |        Function         |
| :----------: | :---------------------: |
|    clang     |         Compile         |
|    clangd    |    Language support     |
| clang-format |     Format the code     |
|     lldb     |          Debug          |
|    cmake     | Quick configure project |

### Install Plugins

Search the plugins, `CodeLLDB` and `clangd`.

### Config the Cmake and Clangd (Using plugin `cmake tool`)

Open a WSL2 distro and get into a folder, input `code .` and press `enter`.
Vscode will be started. There is empty in the folder.
Press `ctrl+shift+p` and input `cmake: quick start`, select the `CMake: Quick Start`.
Choice the `clang` variant.
Input the name of you project.
A `hello world` program will be auto-created.
Now, you can `build` and `run` your project.

#### Config debug

Add a `launch.json` in the `workfolder`, and add configuration `lldb`.
Modify the program.

```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "type": "lldb",
            "request": "launch",
            "name": "Debug",
            "program": "${workspaceFolder}/build/</*Your project name/>",
            "args": [],
            "cwd": "${workspaceFolder}"
        }
    ]
}
```

### MultiFolders

Add the `include_directories(./Sources)` to `CMakeLists.txt` .

```cmake
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
include_directories(Includes)
include_directories(Sources)
......
```

or `cd ${PROJ_DIR}/build` then run command `cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=1` in terminal.

### Clang-format

编辑 `.clang-format` 文件

```bash
IndentWidth:   4
# Mind the blank place is a tab
```

### Setting.json (Updating)

```json
{
    "files.autoSave": "onFocusChange",
    "editor.links": false,
    "editor.guides.indentation": false,
    "editor.fontFamily": "'Fira Code'",
    "editor.fontWeight": 400,
    "terminal.integrated.fontFamily": "'Fira Code'",
    "editor.fontSize": 13,
    "terminal.integrated.fontSize": 13,
    "editor.lineHeight": 1.5,
    // "vscode-neovim.neovimInitVimPaths.linux": "~/.config/nvim/init.lua",
    "editor.inlayHints.enabled": "off",
    "git.enabled": false,
    "markdown-preview-enhanced.codeBlockTheme": "github.css",
    "markdown.preview.breaks": true,
    "markdown.extension.tableFormatter.enabled": true,
    "editor.formatOnSave": true,
    "editor.codeActionsOnSave": {
        "source.fixAll.markdownlint": true
    },
    "cmake.autoSelectActiveFolder": false,
    "cmake.configureOnEdit": false,
    "editor.lineNumbers": "on",
    "workbench.editor.showTabs": true,
    "editor.minimap.autohide": true,
    "editor.minimap.enabled": false,
    "terminal.integrated.copyOnSelection": true,
    "terminal.integrated.cursorBlinking": true,
    "cmake.ignoreCMakeListsMissing": true,
    "vim.vimrc.path": "$HOME/backup/.vimrc",
    "vim.vimrc.enable": true,
    "extensions.webWorker": false,
    "vim.useCtrlKeys": false,
    "vim.enableNeovim": true,
    "vim.neovimConfigPath": "~/backup/init.vim",
}
```

Termux

```json
{
    "editor.fontFamily": "'Fira Code'",
    "terminal.integrated.fontFamily": "Fira Code",
    "markdown.preview.breaks": true,
    "editor.minimap.autohide": true,
    "markdown.styles": [
        "/data/data/com.termux/files/home/storage/documents/note/notes/.vscode/markdown-styles/ia_typora_night.css"
    ],
    "editor.wordWrap": "on",
}
```

### Issues

#### Font size in console of Script run (vscode plugin)

Add to stylesheet：

```css
.script-view .line {  
 font-size: 17px;  
}
```

#### CMake tools 在插件商店找不到

`Ctrl＋p` 输入命令：

```bash
ext install ms-vscode.cmake-tools
```

#### Codelldb配置

Install codelldb and create a `launch.json` :

```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "type": "lldb",
            "request": "launch",
            "name": "Debug",
            "program": "${workspaceFolder}/myvector/build/myvector",
            "args": [],
            "cwd": "${workspaceFolder}/myvector",
        }
    ]
}
```

- If breakpoint doesn't work, use cmake build a `debug` target.

Use shell:

```bash
cmake .. -DCMAKE_BUILD_TYPE=Debug
cmake --build . --config Debug
```

Or add to `CMakeLists.txt` :

```cmake
set(CMAKE_BUILD_TYPE Debug)
```

 

 