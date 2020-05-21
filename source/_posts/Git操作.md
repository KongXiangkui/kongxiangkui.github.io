---
title: Git操作
date: 2020-05-16 12:10:01
tags:
categories:
- 软件工程
toc: true
---
版本控制在软件构造中具有重要作用，本文记录学习Git的相关笔记
<!-- more -->

> 参考：[Git Pro](https://git-scm.com/book/zh/v2)

## 版本控制VCS

一个记录若干个文件内容的变化，以便将来查阅特定版本修订情况的系统。

### 本地版本控制系统

采用数据库来记录文件的历次更新差异

RCS：在硬盘上保存补丁集（补丁指文件修订前后的变化），通过应用所有的补丁，可以重新计算出各个版本的文件内容

### 集中化的版本控制系统Centralized VCS，CVCS

Subversion，Perforce：都有一个单一的集中管理的服务器，保存所有文件的修订版本，协同工作的人们都通过客户端连到这台服务器，去除最新的文件或者提交更新

- 便于管理，，控制项目进度和每个开发者的权限
- 如果中心数据库发生损坏，谁都无法提交更新，无法系如同工作，如果没有备份，将丢失所有数据

### 分布式版本控制系统Distributed VCS，DCVS

Git，Mercurial，Bazaar，Darcs：客户端不只是提取最新版本的文件快照，而是把整个代码仓库都完整的镜像下来，包括完整的历史记录，每一次clone都是对代码仓库的一次完整的备份。

## Git简史

开发目标
- 速度
- 简单的设计
- 非线性开发模式的强力支持（允许成千上万个并行开发的分支）
- 完全分布式
- 有能力搞笑管理类似Linux内核一样的超大规模项目（速度和数据量）

Git与其他版本控制系统主要的差别在于Git对待数据的方法：

- 其他大部分系统以文件变更列表的方式是存储信息，这类系统将他们存储的信息看作是一组基本文件和每个文件随时间逐步累积的差异（基于差异delta-based的版本控制）
- GIt将数据看作是小型文件系统的一些列快照，每当你提交更新或保存项目状态时，它基本上就会对当时的全部文件创建一个快照，并保存这个快照的索引（为了效率，如果文件没有修改，Git不会冲信存储该文件，而是保留一个连接指向之前存储的文件）——Git对待数据更像是一个快照流

Git:

- Git更像是一个小型文件系统，提供许多以此为基础构建的工具
- Git绝大多数操作只需要访问本地文件和资源（浏览项目历史也可以直接从本地数据库中读取），降低网络延时的影响
- Git中所有的数据在存储前都计算校验和，然后以校验和来引用（=> 不可能在Git不知情时更改文件）

> Git使用SHA-a散列（hash，哈希）机制来计算校验和。这是个由40个十六进制字符组成的字符串，是基于Git中文件的内容和目录结构计算出来的

- Git一般只添加数据，一旦提交快照到Git中，就难以丢失数据（可以设定定期推送到仓库）
- Git有三种状态：
  - 已修改modified:修改了文件，但还没保存到数据库中
  - 已暂存staged：对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中
  - 已提交committed：数据已经安全的保存在本地数据库中

- Git：
  - 工作目录Work Directory：对项目的某个版本独立提取出来的内容，从Git仓库的压缩数据库中提取出来的文件，放在磁盘上供你使用或修改
  - 暂存区Staging Area（索引）：是一个文件，保存了下次将要提交的文件列表信息，一般在Git仓库目录中
  - Git仓库目录（.git directory: Repository)：保存项目元数据和对象数据库的地方（从其他计算机克隆仓库时，赋值这里的数据）


Git工作流程
1. 在工作区中修改文件
2. 将你下次提交的更改选择性的暂存（只会将更改的部分添加到暂存区）
3. 提交更新，找到暂存区的文件，将快照永久性的存储到Git目录

> 可以跳过暂存，直接提交

## Git的配置
Git自带一个git config工具来帮助控制Git的外观和行为的配置变量。

### 配置文件

