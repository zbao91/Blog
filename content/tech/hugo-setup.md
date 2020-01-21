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

所以，其实你并不用下载Go语言就可以安装Hugo。
### Step 1:  Homebrew源切换
Homebrew的源是部署在github上的，但是在国内访问特别慢，所以没有切换源的小伙伴可以切换一下。具体步骤如下：

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

_Ref：[https://www.jianshu.com/p/8a2ac505ff3e][2]_
### Step 2: 安装Hugo
官方提供的安装文档的地址如下：[https://gohugo.io/getting-started/installing/#binary-cross-platform][3]
使用不同平台的小伙伴可以根据自己的实际情况进行安装，下面我就以MacOS为例。
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
我的主题是从hugo的themes里面找的，名字叫`hello-friend-ng`，一个很简约的主题。
```bash
$ git init
$ git submodule add https://github.com/rhazdon/hugo-theme-hello-friend-ng.git themes/hello-friend-ng
```
然后根据自己的实际情况，对配置文件进行修改。
### Step 5: 启动Hugo服务
启动命令
```bash
$ hugo server -D
```
默认端口是1313，铛铛！博客就成功启动啦！
## 上传到github
github支持免费的托管服务，直接把


---

