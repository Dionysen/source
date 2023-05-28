---
title: Neovim
date: 2023-05-25 17:34:01
categories: Wiki
tags: [Vim, Neovim, Termux, Linux, IDE]
comment: true
---

## Introduction

一个`vim`的社区版本，使用`lua`语言配置脚本，简单快捷

有强大的社区插件支持，可以打造成一个只有自己想要的功能的IDE

<!--more-->

## Installation

```bash
sudo pacman -S neovim
```

## Usage

### `init.lua`

Neovim allows load `init.vim` or `init.lua` in your path `~/.config/nvim`.

**Lua** is a simple and easy using script language, which is very suitable to configure the nvim.

### Quick start

The file directory of neovim configuration:

``` bash
$ cd .config/nvim
$ tree .

.
├── init.lua
├── lua
│   ├── colorscheme.lua
│   ├── keymaps.lua
│   ├── lsp
│   │   └── cmp.lua
│   ├── plugin-config
│   │   ├── autopairs.lua
│   │   ├── bufferline.lua
│   │   ├── comment.lua
│   │   ├── dashboard.lua
│   │   ├── formatter.lua
│   │   ├── lualine.lua
│   │   ├── mason.lua
│   │   ├── nvim-tree.lua
│   │   ├── nvim-treesitter.lua
│   │   ├── project.lua
│   │   └── telescope.lua
│   └── plugins.lua
└── plugin
    └── packer_compiled.lua

4 directories, 17 files
```

### 基础配置`init.lua`

```lua
require("plugins")
require("colorscheme")
require("keymaps")
require("plugin-config.nvim-tree")
require("plugin-config.bufferline")
require("plugin-config.lualine")
require("plugin-config.telescope")
require("plugin-config.dashboard")
require("plugin-config.project")
require("plugin-config.nvim-treesitter")
require("plugin-config.comment")
require("plugin-config.autopairs")
require("plugin-config.mason")
require("plugin-config.formatter")

require("lsp.cmp")
require("lspconfig").pyright.setup({})
require("lspconfig").clangd.setup({})

-- Set cursor sharp
vim.cmd("set guicursor =i:blinkon150")

vim.g.material_style = "darker"

-- utf8
vim.g.encoding = "UTF-8"
vim.o.fileencoding = "utf-8"
-- jkhl 移动时光标周围保留8行
vim.o.scrolloff = 8
vim.o.sidescrolloff = 8
-- 使用相对行号
vim.wo.number = true
vim.wo.relativenumber = true

-- 高亮所在行
vim.wo.cursorline = true
-- 显示左侧图标指示列
vim.wo.signcolumn = "yes"
-- 右侧参考线，超过表示代码太长了，考虑换行
-- vim.wo.colorcolumn = "80"
-- 缩进2个空格等于一个Tab
vim.o.tabstop = 4
vim.bo.tabstop = 4
vim.o.softtabstop = 4
vim.o.shiftround = true
-- >> << 时移动长度
vim.o.shiftwidth = 4
vim.bo.shiftwidth = 4
-- 空格替代tab
vim.o.expandtab = true
vim.bo.expandtab = true
-- 新行对齐当前行
vim.o.autoindent = true
vim.bo.autoindent = true
vim.o.smartindent = true
-- 搜索大小写不敏感，除非包含大写
vim.o.ignorecase = true
vim.o.smartcase = true
-- 搜索不要高亮
vim.o.hlsearch = false
-- 边输入边搜索
vim.o.incsearch = true
-- 命令行高为2，提供足够的显示空间
vim.o.cmdheight = 0
-- 当文件被外部程序修改时，自动加载
vim.o.autoread = true
vim.bo.autoread = true
-- 禁止折行
vim.wo.wrap = false
-- 光标在行首尾时<Left><Right>可以跳到下一行
vim.o.whichwrap = "<,>,[,]"
-- 允许隐藏被修改过的buffer
vim.o.hidden = true
-- 鼠标支持
vim.o.mouse = "a"
-- 禁止创建备份文件
vim.o.backup = false
vim.o.writebackup = false
vim.o.swapfile = false
-- smaller updatetime
vim.o.updatetime = 300
-- 设置 timeoutlen 为等待键盘快捷键连击时间500毫秒，可根据需要设置
vim.o.timeoutlen = 500
-- split window 从下边和右边出现
vim.o.splitbelow = true
vim.o.splitright = true
-- 自动补全不自动选中
vim.g.completeopt = "menu,menuone,noselect,noinsert"
-- 样式
vim.o.background = "dark"
vim.o.termguicolors = true
vim.opt.termguicolors = true
-- 不可见字符的显示，这里只把空格显示为一个点
vim.o.list = false
-- vim.o.listchars = "space: "
-- 补全增强
vim.o.wildmenu = true
-- Dont' pass messages to |ins-completin menu|
vim.o.shortmess = vim.o.shortmess .. "c"
-- 补全最多显示10行
vim.o.pumheight = 10
-- 永远显示 tabline
-- vim.o.showtabline = 2
-- 使用增强状态栏插件后不再需要 vim 的模式提示
vim.o.showmode = false

```

