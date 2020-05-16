---
title: Python库-virtualenv
date: 2020-03-23 23:25:02
tags: 
- Python
- 虚拟环境
- virtualenv
categories:
- [Python, 第三方库]
toc: true
---
The best way to manage dependencies in Python is via a virtual environment. A virtual environment is simply a local directory that contains the libraries for a specific project. This is unlike a system-wide installation of those libraries, which would affect all of your other projects as well.
<!--more-->

> 以前一直不知道虚拟环境的作用，当我在Linux系统（Ubuntu）上安装Python相关的库的的时候，发现系统自带的Python有2.x和3.x版本，所以库的安装存在问题。虚拟环境正是用于解决这个问题。
> 此外，随着Python的使用，包安装的越来越多，很多时候，都不知道包与包之间的依赖关系。如果使用虚拟环境，直在该开发环境下安装必备的包

Python应用程序通常会使用不在标准库内的软件包和模块。应用程序有时需要特定版本的库，因为应用程序可能需要修复特定的错误，或者可以使用库的过时版本的接口编写应用程序

### 虚拟环境的包virtualenv

```shell
pip install virtualenv

```
### 虚拟环境的使用

创建一个virtual environment(一个目录树，其中安装有特定的Python版本，以及许多其他包) ,不同的应用使用不同的虚拟环境
   
```shell
# 使用模块venv创建虚拟环境：确定虚拟环境的目录，在其中包含Python解释器，标准可和各种支持文件的副本目录
cd my_project_dir
python3 -m venv tutorial-env

# 切换到虚拟环境的路径，激活虚拟环境
# Windows
call tutorial-env\Scripts\activate.bat
# Unix、MacOS
source tutorial-env/bin/activate

# 关闭虚拟环境
deactivate
```