对于Linux/Unix，变量存储在三个位置：
- /etc/gitconfig文件：包含系统上每一用户及他们仓库的通用配置，如果在执行git config时带上--system选项，就会读写该文件中的配置变量（系统配置文件，需要管理员或超级用户权限来修改）
- ~/.gitconfig文件 或 ~/.config/git/config文件：只针对当前用户，可以传递--global选项让Git读写此文件，会对系统上**所有**的仓库生效
- 当前仓库的Git目录中的config文件（即.git/config）：针对该仓库，可以传递--local选项让Git读写此文件，默认也是读写此文件（需要进入某个Git仓库才会生效）

对于Windows系统，Git会查找$HOME目录（一般为C:\\Users\\$USER）下的.gitconfig文件。Git同样也会虚招/etc/gitconfig文件，但只限于MSys的根目录下，即安装Git时所需的目标位置。对于Git2.x以上的版本，还有一个系统级的配置文件（一般在C:\\ProgramData\\Git\\config)，只能以管理员权限通过git config -f \<file>来修改。

```shell
$ git config --list --show-origin   #查看所有配置及他们所在的文件 
```

### 用户信息

安装Git之后，需要设置用户名和邮件地址。每一个Git的提交都会用到这些信息，会记录到每次提交中，而且不能修改。

```shell
$ git config --global user.name "Kong Xiangkui"
$ git config --global user.email xiangkuikong@163.com
```

### 文本编辑器

配置默认文本编辑器，否则使用操作系统默认的文本编辑器

```shell
$ git config --global core.editor vim
# 在 Windows 系统上，如果你想要使用别的文本编辑器，那么必须指定可执行文件的完整路径。
$ git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
```

```shell
# 检查自己的配置(你可能会看到重复的变量名，因为 Git 会从不同的文件中读取同一个配置)
$ git config --list

$ $ git config user.name    # git config <key> 检查Git的某一项配置
```

## Git基础

### 获取Git仓库

- 将尚未进行版本控制的本低目录转换为Git仓库
- 从其他服务器克隆一个已存在的Git仓库

#### 在已存在的目录中初始化仓库

```shell
$ cd ~/my_project       
$ git init      # 将会创建一个名为.git的子目录，包含了初始化Git仓库中所有的必须文件（仅做了初始化操作，项目里的文件还没有被跟踪）
$ git add *.c   # 跟踪.c文件
$ git commit -m 'initial project version'
```

#### 克隆现有仓库

```shell
$ git clone https://github.com/libgit2/libgit2    # 在当前目录下创建一个libgit2目录，并在这个目录下初始化一个.git文件夹，从远程仓库拉去下所有数据放入 .git文件夹，然后从中读取最新版本的文件的拷贝。
$ git clone https://github.com/libgit2/libgit2 myreponame     # 可以自己定义本地仓库名字
```

> Git支持多种数据传输协议，如https:// 协议, git:// 协议 或者 使用SSH传输协议（user@server:path/to/repo.git）

### 记录每次更新到仓库

工作目录下的每一个文件都不外乎两种状态：
- 已跟踪：被纳入本本控制的文件，上一次**快照**中有他们的记录，工作一段时间之后，它们的状态可能是未修改，已修改，或已放入暂存区。
- 未跟踪：既不存在于上次快照的记录中，也没有被放入暂存区

![Figure 文件的状态变化周期](/images/lifecycle.png)

```shell
$ git status    # 查看哪些文件属于什么状态(默认分支名为master)
```

### 跟踪新文件

```shell
$ vim README    # 创建新文件
$ git add README     # 开始跟踪一个文件，此时出与暂存状态
# git add 命令使用文件或目录的路径作为参数，如果是目录的路径，该命令将递归地跟踪该目录下的所有文件。
$ git status
```

### 跟踪已修改的文件

```shell
$ vim hello.c
$ git status    # 此时，已跟踪的文件的内容发生变化，但还没有放到暂存区
$ git add hello.c   # 此时，该文件已经加入到暂存区
```

> git add 命令可以被认为是“精确地将内容添加到下一次提交中”而不是“将文件添加到项目中”












