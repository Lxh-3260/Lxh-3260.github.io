---
layout: post
title: Goland strategy
date: 2025-4-23 16:00:00 +08:00
description: # Add post description (optional)
img: # Add image post (optional)
tags: [Configuration] # add tag
---

# Goland如何设置鼠标点击文件，项目目录自动跳转到所选文件
- 问题：protobuf生成函数文件时，在单机跳转会跳到错误的地方
- 解决方法：Goland-Settings- Editor-General-Code Folding，打开Custom folding regions
- 结果：点击代码跳转时，由proto生成的文件，就不会再跳转到proto定义处，而是跳转到代码使用的地方

# Goand设置鼠标点击文件，项目目录自动跳转到所选文件
- 问题：单机函数跳转，但是目录不跳转，导致无法对项目目录有个清楚的认知
- 解决方法：Goland-双击shift搜索，打开Always Select Opened File
- 结果：单机函数跳转，目录也会跳转