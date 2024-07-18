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

<!-- ![Macbook]({{site.baseurl}}/assets/img/mac.jpg) -->

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

2. 配置jekyll环境首先需要下载ruby，mac自带了一个ruby，但是版本太低不能使用，可以`command+space`输入terminal打开终端，输入`ruby -v`查看ruby版本，发现是2.x版本，所以需要自己下载一个3.x的ruby。执行，会报错`zsh: command not found: brew`，这是因为没有下载brew包管理器，大陆地区直接下载brew会报网络错误，这里就要用到代理了，改为执行就可以正常下载brew了。
```shell
# 下载ruby，提示没有brew包管理语句
brew install ruby
# 下载brew后再下载一次ruby
ALL_PROXY=http://127.0.0.1:7890 /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
# 将brew加入环境变量
(echo; echo 'eval "$(/opt/homebrew/bin/brew shellenv)"') >> /Users/xinghaoli/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
# 验证brew是否生效
brew help
# 下载ruby
brew install ruby
# 出现提示，如果要使用新安装的ruby需要执行这个命令行语句，将其加入环境变量：
echo 'export PATH="/opt/homebrew/opt/ruby/bin:$PATH"' >> ~/.zshrc
# 使环境变量生效
source ～/.zshrc
```

3. 下载jekyll，这里用的镜像源下载的，否则会超时
```shell
# 将RubyGems 的源替换为国内镜像，以解决下载超时
gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/
gem sources -l
# 安装jekyll
gem install --user-install bundler jekyll
# 修改gem的运行环境
echo 'export PATH="$HOME/.gem/ruby/3.3.0/bin:$PATH"' >> ~/.zshrc
echo 'export PATH="$HOME/.local/share/gem/ruby/3.3.0/bin:$PATH"' >> ~/.zshrc
source ～/.zshrc
# 确认gem环境已安装
gem environment
jekyll --version
```

4. 新建Jekyll项目，并安装bundle依赖项（这里需要换源），启动Jekyll项目后就可以在本地的4000端口及时地查看修改了
```shell
# 将 RubyGems 的源更换为国内镜像
bundle config mirror.https://rubygems.org https://gems.ruby-china.com
# 安装 Gemfile 中指定的所有依赖
bundle install
# 新建jekyll项目
jekyll new site
# 启动jekyll项目
bundle exec jekyll serve --trace
# 有时有奇怪的错误，可以清一下jekyll缓存
bundle exec jekyll clean
```

5. 去[Jekyll官网](http://jekyllthemes.org/)找一个模版，点击demo可以查看每个模版的demo，download可以下载其源码，解压到本地，运行模版并把模版修改成自己想要的样子即可，我用的是[Flexible Jekyll](http://jekyllthemes.org/themes/flexible-jekyll/)这个模版。将源码push到远程的github.io仓库，github会自动执行github-pages deployments，再次访问网页即可完成部署。

6. 拓展：Jekyll的模版一般有几个模块，常用的有如下几个模块:
* _config.yaml:用于进行一些模块基础信息配置
* _layouts:每个模块的布局，例如post模块、main模块
* _posts:每一篇博客就是一篇post，按照模版的样式新建post即可
* assets:存放图片资源、各个模块的css和字体资源
>值得一提的是，如果想实现评论功能，需要去注册一个disqus，并在_config.yaml中配置相关的文件。
如果想实现浏览访问计数，需要用到百度的api，这里我并没有实现，故不做赘述。

1. 如果想在博客中展示公式，那么需要额外引入script脚本。
```yml
# 在_config.yaml中加入下面几行配置
# Build Settings
markdown: kramdown
kramdown:
  input: GFM
  math_engine: mathjax
  mathjax:
    cdn: //cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js
```
```javascript
// 在_include/head.html中head模块加入下面这一行
<script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js" defer></script>
```
```shell
# 重新运行网页，加载script脚本
bundle exec jekyll serve
```