其中`require`是`lua`语言中的关键字，意为调用目录中的某一个`lua`脚本文件，格式为

```lua
require("something")
-- 这意味着调用了所有环境变量下可能存在的一个名为`something.lua`的脚本文件
-- lua会自己搜索所有的路径，寻找这个文件，找不到会返回异常
-- 类似于C中的#include
```

`lua`脚本中可以直接条用`vim script`语句，形式如：

```lua
vim.cmd("set nu")
```

或者使用neovim的API：

```lua
vim.o.background = "light"
-- 意为全局设置 vim.option
vim.wo.background = "light"
-- 意为窗口区设置 vim.window.option
vim.bo.background = "light"
-- 意为缓冲区设置 vim.buffer.option
vim.g.mapleader = " "
-- 获取或设置全局变量
```

### 快捷键映射

创建一个`lua`脚本文件专门用来设置快捷键映射：

```bash
cd ~/.config/nvim
mkdir lua
nvim ./lua/keymaps.lua
```

编辑此文件：

```lua

-- 设置全局变量“leader”键为空格键
vim.g.mapleader = " "
vim.g.maplocalleader = " "

local map = vim.api.nvim_set_keymap -- 创建快捷键映射函数的别名为“map”
local opt = { noremap = true, silent = true } -- 创建一个配置为opt, 非递归映射，且使用时不显示命令

-- basic 此为基础设置------------------------------------------------

map("n", "Q", ":q<cr>", opt)
map("n", "qQ", ":q!<cr>", opt)
map("n", "W", ":w<cr>", opt)
map("n", "S", ":wq<cr>", opt)
map("n", "U", ":PackerSync<cr>", opt)

map("n", "h", "i", opt)
map("i", "jj", "<esc>", opt)
map("v", "jj", "<esc>", opt)
map("i", "ji", "<esc>la", opt)


-- Orient 修改方向键------------------------------------------------

map("n", "i", "k", opt)
map("n", "k", "j", opt)
map("n", "j", "h", opt)

map("n", "I", "5k", opt)
map("n", "K", "5j", opt)
map("n", "J", "5h", opt)
map("n", "L", "5l", opt)

map("n", "<C-i>", "15k", opt)
map("n", "<C-k>", "15j", opt)
map("n", "<C-j>", "15h", opt)
map("n", "<C-l>", "15l", opt)

map("v", "i", "k", opt)
map("v", "k", "j", opt)
map("v", "j", "h", opt)

map("v", "I", "5k", opt)
map("v", "K", "5j", opt)
map("v", "J", "5h", opt)
map("v", "L", "5l", opt)

map("v", "<C-i>", "15k", opt)
map("v", "<C-k>", "15j", opt)
map("v", "<C-j>", "15h", opt)
map("v", "<C-l>", "15l", opt)

-- Split window 分屏------------------------------------------------
map("n", "s", "", opt)
-- windows 分屏快捷键
map("n", "sl", ":vsp<CR>", opt)
map("n", "sj", ":vsp<CR><C-w>h", opt)
map("n", "sk", ":sp<CR>", opt)
map("n", "si", ":sp<CR><C-w>k", opt)
-- 关闭当前
map("n", "sc", "<C-w>c", opt)
-- 关闭其他
map("n", "so", "<C-w>o", opt)
-- Alt + hjkl  窗口之间跳转
map("n", "<A-j>", "<C-w>h", opt)
map("n", "<A-k>", "<C-w>j", opt)
map("n", "<A-i>", "<C-w>k", opt)
map("n", "<A-l>", "<C-w>l", opt)

-- 左右比例控制
map("n", "<A-L>", ":vertical resize -5<CR>", opt)
map("n", "<A-J>", ":vertical resize +5<cr>", opt)
map("n", "<leader>l", ":vertical resize -20<CR>", opt)
map("n", "<leader>j", ":vertical resize +20<CR>", opt)
-- 上下比例
map("n", "<leader>i", ":resize +10<CR>", opt)
map("n", "<leader>k", ":resize -10<CR>", opt)
map("n", "<A-I>", ":resize +5<CR>", opt)
map("n", "<A-K>", ":resize -5<CR>", opt)
-- 等比例
map("n", "s=", "<C-w>=", opt)

-- Terminal 终端------------------------------------------------

map("n", "<leader>t", ":sp | terminal<CR>", opt)
map("n", "<leader>vt", ":vsp | terminal<CR>", opt)
map("t", "<Esc>", "<C-\\><C-n>", opt)
map("t", "<A-j>", [[ <C-\><C-N><C-w>h ]], opt)
map("t", "<A-k>", [[ <C-\><C-N><C-w>j ]], opt)
map("t", "<A-j>", [[ <C-\><C-N><C-w>k ]], opt)
map("t", "<A-l>", [[ <C-\><C-N><C-w>l ]], opt)

-- Visual 在visual模式下的按键------------------------------------------------

-- 缩进代码
map("v", "<", "<gv", opt)
map("v", ">", ">gv", opt)
-- 上下移动选中文本
map("v", "<C-K>", ":move '>+1<CR>gv-gv", opt)
map("v", "<C-I>", ":move '<-2<CR>gv-gv", opt)
-- 在visual 模式里粘贴不要复制
map("v", "p", '"_dP', opt)
-- insert 模式下，跳到行首行尾
map("i", "<C-j>", "<ESC>I", opt)
map("i", "<C-l>", "<ESC>A", opt)


-- 插件快捷键
local pluginKeys = {} -- 创建插件快捷变量，可以在其他lua文件中调用以下各个插件的快捷键
-- nvim-tree
-- alt + m 键打开关闭tree
map("n", "<A-m>", ":NvimTreeToggle<CR>", opt)
-- 列表快捷键
pluginKeys.nvimTreeList = {
 -- 打开文件或文件夹
 { key = { "<CR>", "o", "<2-LeftMouse>" }, action = "edit" },
 -- 分屏打开文件
 { key = "v", action = "vsplit" },
 { key = "h", action = "split" },
 -- 显示隐藏文件
 --{ key = "i", action = "toggle_custom" }, -- 对应 filters 中的 custom (node_modules)
 { key = ".", action = "toggle_dotfiles" }, -- Hide (dotfiles)
 -- 文件操作
 { key = "<F5>", action = "refresh" },
 { key = "a", action = "create" },
 { key = "d", action = "remove" },
 { key = "r", action = "rename" },
 { key = "x", action = "cut" },
 { key = "c", action = "copy" },
 { key = "p", action = "paste" },
}

-- bufferline
-- 左右Tab切换
map("n", "<C-j>", ":BufferLineCyclePrev<CR>", opt)
map("n", "<C-l>", ":BufferLineCycleNext<CR>", opt)
-- 关闭
--"moll/vim-bbye"
map("n", "<C-w>", ":Bdelete!<CR>", opt)
map("n", "<leader>bl", ":BufferLineCloseRight<CR>", opt)
map("n", "<leader>bh", ":BufferLineCloseLeft<CR>", opt)
map("n", "<leader>bc", ":BufferLinePickClose<CR>", opt)

-- Telescope
-- 查找文件
map("n", "<C-p>", ":Telescope find_files<CR>", opt)
-- 全局搜索
map("n", "<C-f>", ":Telescope live_grep<CR>", opt)
pluginKeys.telescopeList = {
 i = {
  -- 上下移动
  ["<C-k>"] = "move_selection_next",
  ["<C-i>"] = "move_selection_previous",
  ["<Down>"] = "move_selection_next",
  ["<Up>"] = "move_selection_previous",
  -- 历史记录
  ["<C-n>"] = "cycle_history_next",
  ["<C-p>"] = "cycle_history_prev",
  -- 关闭窗口
  ["<C-c>"] = "close",
  ["<Esc>"] = "close",
  -- 预览窗口上下滚动
  ["<C-u>"] = "preview_scrolling_up",
  ["<C-d>"] = "preview_scrolling_down",
 },
}
-- Lsp Mappings 待看
-- See `:help vim.diagnostic.*` for documentation on any of the below functions
vim.keymap.set('n', '<space>e', vim.diagnostic.open_float, opts)
vim.keymap.set('n', '[d', vim.diagnostic.goto_prev, opts)
vim.keymap.set('n', ']d', vim.diagnostic.goto_next, opts)
vim.keymap.set('n', '<space>q', vim.diagnostic.setloclist, opts)
-- Use an on_attach function to only map the following keys
-- after the language server attaches to the current buffer
local on_attach = function(client, bufnr)
  -- Enable completion triggered by <c-x><c-o>
  vim.api.nvim_buf_set_option(bufnr, 'omnifunc', 'v:lua.vim.lsp.omnifunc')
  -- Mappings.
  -- See `:help vim.lsp.*` for documentation on any of the below functions
  local bufopts = { noremap=true, silent=true, buffer=bufnr }
  vim.keymap.set('n', 'gD', vim.lsp.buf.declaration, bufopts)
  vim.keymap.set('n', 'gd', vim.lsp.buf.definition, bufopts)
map("n", "gh", vim.lsp.buf.hover, opt)
  vim.keymap.set('n', 'gh', vim.lsp.buf.hover, bufopts)
  vim.keymap.set('n', 'gi', vim.lsp.buf.implementation, bufopts)
  vim.keymap.set('n', '<C-k>', vim.lsp.buf.signature_help, bufopts)
  vim.keymap.set('n', '<space>wa', vim.lsp.buf.add_workspace_folder, bufopts)
  vim.keymap.set('n', '<space>wr', vim.lsp.buf.remove_workspace_folder, bufopts)
  vim.keymap.set('n', '<space>wl', function()
    print(vim.inspect(vim.lsp.buf.list_workspace_folders()))
  end, bufopts)
  vim.keymap.set('n', '<space>D', vim.lsp.buf.type_definition, bufopts)
  vim.keymap.set('n', '<space>rn', vim.lsp.buf.rename, bufopts)
  vim.keymap.set('n', '<space>ca', vim.lsp.buf.code_action, bufopts)
  vim.keymap.set('n', 'gr', vim.lsp.buf.references, bufopts)
  vim.keymap.set('n', '<space>f', function() vim.lsp.buf.format { async = true } end, bufopts)
end
local lsp_flags = {
  -- This is the default in Nvim 0.7+
  debounce_text_changes = 150,
}
require('lspconfig')['pyright'].setup{
    on_attach = on_attach,
    flags = lsp_flags,
}
require('lspconfig')['clangd'].setup{
    on_attach = on_attach,
    flags = lsp_flags,
}



-- nvim-cmp 自动补全
pluginKeys.cmp = function(cmp)
 local feedkey = function(key, mode)
  vim.api.nvim_feedkeys(vim.api.nvim_replace_termcodes(key, true, true, true), mode, true)
 end

 local has_words_before = function()
  local line, col = unpack(vim.api.nvim_win_get_cursor(0))
  return col ~= 0 and vim.api.nvim_buf_get_lines(0, line - 1, line, true)[1]:sub(col, col):match("%s") == nil
 end

 return {

  -- 自定义代码段跳转到下一个参数
  ["<C-l>"] = cmp.mapping(function(_)
   if vim.fn["vsnip#available"](1) == 1 then
    feedkey("<Plug>(vsnip-expand-or-jump)", "")
   end
  end, { "i", "s" }),

  -- 自定义代码段跳转到上一个参数
  ["<C-h>"] = cmp.mapping(function()
   if vim.fn["vsnip#jumpable"](-1) == 1 then
    feedkey("<Plug>(vsnip-jump-prev)", "")
   end
  end, { "i", "s" }),

  -- Super Tab
  ["<Tab>"] = cmp.mapping(function(fallback)
   if cmp.visible() then
    cmp.select_next_item()
   elseif vim.fn["vsnip#available"](1) == 1 then
    feedkey("<Plug>(vsnip-expand-or-jump)", "")
   elseif has_words_before() then
    cmp.complete()
   else
    fallback() -- The fallback function sends a already mapped key. In this case, it's probably `<Tab>`.
   end
  end, { "i", "s" }),

  ["<S-Tab>"] = cmp.mapping(function()
   if cmp.visible() then
    cmp.select_prev_item()
   elseif vim.fn["vsnip#jumpable"](-1) == 1 then
    feedkey("<Plug>(vsnip-jump-prev)", "")
   end
  end, { "i", "s" }),
  -- end of super Tab
 }
end

-- format

map("n", "<leader>f", ":Format<CR>", opt)
map("n", "<leader>F", ":FormatWrite<CR>", opt)

return pluginKeys
```

