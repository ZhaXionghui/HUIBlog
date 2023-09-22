# 1. 引言

> 参考：[个人博客搭建教程 | 爱扑bug的熊 (cuijiacai.com)](https://blog.cuijiacai.com/blog-building/)

## 1.1 工具系统

这个教程使用的个人博客框架是hexo，博客文件拖管于github，博客网站用netlify生成，国内访问采用cloudflare进行CDN加速。使用这个解决方案的原因是不需要云服务器，也不需要备案，只需要有一个域名即可。

下面，我们来一步一步介绍我们即将使用的工具及其基本用法。

我个人的系统环境是Windows，后续的教程都基于Windows的终端及其包管理软件`cmd`进行。



# 2. hexo博客框架

## 2.1 介绍

`hexo`是一个基于nodejs的静态博客网站生成器，作者是来自台湾的`Tommy Chen`，为许多技术博客的博主所青睐，主要有如下的一些优点：

- 支持`Markdown`语法，编辑简单，排版优美；
- 能够快速生成静态html文件；
- 部署容易，接口简单；
- 兼容于各大主流操作系统；
- 社区主题、插件很多，遇到问题的时候能查到的参考材料也很多。



## 2.2 环境配置

搭建hexo首先需要有[nodejs](https://nodejs.org/zh-cn/)的环境，可以从官网直接下载。
[![nodejs](.\img\2xKvaPTDoyZcFbp.png)](https://s2.loli.net/2022/09/05/2xKvaPTDoyZcFbp.png)

当然，你也可以直接通过终端下的包管理软件一键安装，比如说我的环境是macOS，使用`brew`进行包管理，可以通过如下命令安装。



> Linux类似，用包管理软件即可。Windows的话需要从官网下载安装，安装完成后建议用git-bash（在安装git的时候应该已经同时安装了，如果没有的话可以去[官网](https://git-scm.com/)下载）来进行后续的操作，因为windows和unix的命令体系有差别。

最后查看版本信息以确认环境配置信息：

```bash
node -v # 查看node版本信息
npm -v # 查看npm版本信息
```

能够正常查看到版本信息，说明环境配置就已经完成了。

这里还需要提一下的是`npm`默认的官网源可能会比较慢，如果想要后续的下载速度快一些，可以通过下面的方式将源设置为淘宝源。

```bash
npm config get registry # 查看原来的源
npm config set registry https://registry.npm.taobao.org # 修改为淘宝源
npm config get registry # 查看现在的源
```

## 2.3 生成博客

### 2.3.1 安装

有了`npm`包管理软件，安装`hexo`就很方便了，只需要一行命令：

```bash
npm install hexo-cli -g # 全局安装hexo命令行工具
```

其中`-g`参数表示全局安装，没有这个参数就只在当前目录下安装，建议全局安装。

### 2.3.2 初始化

运行命令：

```
hexo init "ZXH Blog" # 目录名称不含空格的时候双引号可以省略
```

得到如下的反馈信息：

```
INFO  Cloning hexo-starter https://github.com/hexojs/hexo-starter.git
INFO  Install dependencies
# 一些可能的中间信息
INFO  Start blogging with Hexo!
```

然后进入博客目录：

```bash
cd "ZXH Blog"
```

安装博客需要的其他支持：

```bash
npm install # 安装的依赖项在package.json文件的dependencies字段中可以看到
```

### 2.3.3 博客项目目录结构介绍

查看目录结构：

```bash
tree -L 1
```

结果如下：

```
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

各部分的含义：

- `_config.yml`

  - 为全局配置文件，网站的很多信息都在这里配置，比如说网站名称，副标题，描述，作者，语言，主题等等。具体可以参考官方文档：https://hexo.io/zh-cn/docs/configuration.html。

- ```
  scaffolds
  ```

  - 骨架文件，是生成新页面或者新博客的模版。可以根据需求编辑，当`hexo`生成新博客的时候，会用这里面的模版进行初始化。

- ```
  source
  ```

  - 这个文件夹下面存放的是网站的`markdown`源文件，里面有一个`_post`文件夹，所有的`.md`博客文件都会存放在这个文件夹下。现在，你应该能看到里面有一个`hello-world.md`文件。

- ```
  themes
  ```

  - 网站主题目录，`hexo`有非常丰富的主题支持，主题目录会存放在这个目录下面。
  - 我们后续会以默认主题来演示，更多的主题参见：https://hexo.io/themes/

### 2.3.4 生成新文章

```bash
hexo new post "test" # 会在 source/_posts/ 目录下生成文件 ‘test.md’，打开编辑
hexo generate        # 生成静态HTML文件到 /public 文件夹中
hexo server          # 本地运行server服务预览，打开 http://localhost:4000 即可预览你的博客
```

本地预览效果：

[![preview](.\img\jvikZ3Yu5mqBxPl.png)](https://s2.loli.net/2022/09/05/jvikZ3Yu5mqBxPl.png)

这是`hexo`的默认主题，更多的主题可以从官网下载。

更详细的`hexo`命令可以查看文档：https://hexo.io/zh-cn/docs/commands

### 2.3.5 添加建站脚本

为了后续`netlify`建站方便，我们可以在`package.json`里面添加一个命令：

```json
{
    // ......
    "scripts": {
        "build": "hexo generate",
        "clean": "hexo clean",
        "deploy": "hexo deploy",
        "server": "hexo server",
        "netlify": "npm run clean && npm run build" // 这一行为新加
    },
    // ......
}
```

### 2.3.6 博客配置

由于我们这个教程的重点不是如何编写自己的博客，而是怎么把博客搭建起来，所以`hexo`的细节用法以及各种主题的设置我们就不展开细说了，官网有非常详细的文档和教程可供参考。这里简单提一下`_config.yml`的各个字段的含义：

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





# 3. Github项目文件托管



[![git-github](.\img\bXDOiVtTygAH1CM.jpg)](https://s2.loli.net/2022/09/05/bXDOiVtTygAH1CM.jpg)

这一步非常简单，`git`和`github`的基本用法我就不赘述了。创建本地仓库，然后推送到远端服务器即可：

```bash
cd "ZXH Blog"
git init
git add .
git commit -m "my blog first commit"
git remote add origin "远端github仓库地址" 
git branch -M main
git push -u origin main
```











