---
layout: post
title: How to bind mail in MacOS
date: 2025-3-15 17:00:00 +08:00
description: # Add post description (optional)
img: # Add image post (optional)
tags: [Configuration] # add tag
---

# outlook
最简单，添加账户，选择Microsoft Exchange，输入outlook邮箱的账户密码即可

# qq邮箱
去qq邮箱-账户设置-POP3/IMAP/SMTP/Exchange/CardDAV 服务（已开启）

开启这项服务，会要求你用发一个短信，发完后会有个验证码，输入qq邮箱的地址，密码输入刚生成的验证码即可。

**注意，密码不是自己的邮箱密码**，否则会提示添加邮箱无法验证。

# 教育邮箱
以ecnu的教育邮箱为例，登入学校的邮箱，选择设置-微信绑定-客户端专用密码-生成新密码

然后在Mac-邮件-

接收服务器：
imap.exmail.qq.com(使用SSL，端口号993)
发送服务器：
smtp.exmail.qq.com(使用SSL，端口号465)