### 插件及插件管理器

单纯使用`neovim`的自带功能太过单薄，社区有许多令人赏心悦目的插件可供使用，插件的安装依赖一个插件管理器，目前最流行的插件管理器是`packer.nvim`。

安装`packer.nvim`,在`shell`中执行：

```bash
git clone --depth 1 https://ghproxy.com/https://github.com/wbthomason/packer.nvim ~/.local/share/nvim/site/pack/packer/start/packer.nvim
nvim ~/.config/nvim/lua/plugins.lua
```

编辑`plugins.lua`文件为：

```lua
-- This file can be loaded by calling `lua require('plugins')` from your init.vim

-- Only required if you have packer configured as `opt`
vim.cmd([[packadd packer.nvim]])

return require("packer").startup(function(use)
 -- Packer can manage itself
 use("wbthomason/packer.nvim")

 -- file tree
 use({ "kyazdani42/nvim-tree.lua", requires = "kyazdani42/nvim-web-devicons" })

 -- colorscheme -------------------------------------------------------------------------------------
 -- tokyonight
 use("folke/tokyonight.nvim")
 -- OceanicNext
 use("mhartington/oceanic-next")
 -- gruvbox
 use({ "ellisonleao/gruvbox.nvim", requires = { "rktjmp/lush.nvim" } })
 -- nord
 use("shaunsingh/nord.nvim")
 -- onedark
 use("ful1e5/onedark.nvim")
 -- nightfox
 use("EdenEast/nightfox.nvim")
 -- github
 use("projekt0n/github-nvim-theme")
 -- material
 use("marko-cerovac/material.nvim")
 -- one_monokai
 use("cpea2506/one_monokai.nvim")

 use("lourenci/github-colors")

 --buffer line
 use({ "akinsho/bufferline.nvim", requires = { "kyazdani42/nvim-web-devicons", "moll/vim-bbye" } })

 -- lualine
 use({ "nvim-lualine/lualine.nvim", requires = { "kyazdani42/nvim-web-devicons" } })
 use("arkav/lualine-lsp-progress")

 -- telescope FILE FINDER
 use({ "nvim-telescope/telescope.nvim", requires = { "nvim-lua/plenary.nvim" } })

 -- dashboard-nvim
 use("glepnir/dashboard-nvim")

 -- project
 use("ahmedkhalf/project.nvim")

 -- treesitter
 use({
  "nvim-treesitter/nvim-treesitter",
  run = ":TSUpdate",
  -- config = function()
  --  require("nvim-treesitter.configs").setup({
  --   highlight = {
  --    enable = true,
  --   },
  --  })
  -- end,
 })

 -- Comment
 use({
  "numToStr/Comment.nvim",
  config = function()
   require("Comment").setup()
  end,
 })

 -- Lspconfig
 use({ "neovim/nvim-lspconfig" })

 -- 补全引擎
 use("hrsh7th/nvim-cmp")
 -- snippet 引擎
 use("hrsh7th/vim-vsnip")
 -- 补全源
 use("hrsh7th/cmp-vsnip")
 use("hrsh7th/cmp-nvim-lsp") -- { name = nvim_lsp }
 use("hrsh7th/cmp-buffer") -- { name = 'buffer' },
 use("hrsh7th/cmp-path") -- { name = 'path' }
 use("hrsh7th/cmp-cmdline") -- { name = 'cmdline' }

 -- 常见编程语言代码段
 use("rafamadriz/friendly-snippets")

 -- auto-pairs
 use("windwp/nvim-autopairs")

 -- lsp-spport
 use({ "williamboman/mason.nvim" })

 -- formatter
 use({ "mhartington/formatter.nvim" })
end)
```

