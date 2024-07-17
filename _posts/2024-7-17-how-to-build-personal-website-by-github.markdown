---
layout: post
title: How to build personal website by github
date: 2024-07-17 10:29:27 +08:00
description: # Add post description (optional)
img: website.png # Add image post (optional)
tags: [Website, Software] # add tag
---

## 搭建前准备
1. 一个github账号
2. 操作系统：macos Sonoma 14.5
3. 科学上网（涉及一些包和依赖的下载，找镜像源也可以，直接执行`ALL_PROXY=http://127.0.0.1:7890`走代理下载更快，其中127.0.0.1是本机回环地址，7890是clash的端口号，其他代理客户端填自己对应的代理端口即可，例如v2rayN是10809）

![Macbook]({{site.baseurl}}/assets/img/mac.jpg)

## 搭建基础
1. 按照如下顺序github - your profile - repositories - new创建一个新的项目，其中repository name必须写自己的github账户名.github.io
>例如我的账户名是lxh-3260，则我的repository name应为lxh-3260.github.io

2. 浏览器里输入url：https://lxh-3260.github.io/，就可以访问界面了，可以把仓库拉取到本地（例如我的项目则为`git clone https://github.com/Lxh-3260/Lxh-3260.github.io.git`），然后在里面新建一个index.html的文件，在里面随便输入一段html的语句，保存后把代码推送到git上，再访问自己的网页，可以发现网页的内容展示了index.html中的内容（如果你只是想展示一些个人信息或者简历，那做到这已经结束了）。
>这里有可能出现git clone的网络错误，最好的办法就是走代理
```shell
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890
```

## Jekyll环境配置
1. 首先要介绍一下Jekyll，它提供了很多网站模板，可以让我们不用自己去设计个人网页的前端界面，同时还可以执行本地修改后直接预览，执行`bundle exec jekyll serve --trace`，可以在本机的浏览器输入`http://127.0.0.1:4000/`预览修改结果，不用等`git push`后执行流水线的deploy action操作，大大提高了网站维护的效率。同时它基本是开箱即用，不必预先学习jekyll语法，就能使得非前端程序员也能有一个美观的个人网页。

2. 配置jekyll环境首先需要下载ruby，mac自带了一个ruby，但是版本太低不能使用，可以`command+space`输入terminal打开终端，输入`ruby -v`查看ruby版本，发现时2.x，所以需要自己下载一个3.x的ruby。执行`brew install ruby`，会报错`zsh: command not found: brew`，这是因为没有下载brew包管理器，大陆地区直接下载brew`/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`会报网络错误，这里就要用到代理了，改为执行`ALL_PROXY=http://127.0.0.1:7890 /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)`就可以正常下载brew了。

3. 