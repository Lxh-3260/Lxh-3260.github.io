---
layout: post
title: How to config C++23 in Win11 vscode 
date: 2024-11-7 09:48:00 +08:00
description: # Add post description (optional)
img: # Add image post (optional)
tags: [Vscode, C++23] # add tag
---

## mingw64下载及环境变量配置
1. 在[mingw官网](https://www.mingw-w64.org/downloads/#mingw-builds)跳转的[github的realse](https://github.com/niXman/mingw-builds-binaries/releases)下载最新版本的mingw64，我选择的x86_64-14.2.0-release-win32-seh-msvcrt-rt_v12-rev0.7z
2. 下载后直接解压到C盘根目录，一开始解压到Program File里面，不知道是不是有空格的原因，导致debug的时候一直找不到gdb.exe
3. 在搜索栏搜查看高级系统设置，点击环境变量，双击系统变量的PATH，往里面加入bin的路径，C:\mingw64\bin，然后一直确定，直到环境变量添加成功

## vscode下载及插件配置
1. 在vscode下载插件
- C/C++的扩展
- Code Runner
- Chinese
2. 按下F1或者按下Ctrl + Shift + P调出面板，输入C/C++，选择编辑配置UI
- 在include path处加入路径，这里的路径用win + R，输入cmd后，使用`gcc -v -E -x c++ -`指令复制最后几行自己的头文件，这些路径在`c_cpp_properties.json`这一步也用到了
```shell
${workspaceFolder}/**
C:/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/14.2.0/include/c++
C:/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/14.2.0/include/c++/x86_64-w64-mingw32
C:/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/14.2.0/include/c++/backward
C:/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/14.2.0/include
C:/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/14.2.0/include-fixed
C:/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/14.2.0/../../../../x86_64-w64-mingw32/include
```
- 修改c和c++standard为c23和c++23
3. 右键Code Runner选择设置
- 修改Default Language为c++
- 点击在settings.json中编辑，在cpp那行加入`-std=c++2a`，注意这里不能用c++23
新建.vscode文件夹，然后建`launch.json`, `tasks.json`, `c_cpp_properties.json`
4. `launch.json`:
```json
{
    "version": "0.2.0",
    "configurations": [

        {
            "name": "g++.exe - 生成和调试活动文件",
            "type": "cppdbg",
            "request": "launch",
            "program": "${fileDirname}\\${fileBasenameNoExtension}.exe",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": true,
            "MIMode": "gdb",
            "miDebuggerPath": "C:/mingw64/bin/gdb.exe",
            "setupCommands": [
                {
                    "description": "为 gdb 启用整齐打印",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "C/C++: g++.exe 生成活动文件"
        }
    ]
}
```
5. `tasks.json`
将 .vscode 文件夹中的 `tasks.json` 中的 args 中增加 "-std=c++2a", 使得在编译代码的过程中支持 C++20。
```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "type": "cppbuild",
            "label": "C/C++: g++.exe 生成活动文件",
            "command": "C:/mingw64/bin/g++.exe",
            "args": [
                "-fdiagnostics-color=always",
                "-std=c++2a",
                "-g",
                "${file}",
                "-o",
                "${fileDirname}\\${fileBasenameNoExtension}.exe"
            ],
            "options": {
                "cwd": "C:/mingw64/bin"
            },
            "problemMatcher": [
                "$gcc"
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            },
            "detail": "C:/mingw64/bin/g++.exe"
        }
    ]
}
```
6. `c_cpp_properties.json`
这里的路径用win + R，输入cmd后，使用`gcc -v -E -x c++ -`指令复制最后几行头文件，这是为了防止出现头文件可能无法找到的问题
```json
{
    "configurations": [
        {
            "name": "Win32",
            "includePath": [
                "${workspaceFolder}/**",
                "C:/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/14.2.0/include/c++",
                "C:/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/14.2.0/include/c++/x86_64-w64-mingw32",
                "C:/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/14.2.0/include/c++/backward",
                "C:/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/14.2.0/include",
                "C:/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/14.2.0/include-fixed",
                "C:/mingw64/bin/../lib/gcc/x86_64-w64-mingw32/14.2.0/../../../../x86_64-w64-mingw32/include"
            ],
            "defines": [
                "_DEBUG",
                "UNICODE",
                "_UNICODE"
            ],
            "compilerPath": "C:\\mingw64\\bin\\gcc.exe",
            "cStandard": "c23",
            "cppStandard": "c++23",
            "intelliSenseMode": "windows-gcc-x64"
        }
    ],
    "version": 4
}
```