---
title: Python发布
date: 2020-03-23 09:48:06
tags: 
- Python
categories: 
- [Python, 开发]
toc: true
---
a stand-alone version of your app: a binary executable that other people can run on their systems without having to install anything.
<!--more-->

- py2exe(Windows下使用)
- PyInstaller(与py2exe类似，能够在*nix系统使用，并能够生成自安装的二进制文件)
- freezing：

> 通过虚拟机PVM运行*字节码*。冻结的二进制文件和最初的源代码程序运行速度完全相同，但对接收者来说，代码都隐藏起来

fbs，fman build system [https://build-system.fman.io/manual/]

- a Python-based build tool for desktop applications that use PyQt or Qt for Python. 

```python
   # start a new fbs project
   fbs startproject     # creates a new folder called src/ in your current directory. 
                        # This folder contains the minimum configuration for a bare-bones PyQt app.

   # run the app
   fbs run

   # freezing the app
   fbs freeze        # turn the app's source code into a standalone executable

   # create an installer
   fbs installer     # If you are on windows, you first need to install NSIS and place it on your PATH

```

## 基本命令介绍

- **-h, -help**: 帮助
- **-F, -onefile**: 产生单个的可执行文件
- **-D, --onedir**: 产生一个目录（包含多个文件）作为可执行程序
- **-w, --windowed，--noconsolc**: 指定程序运行时不显示命令行窗口（仅对 Windows 有效）
- **-F, -c，--nowindowed，--console**: 指定使用命令行窗口运行程序（仅对 Windows 有效）
- **-icon=目标路径**： 指定icon

```shell
# Executable will be generated on a floder called dist on yout Python directory.
$ pyinstaller test.py

# generate one executable file using one file mode
$ pyinstaller --onefile test.py

$ pyinstaller -w test.py

$ pyinstaller --help

$ Pyinstaller -F -w main.py
```

## 问题 

### icon

ico文件只有一种尺寸，你应该准备四张不同尺寸的png文件，然后用png2icon(下载)脚本把它们合成一张icon图标文件

原理的话，就是在不同情况下（比如资源管理器文件列表前面的图标、桌面、开始菜单等）需要不一样尺寸的图标。如果尺寸不合适的话，可能出现有的地方显示正确有的显示不正确的情况。最后几个地方都要检查一遍。

使用png2ico脚本生成包含以下6个尺寸的ico文件：128×128 64×64 48×48 32×32 16×16
然后在pyinstaller里使用--icon参数可以成功，在缩略图和列表等多种显示模式下显示正常。
注意png2ico转换的分辨率顺序需要从大到小
