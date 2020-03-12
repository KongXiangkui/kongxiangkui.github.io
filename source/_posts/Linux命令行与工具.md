---
title: Linux命令行与工具
date: 2020-02-28 23:08:16
tags: Linux
---
## 帮助文档

```shell
#--- 命令 ---
$man
#查手册
$man cp
#查询cp命令

$info

$history
#跟踪使用的命令（保存在.bash_history)

$alias -p     #命令别名
#-p 查看当前可用别名
$alias li='ls -li'
```

## 文件目录

> - 单点符 .  表示当前目录
> - 双点符 .. 表示当前目录的父目录

```shell
$cd destination
#改变目标目录change directories

$pwd
#当前路径

$ls
#-F  目录后加/，可执行文件加*
#-R  递归选项

$mkdir New_Dir
#创建目录

$mkdir -p New_Dir/Sub_Dir/Under_Dir
#-p  创建多个目录和子目录

$rmdir New_Dir
#删除空目录

$rmdir -ri My_Dir
#-r  向下进入目录，删除其中的文件，再删除目录
```
> 通配符——正则表达式
> - ？  单个字符
> - \*     任意个字符

## 文件操作

```shell
$touch file_name
#创建空文件

$cp source destination
#复制文件

$mv source_file new_file
#重命名文件，移动
$rm -i file
#移除removing
#-i  出现提示
#-f  强制删除
```

```shell
### 文本文件查看

$file my_file
#探测文件类型

$cat my_file
#显示文件
#-n  行号
#-b  只给有文本的行加上行号
#-T  不出现制表符

$more
#分页工具 空格或回车逐行向前

$less
#less is more
#更高级的more，可识别上下键翻页

$tail
#显示文件尾部
#tail -n 2 file
##-n 参数，显示所显示的行号

$head
#显示文件开头，支持-n参数
```
## 文本文件处理

### 正则表达式

一种可定义的模式模板 pattern template ，linux工具（sed，gawk，grep等）使用正则表达式模式匹配数据通过正则表达式引擎regular expression engine （一套底层软件，负责解释正则表达式模式并使用模式进行文本匹配）实现。

```flow
st=>start: 数据流
cond=>condition:  正则表达式
match=>operation: 匹配的数据
strain=>operation: 滤掉的数据 
st->cond
cond(yes)->match
cond(no)->strain
```

Linux 正则表达式引擎类型

- POSIX 基础正则表达式（basic regular expression ， BRE）引擎
- POSIX 扩展正则表达式（extended regular expression ， ERE）引擎

注：sed 符合POSIX BRE 引擎规范的子集；gawk 符合POSIX ERE 引擎规范

### sed

   > 流编辑器 stream editor ，在编辑处理数据之前，基于预先提供的一组规则来编辑数据流
   >  sed 编辑器根据命令（从命令行输入 或者 存储在命令文本文件）处理数据流中的数据

      1. 一次从输入中读取一行数据
      2. 根据所提供的编辑器命令匹配数据
      3. 按照命令修改数据流中的数据
      4. 将新的数据输入到STDOUT


### gawk

```shell
$sort file
#按照默认语言的排序规则对文本文件的数据进行排序
#-n  识别数字，按数字排序
#-M  识别三个字符的月份名

$grep [options] pattern [file]
#在输入或指定的文件中查找包含匹配指定模式的字符的行

$grep th file
the
three
#-v  进行反向搜素（输出不匹配该模式的行）
#-n  显示行号
#-c  只显示包含匹配的模式的行
#grep -e t -e f file  #指定每个模式
#使用正则表达式
```

## 链接文件

- 符号链接：两个不同的文件通过符号链接在一起
- 硬链接：创建独立的虚拟文件，包含原始文件信息及位置，根本而言是同一个文件

```shell
#创建链接，源文件必须存在
$ln -s data_file s1_data_file
$ls -i *data_file
#查询incode编号
$ln code_file hl_code_file
#引用硬链接等同于引用源文件，两个文件共享一个incode编号
```

>  incode编号: 唯一标识文件和目录的数字

> 只能对同一存储媒体文件创建硬链接，不同存储媒体文件文件之间创建链接，只能使用符号链接

## 压缩工具

- bzip2(.bz2): 采用Burrows-Wheeler块排序文本压缩算法和霍夫曼编码
- compress(Z):最初的Unix压缩工具（淘汰中）
- gzip(.gz): GNU压缩工具，用Lemep-Ziv编码
- zip(.zip): Windows上PKZIP工具的Unix实现

## 归档工具

```shell
$tar function [options] object1 object2 ...
#功能
##-A  追加到另一个已有的tar文件
##-c  创建一个新的tar文件
##-t  列出已有tar归档文件的内容
#选项
##-v  处理文件时显示文件
##-p  保留所有文件的权限
##-z  将输出重定向到gzip命令来压缩内容
```

