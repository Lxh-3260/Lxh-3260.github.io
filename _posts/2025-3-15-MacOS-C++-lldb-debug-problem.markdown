---
layout: post
title: MacOS C++ lldb debug problem
date: 2025-3-15 21:00:00 +08:00
description: # Add post description (optional)
img: # Add image post (optional)
tags: [Configuration] # add tag
---

# lldb-mi
今天做笔试的时候，因为是ACM模式需要自己构建输入输出，当我在本地调试时发现，没法cin？具体表现为，卡在cin的地方没法单步调试，只能结束调试。

之前做lc的时候都是核心代码模式，所以没考虑过输入输出的debug，遂作罢回笔试界面用眼睛看哪里有问题。

问题来了，笔试做到一半，mac突然提示我内存不足，好家伙vscode占了50GB(lldb-mi你干的好事)

笔试结束之后找了一下原因，发现了[别人也有这个问题](https://github.com/microsoft/vscode-cpptools/issues/3829)，同时issue中指路[这篇文章](https://stackoverflow.com/questions/58329611/vscode-macos-catalina-doesnt-stop-on-breakpoints-on-c-c-debug/70991654#70991654)，讲解了解决方法，记录一下。

# 如何配置才能正确用VSC debug处理 cin

1. 首先在本地安装llvm@19，安装完后他会提示你添加环境变量
```shell
brew install llvm@19
echo 'export PATH="/opt/homebrew/opt/llvm@19/bin:$PATH"' >> ~/.zshrc
echo 'export LDFLAGS="-L/opt/homebrew/opt/llvm@19/lib"' >> ~/.zshrc
echo 'export CPPFLAGS="-I/opt/homebrew/opt/llvm@19/include"' >> ~/.zshrc
echo 'export PKG_CONFIG_PATH="/opt/homebrew/opt/llvm@19/lib/pkgconfig"' >> ~/.zshrc
source ~/.zshrc
```
2. 添加tasks.json
```json
{
	"tasks": [
		{
			"type": "cppbuild",
			"label": "C/C++: clang++ build active file",
			"command": "/usr/bin/clang++",
			"args": [
				"-std=c++23",
				"-fcolor-diagnostics",
				"-fansi-escape-codes",
				"-g",
				"${file}",
				"-o",
				"${fileDirname}/${fileBasenameNoExtension}"
			],
			"options": {
				"cwd": "${fileDirname}"
			},
			"problemMatcher": [
				"$gcc"
			],
			"group": {
				"kind": "build",
				"isDefault": true
			},
			"detail": "调试器生成的任务。"
		}
	],
	"version": "2.0.0"
}
```

3. 在vscode下载插件 - Code Runner 和 CodeLLDB
下载好后，在VSC设置，搜索run in terminal打开，同时select default profile，修改terminal的默认值为zsh（原来是bash）

4. 添加launch.json
**注意**
- type那行一定要修改成lldb
- launch.json的preLaunchTask需要和tasks.json的label相同
```json
{
	// Use IntelliSense to learn about possible attributes.
	// Hover to view descriptions of existing attributes.
	// For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
	"version": "0.2.0",
	"configurations": [
		{
			"name": "C/C++: clang++ build and debug active file",
			// "type": "cppdbg",
			"type": "lldb",
			"request": "launch",
			"program": "${fileDirname}/${fileBasenameNoExtension}",
			"args": [],
			// "stopAtEntry": false,
			"cwd": "${workspaceFolder}",
			// "environment": [],
			// "externalConsole": true,
			// "MIMode": "lldb",
			"preLaunchTask": "C/C++: clang++ build active file"
		}
	]
}
```