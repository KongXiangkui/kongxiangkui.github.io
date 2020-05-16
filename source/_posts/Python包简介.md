---
title: Python包简介
date: 2020-03-29 11:04:11
tags: 
- Python
categories:
- [Python, 第三方库]
toc: true
---
本文对自己所使用过的一些包进行介绍
<!--more-->
## wheel

Python 打包模块

> Python打包格式Wheel和Egg,不需要编译或制作的安装过程，实际上也是一种压缩文件，将.whl的后缀改为.zip即可可看到压缩包里面的内容。
> Egg格式是由setuptools在2004年引入，而Wheel格式是由PEP427在2012年定义。 Wheel现在被认为是Python的二进制包的标准格式。

## pyinstaller

Python打包工具

> Requires: pefile, pywin32-ctypes, setuptools, altgraph

## pefile

PE, portable executable文件,可移植的可执行文件，如: EXE, DLL, OCX, SYS, COM文件，PE文件时Microsoft Windows系统的程序文件

> Requires: future
> Required-by: Pyinstaller

```Python
import pefile
pe = pefile.PE('E:\\TestTool\\LaoMaoTao.exe')
print(pe)
```

## torch

pytorch

> Requires: numpy
> Required-by: torchvision

## [requests](.\Python库-requests.md)

基于 urllib，采用 Apache2 Licensed 开源协议的 HTTP 库。它比 urllib 更加方便，可以节约我们大量的工作，完全满足 HTTP 测试需求。

> Requires: certifi, idna, chardet, urllib3

## pyserial

Python串口通信模块

## crcmod

CRC校验