保存后重新打开`nvim`，然后在`normal`模式下，输入`:PackerSync`，回车即可开始安装插件。

若有现有插件与配置文件中所列的插件不同，`Packer.nvim`会询问是否删除不用的插件，直接回车即可。（默认是不删除）

### 配置插件

创建存放插件配置文件的文件夹：

```bash
mkdir ~/.config/nvim/lua/plugin-config
```

#### `nvim-tree`

在`plugins.lua`中加入`use({ "kyazdani42/nvim-tree.lua", requires = "kyazdani42/nvim-web-devicons" })`来启用此插件。

```bash
nvim ~/.config/nvim/lua/plugin-config/nvim-tree.lua
```

添加以下配置信息：

```lua
local status, nvim_tree = pcall(require, "nvim-tree")
if not status then
    vim.notify("没有找到 nvim-tree")
  return
end

-- 以上是一个经典的插件配置调用函数
-- 先创建一个本地变量status, nvim_tree
-- 变量的值为函数pcall的返回值
-- pcall函数的参数是require一个函数“nvim-tree”
-- 如果成功，返回true，失败，则返回false
-- 下面是一个if判断语句，如果调用成功（即status为true），继续往下走读取配置信息，如果调用失败，则直接return，跳出此脚本


-- 列表操作快捷键
local list_keys = require('keymaps').nvimTreeList
nvim_tree.setup({
    -- 是否显示 git 状态图标
    git = {
        enable = true,
    },
    -- project plugin 需要这样设置
    update_cwd = true,
    update_focused_file = {
        enable = true,
        update_cwd = true,
    },
    -- 隐藏 .文件 和 node_modules 文件夹
    filters = {
        dotfiles = false,
        custom = { 'node_modules' },
    },
    view = {
        -- 宽度
        width = 30,
        -- 也可以 'right'
        side = 'left',
        -- 隐藏根目录
        hide_root_folder = false,
        -- 自定义列表中快捷键
        mappings = {
            custom_only = false,
            list = list_keys,
        },
        -- 不显示行数
        number = false,
        relativenumber = false,
        -- 显示图标
        signcolumn = 'yes',
    },
    actions = {
        open_file = {
            -- 首次打开大小适配
            resize_window = true,
            -- 打开文件时关闭
            quit_on_open = true,
        },
    },
    -- wsl install -g wsl-open
    -- https://github.com/4U6U57/wsl-open/
    system_open = {
        cmd = 'open', -- mac 直接设置为 open
    },

    -- project plugin 
    update_cwd = true,
    update_focused_file = {
      enable = true,
      update_cwd = true,
    },

})
-- 设置打开文件时自动关闭
vim.cmd([[
  autocmd BufEnter * ++nested if winnr('$') == 1 && bufname() == 'NvimTree_' . tabpagenr() | quit | endif
]])
```
