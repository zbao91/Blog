---
title: "Hugo博客的搭建"
date: 2020-01-17T18:23:30+08:00
draft: false
author: Dave
tags: ["Tech"]
categories: ["tech"]
---

## 前言
岁月不饶人啊，辞职也有一段时间了，找工作的焦虑感也越来越强烈，为了找工作我第一次开始刷一些之前不曾会看的题目。也是为了记录和总结，也为了给自己加深印象，我搭建了这个博客。这篇文章主要是分享一下搭建的步骤和踩过的坑，给同样想要搭建Hugo博客的小伙帮一点帮助。同样分享下自己从网上整理的各大互联网公司的面试题。
<!--more-->

## Hugo安装
虽然Hugo是Golang编写的，但是官网做了提示：
> There is lots of talk about “Hugo being written in Go”, but you don't need to install Go to enjoy Hugo. Just grab a precompiled binary!

所以，其实你并不用下载Go语言就可以安装Hugo。由于我的电脑是Mac，所以可以用Homebrew直接安装，下面我以Mac OS为例进行：
### Step 1:  Homebrew源切换
Homebrew的源是部署在github上的，但是在国内访问特别慢，所以没有切换源的小伙伴可以切换一下来增加下载速度，切换过的小伙伴可以忽略这个步骤。具体步骤如下：

```bash
# 替换brew.git:
$ cd "$(brew --repo)"
# 中国科大:
$ git remote set-url origin https://mirrors.ustc.edu.cn/brew.git
# 清华大学:
$ git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git

# 替换homebrew-core.git:
$ cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
# 中国科大:
$ git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
# 清华大学:
$ git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git

# 替换homebrew-bottles:
# 中国科大:
$ echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.bash_profile
$ source ~/.bash_profile
# 清华大学:
$ echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles' >> ~/.bash_profile
$ source ~/.bash_profile

# 应用生效:
$ brew update
```
### Step 2: 安装Hugo
官方提供的安装文档的地址如下：https://gohugo.io/getting-started/installing/#binary-cross-platform。
使用不同平台的小伙伴可以根据自己的实际情况进行安装，
``` bash
# 安装
$ brew install hugo
==> Downloading https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/bottles/hu
######################################################################## 100.0%
==> Pouring hugo-0.62.2.catalina.bottle.tar.gz
==> Caveats
Bash completion has been installed to:
  /usr/local/etc/bash_completion.d
==> Summary
🍺  /usr/local/Cellar/hugo/0.62.2: 41 files, 68.2MB
```
目录结构
```
blog
|--- archetypes
|		└── default.md
|--- config.toml # 配置文件
|--- content
|     └── posts # 文章
|--- data
|--- data
|--- layouts
|--- themes # 主题文件夹

```

### Step 3: 创建一个网站
```bash
$ hugo new site MyBlog
```
它会自动在/Users/myname/MyBlog目录下创建博客文件
### Step 4: 添加主题
我的主题是从hugo的themes里面找的，名字叫`hello-friend-ng`，一个很简约的主题，比较符合我的审美，不过主题这玩意萝卜青菜更有所爱，大家找到自己喜欢的就好。
```bash
$ git init
$ git submodule add https://github.com/rhazdon/hugo-theme-hello-friend-ng.git themes/hello-friend-ng
```
然后根据自己的实际情况，对配置文件进行修改。比如：
```tmol
theme = "hello-friend-ng"
```
### Step 5: 启动Hugo服务
启动命令
```bash
$ hugo server -D
```
默认端口是1313，铛铛！博客就成功启动啦！
## 在Github Pages进行搭建
github page的页面托管服务，虽然访问速度并不快，但是免费呀！下面介绍一步步进行网页部署：
### Step 1: 创建Repository
在gitub上分别创建, `<username>.github.io`和`blog`的两个Repository。
### Step 2: 同步文件到github
创建完毕后，将blog项目同步到本地，并且进行第一次提交：
```bash
$ git clone https://github.com/<username>/blog.git # 同步rep到本地
$ mv hugo_blog/* blog/
$ cd blog
$ git add .
$ git commit -m "Initial commit for our Hugo site."
$ git submodule add https://github.com/<username>/<username>.github.io.git <username>.github.io.git
$ git add .
$ git commit -m "Initial commit for our generated HTML."
$ git push -u origin master
```
### Step 3: 修改配置文件
然后修改`config.toml`文件，修改参数`baseURL`和`publishDir`：
```toml
baseURL = "https://<username>.github.io/"
publishDir   = "<username>.github.io"
```
> 注意！！！  
> 这里的配置文件一定要修改正确，不然网页是无法在github pages上成功显示的！！
### Step 4: 推送网页文件
``` bash
$ hugo # 会在<username>.github.io文件夹生成html、xml等文件
$ cd <username>.github.io
$ git add .
$ git commit -m "First commit"
$ git push origin master
```
好了，正常情况下过不到1分钟打开就能看到你自己的博客网站啦！
### Step 5: 推送文章
考虑到有的小伙伴对git这个工具不是很熟悉，下面的步骤是为这些小伙伴准备的。当你写了一篇新的博客的时候，要把这篇文章单独提交，这个时候就不能使用`git add .`，这样会将这个路径下所有的文件重新提交，容易遇到冲突的问题。下面是如何操作单独提交MarkDown文档：
``` bash
$ git status # 查看当前项目下，哪些地方做了修改
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   content/.DS_Store
	modified:   content/tech/hugo-setup.md
# 从上面看的处，hugo-setup.md做了修改，所以只要单独提交这个文件就行了
$ git add content/tech/hugo-setup.md
$ git commit -m "new commit"
$ git push origin master
```
有时候你的博客会长时间没有刷新你改动的地方，那么可以重新提交`<username>.github.io`下的静态网页文件。
```bash
$ cd <username>.github.io
$ rm -rf *
$ cd ..
$ hugo
$ cd <username>.github.io
$ git add .
$ git commit -m "rebuild commit"
$ git push origin master
```
好了，希望对有需要的朋友有所帮助，我也会在接下来的时候录制一个博客部署的视频。谢谢！

## 总结
博客只是一个媒介，一个平台。不管你的皮肤多么好看，功能多么齐全，最核心的部分还是你的内容，只有持续不断写作才是真正有意义的事情。共勉！！！

---
Ref:
* https://inside.getambassador.com/creating-and-deploying-your-first-hugo-site-to-github-pages-1e1f496cf88d
* https://www.jianshu.com/p/8a2ac505ff3e

