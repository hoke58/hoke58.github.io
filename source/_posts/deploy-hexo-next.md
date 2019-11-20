---
title: Hexo+NexT打造极简个人博客[持续更新]
date: 2019-10-28 18:39:05
categories: Web
tags: [Hexo, NexT, Github, Coding]
---

# 前言
`markdown`记录, 纯静态, `git`免费托管个人网站, 拥有网上个人小窝so easy, 来吧和我一起折腾吧！

## 环境与工具
  * OS: Deepin 15.11
  * Node.js: v12.13.0
  * Npm: 6.12.0
  * Hexo: 4.0.0
  * NexT: 7.4.2
  * IDE: VS Code

# Node.js
[Node.js官网下载页面](https://nodejs.org/zh-cn/download/)选择自己的OS版本下载部署包, 推荐长期支持版（LTS）, 因为是我Linux环境, 选择**Linux 二进制文件 (x64)**, 以下是我的安装步骤
1. 下载二制包并解压
```shell
[root@VM-202 ~]$ wget https://nodejs.org/dist/v12.13.0/node-v12.13.0-linux-x64.tar.xz
[root@VM-202 ~]$ sudo tar -xJvf node-v12.13.0-linux-x64.tar.xz -C /usr/local/lib
[root@VM-202 ~]$ sudo vi /etc/profile
```
2. 配置`/etc/profile`, 在最后增加, 确保路径正确
```vim
export PATH=/usr/local/lib/node-v12.13.0-linux-x64/bin:$PATH
```
3. 重载`/etc/profile`, 测试效果
```shell
[root@VM-202 ~]$ sudo source /etc/profile
[root@VM-202 ~]$ npm version
{
  npm: '6.12.0',
  ares: '1.15.0',
  brotli: '1.0.7',
  cldr: '35.1',
  http_parser: '2.8.0',
  icu: '64.2',
  llhttp: '1.1.4',
  modules: '72',
  napi: '5',
  nghttp2: '1.39.2',
  node: '12.13.0',
  openssl: '1.1.1d',
  tz: '2019a',
  unicode: '12.1',
  uv: '1.32.0',
  v8: '7.7.299.13-node.12',
  zlib: '1.2.11'
}
```
4. 配置国内镜像
```shell
[root@VM-202 ~]# npm config set registry https://registry.npm.taobao.org

# 配置后可通过下面方式来验证是否成功
[root@VM-202 ~]# npm config get registry
```
## 升级Node.js和NPM（可选）
1. 升级**Node.js**
`npm`中有一个模块叫做`n`, 专门用来管理**Node.js**版本的。
```shell
[root@VM-202 ~]# npm install -g n
[root@VM-202 ~]# n --help

Usage: n [options] [COMMAND] [args]

Commands:

  n                              Display downloaded node versions and install selection
  n latest                       Install the latest node release (downloading if necessary)
  n lts                          Install the latest LTS node release (downloading if necessary)
  n <version>                    Install node <version> (downloading if necessary)
  n run <version> [args ...]     Execute downloaded node <version> with [args ...]
  n which <version>              Output path for downloaded node <version>
  n exec <vers> <cmd> [args...]  Execute command with modified PATH, so downloaded node <version> and npm first
  n rm <version ...>             Remove the given downloaded version(s)
  n prune                        Remove all downloaded versions except the installed version
  n --latest                     Output the latest node version available
  n --lts                        Output the latest LTS node version available
  n ls                           Output downloaded versions
  n ls-remote [version]          Output matching versions available for download
  n uninstall                    Remove the installed node and npm

Options:

  -V, --version   Output version of n
  -h, --help      Display help information
  -q, --quiet     Disable curl output (if available)
  -d, --download  Download only
  -a, --arch      Override system architecture
  --all           ls-remote displays all matches instead of last 20
  --insecure      Turn off certificate checking for https requests (may be needed from behind a proxy server)

Aliases:

  which: bin
  run: use, as
  ls: list
  lsr: ls-remote
  rm: -
  lts: stable
  latest: current

Versions:

  Numeric version numbers can be complete or incomplete, with an optional leading 'v'.
  Versions can also be specified by label, or codename,
  and other downloadable releases by <remote-folder>/<version>

    4.9.1, 8, v6.1    Numeric versions
    lts               Newest Long Term Support official release
    latest, current   Newest official release
    boron, carbon     Codenames for release streams
    and nightly, chakracore-release/latest, rc/10 et al
```
安装最新`LTS`版**Node.js**
```
[root@VM-202 ~]#n lts
```
2. 升级**NPM**
```shell
[root@VM-202 ~]# npm -g install npm
/usr/local/bin/npm -> /usr/local/lib/node_modules/npm/bin/npm-cli.js
/usr/local/bin/npx -> /usr/local/lib/node_modules/npm/bin/npx-cli.js
+ npm@6.13.0
added 2 packages from 2 contributors, removed 2 packages and updated 15 packages in 7.73s
```
3. 注释`/etc/profile`中老版本的配置
```
#export PATH=/usr/local/lib/node-v12.13.0-linux-x64/bin:$PATH
```

> **windows**的童鞋可参考[菜鸟教程](http://www.runoob.com/nodejs/nodejs-install-setup.html)

# Git
**Linux**系统下安装Git非常简单, 用包管理命令: 
```shell
sudo apt-get install git

yum install git
```
**windows**下就直接到*[Git官网](https://git-scm.com/download/win)* 下载安装即可

安装完成后执行 `git --version` 验证是否成功
```shell
[root@VM-202 ~]# git --version
git version 2.17.0
```
# Hexo
安装**Hexo**只需一条命令, 详细移步至[官档](https://hexo.io/zh-cn/docs/), 重点说常用命令和配置
```
npm install -g hexo-cli
```
## 常用命令
```shell
hexo init <folder> # 初始化一个网站. 如果没有设置 folder , Hexo 默认在目前的文件夹建立网站
hexo new <layout> <title> # 新建一篇文章. 如果没有设置 layout 的话, 默认使用 _config.yml 中的 default_layout 参数代替. 如果标题包含空格的话, 请使用引号括起来
hexo clean # 清除缓存文件 (db.json) 和已生成的静态文件 (public)
hexo g # 等于hexo generate # 生成静态文件
hexo s # 等于hexo server # 本地预览
hexo d # 等于hexo deploy # 部署, 可与hexo g合并为 hexo d -g
hexo version # 查看版本
```
## 配置文件
> 注意`_config.yml`的区分：`hexo init`生成的目录下的`_config.yml`为<font color=red>Hexo配置文件</font>, `themes`目录下主题文件夹里的 `_config.yml`为<font color=red>主题配置文件</font>, 如`themes\next\_config.yml`。

我的Hexo配置文件`_config.yml`
```yml
title: Hoke's blog
subtitle: 何可的博客
description: 一名追求简单实用的运维开发工程师
keywords: 何可, hoke, hoke58, hoke58.cn, 博客, 个人博客, 个人网站, devops, 运维, 运维开发
author: Hoke
language: zh-CN
timezone: Asia/Shanghai
url: http://www.hoke58.cn
root: /
permalink: :category/:title.html
permalink_defaults:
pretty_urls:
  trailing_index: false # Set to false to remove trailing index.html from permalinks
source_dir: source
public_dir: public
tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code
i18n_dir: :lang
skip_render:
new_post_name: :title.md # File name of new posts
default_layout: post
titlecase: false # Transform title into titlecase
external_link:
  enable: true # Open external links in new tab
  field: site # Apply to the whole site
  exclude:
filename_case: 0
render_drafts: false
post_asset_folder: false # 启动asset文件夹
relative_link: false
future: true
highlight:
  enable: true
  line_number: true
  auto_detect: true
  tab_replace:
index_generator:
  path: ''
  per_page: 10
  order_by: -date
default_category: uncategorized
category_map:
  系统运维: systemops
  DevOps: devops
  Web: web
  数据库: databases
  Docker: docker
  开发: development
  监控: monitor
tag_map:
  
meta_generator: true
date_format: YYYY-MM-DD
time_format: HH:mm:ss
use_date_for_updated: false
per_page: 10
pagination_dir: page
theme: next
symbols_count_time:
  symbols: true
  time: true
  total_symbols: true
  total_time: true
deploy:
  type: git
  repo: https://github.com/hoke58/hoke58.github.io
  branch: master
```
我的Hexo应用程序信息`package.json`
```json
{
  "name": "hexo-site",
  "version": "0.0.0",
  "private": true,
  "scripts": {
    "build": "hexo generate",
    "clean": "hexo clean",
    "deploy": "hexo deploy",
    "server": "hexo server"
  },
  "hexo": {
    "version": "4.0.0"
  },
  "dependencies": {
    "hexo": "^4.0.0",
    "hexo-generator-archive": "^1.0.0",
    "hexo-generator-category": "^1.0.0",
    "hexo-generator-index": "^1.0.0",
    "hexo-generator-tag": "^1.0.0",
    "hexo-related-popular-posts": "^3.0.6",
    "hexo-renderer-ejs": "^1.0.0",
    "hexo-renderer-marked": "^2.0.0",
    "hexo-renderer-stylus": "^1.1.0",
    "hexo-server": "^1.0.0",
    "hexo-symbols-count-time": "^0.6.3"
  }
}
```

# NexT
使用 `Hexo` 也是因为喜欢 `NexT` 模板, 简单漂亮。本章节主要介绍安装、配置、自定义页面模板

## 安装
最简单的安装方式是直接克隆整个仓库：

```sh
$ cd <hexo-init-folder>
$ git clone https://github.com/theme-next/hexo-theme-next themes/next
```
此外, 如果你想要使用其他方式, 你也可以参见[详细安装步骤](https://github.com/theme-next/hexo-theme-next/blob/master/docs/zh-CN/INSTALLATION.md)。

## 配置

在开始之前需要先将 Hexo 默认的主题配置改为 NexT

打开 **Hexo 配置文件**, 找到 `theme` 字段, 并将其值更改为 `next`
```yml
theme: next
```
然后 `hexo clean && hexo g -d && hexo s` 即可预览 NexT 默认的主题效果

### 数据文件
当使用 `git pull` 更新 NexT 主题时经常需要解决冲突问题, 而在手动下载 release 版本时也经常需要手动合并配置, 所以为了解决版本冲突的问题我们使用 NexT 的数据文件

如果在新的 release 中出现了任何新的选项, 那么你只需要从 `/themes/next/_config.yml` 中将他们复制到 `/source/_data/next.yml` 中并设置它们的值为你想要的选项。

#### 用法

1. 请确认你的 Hexo 版本为 3.0 或更高。
2. 在你站点的 `/source/_data` 目录创建一个 `next.yml` 文件（如果 `_data` 目录不存在, 请创建之）。

<p align="center">以上步骤之后有 <b>两种选择</b>, 请<b>任选其一</b>然后<b>继续后面的步骤</b>。</p>

* **选择 1：`override: false`（默认）**：

  1. 检查默认 NexT 配置中的 `override` 选项, 必须设置为 `false`。\
     在 `next.yml` 文件中, 也要设置为 `false`, 或者不定义此选项。
  2. 从站点配置文件（`/_config.yml`）与主题配置文件（`/themes/next/_config.yml`）中复制你需要的选项到 `/source/_data/next.yml` 中。

* **选择 2：`override: true`**：

  1. 在 `next.yml` 中设置 `override` 选项为 `true`。
  2. 从 `/themes/next/_config.yml` 配置文件中复制**所有**的 NexT 主题选项到 `/source/_data/next.yml` 中。

3. 然后, 在站点的 `/_config.yml` 中需要定义 `theme: next` 选项（如果需要的话, `source_dir: source`）。
4. 使用标准参数来启动服务器, 生成或部署（`hexo clean && hexo g -d && hexo s`）。


最佳实践为**选择 1** , copy 需要修改的选项至 `next.yml` , 配置文件中的注释说明基本够用, 不清楚地参考[NexT官方文档](http://theme-next.iissnan.com/getting-started.html)和[NexT github](https://github.com/iissnan/hexo-theme-next), `NexT github`托管项目 `docs` 目录下有markdown文档, 有中文版的哦。

### ~~自定义 scheme~~
{% note info %}
不建议自定义，如果官方对 scheme 有更新，需要人工检查原 scheme 的修改内容，再更新至新增的自定义 scheme
{% endnote %}

~~由于我对 scheme 修改较多, 怕 `git pull` 更新时报冲突, 干脆直接新增专用 scheme , 以下是基本思路：~~

1. 选择中意的 schemes 我是基于 Mist 基础上修改的, 在`/themes/next/source/css/_schemes`下复制 `Mist` 为 `Hoke` （自定义）
2. `/themes/next/source/css/_variables`下 复制 `Mist.styl` 为 `Hoke.styl`
3. 全局搜索 `Mist` 在匹配到的文件中按照 `Mist` 新增 `Hoke` 部分
```bash
[root@VM-202 /home/hoke/Documents/blog/themes/next]# git diff scripts/filters/minify.js 
diff --git a/scripts/filters/minify.js b/scripts/filters/minify.js
index 6640242..6238bbc 100644
--- a/scripts/filters/minify.js
+++ b/scripts/filters/minify.js
@@ -50,4 +50,7 @@ hexo.extend.filter.register('after_generate', () => {
   } else if (theme.scheme === 'Pisces' || theme.scheme === 'Gemini') {
     hexo.route.remove('js/schemes/muse.js');
   }
+  if (theme.scheme === 'Hoke') {
+    hexo.route.remove('js/local-search.js');
+  }
 });

[root@VM-202 /home/hoke/Documents/blog/themes/next]# git diff source/css/_common/outline/header/github-banner.styl 
diff --git a/source/css/_common/outline/header/github-banner.styl b/source/css/_common/outline/header/github-banner.styl
index 42a433a..434c08d 100644
--- a/source/css/_common/outline/header/github-banner.styl
+++ b/source/css/_common/outline/header/github-banner.styl
@@ -47,7 +47,7 @@
     }
   }
 
-  if ($scheme == 'Mist') {
+  if ($scheme == 'Mist') || ($scheme.scheme === 'Hoke') {
     +mobile() {
       svg {
         top: inherit;


[root@VM-202 /home/hoke/Documents/blog/themes/next]# git diff source/js/utils.js 
diff --git a/source/js/utils.js b/source/js/utils.js
index a249585..754423f 100644
--- a/source/js/utils.js
+++ b/source/js/utils.js
@@ -335,6 +335,10 @@ NexT.utils = {
     return CONFIG.scheme === 'Mist';
   },
 
+  isHoke: function() {
+    return CONFIG.scheme === 'Hoke';
+  }, 
+
   isPisces: function() {
     return CONFIG.scheme === 'Pisces';
   },

 ```
4. `/source/_data/next.yml` 新增 scheme
```yml
#scheme: Muse
#scheme: Mist
#scheme: Pisces
#scheme: Gemini
scheme: Hoke
```
5. 执行 `hexo clean && hexo g && hexo s` 可预览效果, 如果遇报错别慌, 看一下报错的文件是不是哪个地方还漏改啦

成功之后就可以在自定义的 scheme 上折腾啦！

# 托管至 Git

安装 [hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)

``` bash
$ npm install hexo-deployer-git --save
```

## GitHub
[Github](https://github.com/) 需要开通 [Github Page](https://pages.github.com/) , 网上一搜一大把, 不啰嗦啦

打 Hexo 配置文件 `_config.yml` , 拉到底部, 修改部署配置: 
```yaml
deploy:
  type: git
  repo: https://github.com/hoke58/hoke58.github.io
  branch: master
```
执行 `hexo clean && hexo g -d` , 输入 Git 的账号和密码,完成后在浏览器输入 `yourName.github.io`, Enjoy it!

## Coding or Gitee
> Gitee Pages 免费版不支持自定义域名，放弃转 coding

登录 [Coding](https://coding.net) 创建一个同名账号的 repository ，在项目仓库内一键开通 Pages 服务

打 Hexo 配置文件 `_config.yml` , 拉到底部, 修改部署配置: 
```yaml
deploy:
  type: git
  repo: 
    github: https://github.com/hoke58/hoke58.github.io
    coding: https://git.coding.net/hoke58/hoke58.git
  branch: master
```
执行 `hexo clean && hexo g -d` , 输入 Git 的账号和密码, 完成后在浏览器输入 `yourName.coding.me` 测试
