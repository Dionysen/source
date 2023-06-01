---
title: Hexo 个人博客搭建
date: 2023-05-25 17:34:01
categories: Wiki
tags: [Hexo, Linux, Termux]
comment: true
sticky: 999
---

互联网时代信息爆炸，干净的工作环境成为必需品，想坐在桌前静心做事情，却恰恰因为坐在了桌前，心中浮躁万分。电脑中的纷杂信息顷刻占满注意力，根本无暇思考哪怕一丝坐下来前想要思考的东西。一个博客并不能解决这个问题，但作为一个干净的出口，它能让正在做事情的我不至分散心神。

<!-- more -->

## hexo安装和部署

windows系统安装wsl2_Archlinux

```bash
sudo pacman -S nodejs npm
node -v # 查看node版本信息  
npm -v # 查看npm版本信息
npm config get registry # 查看原来的源  
npm config set registry https://registry.npm.taobao.org # 修改为淘宝源  
npm config get registry # 查看现在的源
sudo npm install hexo-cli -g # 全局安装hexo命令行工具
```

```bash
hexo init "博客目录名称" # 目录名称不含空格的时候双引号可以省略
```

可以看到如下反馈：

```bash
INFO  Cloning hexo-starter https://github.com/hexojs/hexo-starter.git  
INFO  Install dependencies  
# 一些可能的中间信息  
INFO  Start blogging with Hexo!
```

```bash
cd "博客目录"
npm install # 安装的依赖项在package.json文件的dependencies字段中可以看到
tree -L 1 #查看目录结构
#结果如下
.  
├── _config.landscape.yml  
├── _config.yml  
├── node_modules  
├── package-lock.json  
├── package.json  
├── scaffolds  
├── source  
└── themes
```

各个目录的含义：