## Linux命令选项常用含义

| 选项  |            描述            |
| :---: | :------------------------: |
|  -a   |      显示所有描述对象      |
|  -c   |        生产一个计数        |
|  -d   |        指定一个目录        |
|  -e   |        扩展一个对象        |
|  -f   |     指定读入数据的文件     |
|  -h   |     显示命令的帮助信息     |
|  -i   |        忽略文本大小        |
|  -l   |     产生输出的长格式版     |
|  -n   |  使用非交互模式（批处理）  |
|  -o   | 将所有输出重定向到指定文件 |
|  -q   |       以安静模式运行       |
|  -r   |    递归的处理目录和文件    |
|  -s   |       以安静模式运行       |
|  -v   |        生成详细输出        |
|  -x   |        排除某个对象        |
|  -y   |       对所有回答yes        |


## 包管理系统package management system，PMS

PMS利用数据库记录相关内容：

- 系统以安装的软件包
- 每个包已安装的文件
- 每个已安装软件的版本

> 软件包存储在服务器（仓库repository）上，利用Linux本地的PMS工具通过互联网访问，搜索新的软件包或者更新已有软件包

PMS基础工具

1. 基于Debain发行版

- dpkg（软件包管理系统工具）
- apt-get
- apt-cache
- aptitude（dpkg和apt的前端，完整的软件包管理系统）

```shell
$aptitude
#进入aptitude的全屏模式 q 退出

$aptitude show package_name
#显示某个包的详细信息
$dpkg -L package_name
#列出package_name软件包所安装的全部文件
$dpkg --search absolute_file_name
#查找某个文件（绝对文件路径）属于特定的包
$aptitude search package_name
#找到特定的软件包，无需再package_name周围加通配符
#每个包名字之前  p或v 可用未安装；i 已安装;
#i u 自动解析必要的包依赖关系，并安装额外的库和软件包
#c  软件删除但数据和配置文件还在
#p  软件删除，配置文件也删除

$aptitude install package_name
#apt-get install package_name
#从软件仓库安装软件包

$aptitude safe-upgrade
#不保守的软件升级 full-upgrade  或者  dist-upgrade

$aptitude purge package_name
#卸载软件并删除相关的数据和配置文件
$aptitude remove package_name
#删除软件包而不删除数据和配置文件
```

aptitude 仓库（仓库位置文件设置文件/etc/apt/source.list，若要修改，最安全的是直接复制软件仓库网站的文本)

```shell
deb (or deb-src) address distribution_name package_type_list
#deb (or deb-src)的值表明了软件包的类型
##deb值说明是一个以编译程序源
##deb-src值说明是一个源代码的源
#address条目是软件仓库的Web地址
#distribution_name条目是特定软件仓库的发行版版本名称
#package_type_list（不止一个词）表明仓库里有什么类型的包
```

2. 基于RedHat发行版

- yum（RedHat，Fedora）
- urpm（Mandriva）
- zypper（openSUSE）

3. 基于Arch Linux发行版

- pacman
   
4. Synaptic Package Manager新立得软件包管理器

5. 源代码安装

以 tarball 包形式的软件安装
```shell
$tar -zxvf package_name.tar.gz		#解包
#进入目录，读README或AAAREADME文件按照文件位系统配置该软件
$./configure
#（检查Linux系统，确保拥有合适的编译器编译源代码，并具备正确的库依赖关系
$make		#构建二进制文件，编译和链接所有的源代码文件
$sudo make install		#root用户身份登陆或sudo命令，安装在Linux系统常用位置
```

## 文本编辑器
- nano：简易控制台模式编辑器，无图形化编辑功能。Unix系统的Pico编辑器的克隆版，在窗口底部显示命令及其简要描述。
  
- vim（vi improved）：交互式文本编辑器，用命令来交互的插入，删除，替换数据中的文本
  
- Emacs：提供命令行模式编辑器，也可使用图形化环境
  
- KDE系编辑器（KDE桌面中使用）
  - KWrite 单屏幕文本编辑器程序
  - Kate 功能全面的多窗口文本编辑器程序
  
- GNOME系编辑器
  - gedit 

## 网络下载工具

- wget       将web页面下载到本地

- curl          从特定的web服务器接收数据，也可向web服务器发送数据

  找出手机运营商的SMS网关，通过电子邮件发送短信

  mail -s "your message" your_phone_number@your_sms_gateway


## Lynx，基于文本的浏览器：可直接从终端访问网站（图片被替换成HTML文本标签）

## Mailx（包mailutils）：从shell脚本发送邮件的主要工具，可交互地读取和发送消息，还可用命令行指定发送方式

```shell
$ mail [-eIinv] [-a header] [-b addr] [-c addr] [-s subj] to-addr
```