- **`_config.yml`**
  - 为全局配置文件，网站的很多信息都在这里配置，比如说网站名称，副标题，描述，作者，语言，主题等等。具体可以参考官方文档：[https://hexo.io/zh-cn/docs/configuration.html。](https://hexo.io/zh-cn/docs/configuration.html%E3%80%82)
- `scaffolds`
  - 骨架文件，是生成新页面或者新博客的模版。可以根据需求编辑，当`hexo`生成新博客的时候，会用这里面的模版进行初始化。
- `source`
  - 这个文件夹下面存放的是网站的`markdown`源文件，里面有一个`_post`文件夹，所有的`.md`博客文件都会存放在这个文件夹下。现在，你应该能看到里面有一个`hello-world.md`文件。
- `themes`
  - 网站主题目录，`hexo`有非常丰富的主题支持，主题目录会存放在这个目录下面。
  - 我们后续会以默认主题来演示，更多的主题参见：[https://hexo.io/themes/](https://hexo.io/themes/)

```bash
hexo new post "test" # 会在 source/_posts/ 目录下生成文件 ‘test.md’，打开编辑  
hexo generate        # 生成静态HTML文件到 /public 文件夹中  
hexo server          # 本地运行server服务预览，打开http://localhost:4000 即可预览你的博客
```

这是`hexo`的默认主题，更多的主题可以从官网下载。

更详细的`hexo`命令可以查看文档：[https://hexo.io/zh-cn/docs/commands](https://hexo.io/zh-cn/docs/commands)

简单提一下`_config.yml`的各个字段的含义：

```yml
# Site
title: Hexo  # 网站标题
subtitle:    # 网站副标题
description: # 网站描述
author: John Doe  # 作者
language:    # 语言
timezone:    # 网站时区, Hexo默认使用您电脑的时区

# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child'
## and root as '/child/'
url: http://yoursite.com   # 你的站点Url
root: /                    # 站点的根目录
permalink: :year/:month/:day/:title/   # 文章的 永久链接 格式   
permalink_defaults:        # 永久链接中各部分的默认值

# Directory   
source_dir: source     # 资源文件夹，这个文件夹用来存放内容
public_dir: public     # 公共文件夹，这个文件夹用于存放生成的站点文件。
tag_dir: tags          # 标签文件夹     
archive_dir: archives  # 归档文件夹
category_dir: categories     # 分类文件夹
code_dir: downloads/code     # Include code 文件夹
i18n_dir: :lang              # 国际化（i18n）文件夹
skip_render:                 # 跳过指定文件的渲染，您可使用 glob 表达式来匹配路径。    

# Writing
new_post_name: :title.md  # 新文章的文件名称
default_layout: post      # 预设布局
titlecase: false          # 把标题转换为 title case
external_link: true       # 在新标签中打开链接
filename_case: 0          # 把文件名称转换为 (1) 小写或 (2) 大写
render_drafts: false      # 是否显示草稿
post_asset_folder: false  # 是否启动 Asset 文件夹
relative_link: false      # 把链接改为与根目录的相对位址    
future: true              # 显示未来的文章
highlight:                # 内容中代码块的设置    
  enable: true            # 开启代码块高亮
  line_number: true       # 显示行数
  auto_detect: false      # 如果未指定语言，则启用自动检测
  tab_replace:            # 用 n 个空格替换 tabs；如果值为空，则不会替换 tabs

# Category & Tag
default_category: uncategorized
category_map:       # 分类别名
tag_map:            # 标签别名

# Date / Time format
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD     # 日期格式
time_format: HH:mm:ss       # 时间格式    

# Pagination
## Set per_page to 0 to disable pagination
per_page: 10           # 分页数量
pagination_dir: page   # 分页目录

# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: landscape   # 主题名称

# Deployment
## Docs: https://hexo.io/docs/deployment.html
#  部署部分的设置
deploy:     
  type: '' # 类型，常用的git 
```

hexo的一些命令：

```bash
hexo n "我的博客" == hexo new "我的博客" #新建文章
hexo g == hexo generate #生成
hexo s == hexo server #启动服务预览
hexo d == hexo deploy #部署

hexo server #Hexo会监视文件变动并自动更新，无须重启服务器
hexo server -s #静态模式
hexo server -p 5000 #更改端口
hexo server -i 192.168.1.1 #自定义 IP
hexo clean #清除缓存，若是网页正常情况下可以忽略这条命令
```

## 部署到github

在github中创建一个repository，名字为`用户名.github.io`

```bash
cd dionysen
git config --global user.name Dionysen
git config --global user.email solongnight@outlook.com
ssh-keygen -t rsa -C solongnight@outlook.com #生成公匙
cat ~/.ssh/id_rsa.pub
```

复制公匙的内容，在github上，Setting — Developer settings — Personal access tokens 新建一个token

权限repo，生成一串密码：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

修改站点配置文件`_config.yml`

```bash
# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
  type: git 
  repository: https://<刚才生成的TOKEN>@github.com/<USERNAME>/<REPO>.git # < > 中的内容自己进行替换，< > 记得去掉。
  branch: main # 用 main 还是 master 随你，都行。
```

清理并部署：

```bash
hexo clean # 清理一下缓存，防止一些修改未生效
hexo g # 生成页面的命令
hexo d # 部署到 github远程仓库
hexo g -d #生成并部署
```

### Github仓库中没有README.md的解决方案

在source文件夹中建立`README.md`文档，然后修改`_config.yml`

```yml
skip_render： README.md
```

## 配置

Add links to the menu: Edit the `_config.yml` file of the theme, add `Categories: /categories` and `Tags: /tags` in `menu` like this：

```bash
menu:
  Home: /
  Archives: /archives
  Categories: /categories
  Tags: /tags
```

如果你需要为文章添加多个分类，可以尝试以下 list 中的方法。

```markdown
categories:
- [Diary, PlayStation]
- [Diary, Games]
- [Life]
```

此时这篇文章同时包括三个分类： `PlayStation` 和 `Games` 分别都是父分类 `Diary` 的子分类，同时 `Life` 是一个没有子分类的分类。

## 主题配置

选用极简风格的`Chic`主题。

### 配置文件

```bash
# Header
navname: DIONYSEN

# navigatior items
nav:
  ARCHIVE: /archives
  CATEGORY: /category
  TAGS: /tag
  ABOUT: /about

# favicon
favicon: /icon.svg

# Profile
nickname: Sincere and Fearless
### this variable is MarkDown form.
description: It is this intellectual activity of inquiry, seeking, rather than summative answers, that <br>make one a philosopher, because summative answers can easily be reduced to unthinking <br>dogmas and slogans that require no thought or understanding at all.

avatar: /image/avatar.jpeg


# main menu navigation
## links key words should not be changed.
## Complete url after key words.
## Unused key can be commented out.
links:
  Blog: /archives
  # Category:
  # Tags: 
  # Link:
  # Resume:
  # Publish:
  # Trophy:
  # Gallery:
  # RSS:
  # AliPay:
  # ZhiHu: https://www.zhihu.com/people/sirice
  # LinkedIn:
  # FaceBook:
  # Twitter:
  # Skype:
  # CodeSandBox:
  # CodePen:
  # Sketch:
  # Gitlab:
  # Dribbble:
  # YouTube:
  # QQ:
  # Weibo:
  # WeChat:
  Github: https://github.com/dioysen

# how links show: you have 2 choice--text or icon.
links_text_enable: false
links_icon_enable: true

# Post page
## Post_meta
post_meta_enable: true

post_author_enable: true
post_date_enable: true
post_category_enable: true
## Post copyright
post_copyright_enable: true

post_copyright_author_enable: true
post_copyright_permalink_enable: true
post_copyright_license_enable: true
post_copyright_license_text: Copyright (c) 2019 <a href="http://creativecommons.org/licenses/by-nc/4.0/">CC-BY-NC-4.0</a> LICENSE
post_copyright_slogan_enable: false
post_copyright_slogan_text: Do you believe in <strong>DESTINY</strong>?
## toc
post_toc_enable: true

# Page
page_title_enable: true

# Date / Time format
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
date_format: MMMM D, YYYY
time_format: H:mm:ss

# stylesheets loaded in the <head>
stylesheets:
  - /css/style.css

# scripts loaded in the end of the body
scripts:
  - /js/script.js
  - /js/tocbot.min.js
    # tscanlin/tocbot: Build a table of contents from headings in an HTML document.
    # https://github.com/tscanlin/tocbot


# plugin functions
## Mathjax: Math Formula Support
## https://www.mathjax.org
mathjax:
  enable: true
  import: demand # global or demand
  ## global: all pages will load mathjax,this will degrade performance and some grammers may be parsed wrong.
  ## demand: Recommend option,if your post need fomula, you can declare 'mathjax: true' in Front-matter
```

### 修改代码块样式

编辑`hexo-dir/themes/Chic/source/css/_page/_post/post_code.styl`:

```stylus
.post-content
  code, pre
    line-height 1.7em
    padding 7px
    font-size 14px
    font-family 'Source Code Pro'
```

### 多级分类

主题默认的分类只有一级，修改`hexo-dir/themes/Chic/layout/_page/category`为：

```ejs
<%# single category page%>
<% if (site.categories.length){ %>
<div class="container">
 <div class="post-wrap categories">
        <h2 class="post-title">-&nbsp;Categories&nbsp; - &nbsp;<%-page.category%> -</h2>
        <%- list_categories(site.categories) %> 
    </div>
 <%- partial('archive', {pagination: config.category, index: true}) %>
</div>
<% } %> 
```

使用hexo封装好的函数`list_catrgories()`。

## Watch

```bash
hexo g -w
hexo s
```

Execute these commands in deferent tty and you can see immediate results as you modifying.

## picgo

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



## 字体问题

个性化的字体需要自定义字体样式，加载云端字体或本地字体，汉字