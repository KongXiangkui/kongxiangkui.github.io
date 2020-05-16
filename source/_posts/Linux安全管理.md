---
title: Linux安全管理
date: 2020-04-12 19:02:58
tags:
- Linux
categories:
- Linux
toc: true
---
本文介绍 Linux User Identification 的相关内容。通过使用User ID 和 groupID 来提高Linux/UNIX的安全性。其核心就是**用户账户** ，即通过创建用户ID （user ID，UID 唯一）来跟踪，以控制单个用户的安全性。
<!-- more -->
## 基本概念

### User ID 和 Group ID

**User ID**, **Group ID** 由系统管理员进行分配，后者用于用户按照项目或部门等进行分组，以便于一个组内部之间共享资源。

> root(或者superuser) 用户账户是Linux系统的管理员，其 User ID 为0。如果一个用户由superuser privileges可以绕过大多数文件许可的检查，superuser对整个系统具有完全的控制。
> 系统账户：系统上运行各种服务进程访问资源的特殊账户。

/etc/passwd文件：将用户名匹配到UID值（包含 登录名、用户密码、UID、GID、用户账户描述即备注字段、用户HOME目录的位置、用户默认shell）

> /etc/passwd文件为标准文本文件，可直接修改，但可能导致损坏，系统无法读取其内容，进而不能登陆（包括root账户）；需要用标准的Linux用户管理工具执行用户管理功能。用户密码保存在单独文件/etc/shadow文件，仅限root用户访问，记录登录名，加密的密码，关于密码修改的一系列问题

group组——共享资源的一组用户。组权限允许多个用户对系统中的对象（文件，目录，设备等）。

> 不同发行版处理默认组的成员的方式不同。为每个用户单独创建一个组，更安全

/etc/group文件包含系统上每个组的信息

- 组名
- 组密码
- GID
- 属于该组的用户列表

> 当一个用户在/etc/passwd文件中指定某个组作为默认组，用户账户不会作为该组成员在出现在/etc/group文件中

GID分配：系统用户GID < 500; 用户组GID >= 500

> /etc/group文件存储着group names和group IDs的映射关系

### 文件权限及文件共享

文件权限符

第一个字段：

- -代表文件
- d代表目录
- l代表链接
- c代表字符型设备
- b代表块设备
- n代表网络设备

之后3组三字符的编码，每一组定义3种访问权限：

​	-r 	-w 	-x

如果没有某种权限，以破折线占位

3组权限分别对应三个安全级别：

- 文件属主的权限
- 属组成员的权限
- 其他用户的权限

```shell
# 文件权限展示实例
-rwxrwxr-x 1 dell dell 4096 8月   8 21：00 桌面
```

默认文件权限

umask命令 设置所创建的文件和目录的默认权限，第一位，粘着位sticky bit，后三位表示文件或目录对应的umask八进制值

> 八进制模式的安全性设置，先获取这3个rwx权限的值，然后将其转换成3位二进制值，用八进制表示。在二进制表示中，每个位置代表一个二进制位

| 权限  | 二进制 | 八进制 |
| :---: | :----: | :----: |
|  ---  |  000   |   0    |
|  --x  |  001   |   1    |
|  -w-  |  010   |   2    |
|  -wx  |  011   |   3    |
|  r--  |  100   |   4    |
|  r-x  |  101   |   5    |
|  rw-  |  110   |   6    |
|  rwx  |  111   |   7    |

umask值是**掩码**，屏蔽掉不想授予该安全级别的权限，要把umask值从对象的全权限值（对文件是666所有用户都有读和写的权限，对目录是777，具有读写执行的权限）减掉，即为对象权限。umask值设置在/etc/profile启动文件中，ubuntu在/etc/login.defs文件中。可用umask命令为默认umask设置指定新值

Linux位每个文件和目录存储3个额外的信息位

- 设置用户ID（SUID）：档文件被用户使用时，以文件属主权限运行
- 设置组ID（SGID）：对文件，以文件属组权限运行；对目录，目录中创建的新文件以目录的默认属组作为默认属组
- 粘着位：进程结束后，文件还驻留（或粘着）在内存中

> 启用SGID,可强制在共享目录下创建的新文件属于该目录的属组，这个组也是每个用户的属组

> SGID 可以通过chmod加到标准3位八进制值之前，或在符号模式下使用 s

## User ID 操作

### 添加新用户

useradd工具，可一次性创建新用户账户及设置用户HOME目录结构（使用系统默认值即命令行参数设置用户账户  /etc/default/useradd）

```shell
#/usr/sbin/useradd -D
#-D 查看系统默认值
GROUP=100				#新用户被添加到GID为100的公共组
HOME=/home				#HOME目录位于/home/$login_name
INACTIVE=-1 			#新用户账户密码在过期后不会被禁用
EXPIRE=					#新用户未被设置过期日期
SHELL=/bin/bash 		
SKEL=/etc/skel			
#/etc/skel目录下是bash shell环境的标准启动文件
CREATE_MAIL_SPOOL=yes
#为该用户账户在mail目录下创建一个用于接收邮件的文件夹
```
>  有的Linux版本把Linux用户和组工具放在/usr/sbin目录下，可能不在PATH环境变量里

useradd命令行参数

| 参数               | 描述                                     |
| ------------------ | ---------------------------------------- |
| -c   comment       | 给用户添加备注                           |
| -d   home_dir      | 为主目录指定一个名字（不用登陆名）       |
| -e   expire_date   | 用YYYY-MM-DD的格式指定一个账户的过期日期 |
| -f   inactive_days | 指定账户密码过期多少天禁用；             |
|                    | 0一过期就禁用，1禁用此功能               |
| -g   initial_group | 指定用户登录组的GID或组名                |
| -G   group         | 指定用户登录组之外所属的一个或多个附加组 |
| -k（与-m一起）     | 将/etc/skel目录下的内容复制到HOME目录下  |
| -m                 | 创建HOME目录                             |
| -M                 | 不创建HOME目录（当默认创建时）           |
| -n                 | 创建一个与用户登录名同名的组             |
| -r                 | 创建系统账户                             |
| -p   passwd        | 为用户账户指定默认密码                   |
| -s   shell         | 指定默认的登陆shell                      |
| -u   uid           | 为账户指定唯一UID                        |

> 如果总需要修改某个值，最好修改系统的默认值

useradd更改系统默认的新用户设置

-D选项后跟上下列选项

| 参数                 | 描述                                 |
| -------------------- | ------------------------------------ |
| -b   default_home    | 更改默认HOME目录位置                 |
| -e   expiration_date | 更改默认新账户过期日期               |
| -f   inactive        | 更改默认新用户从密码过期到禁用的天数 |
| -g   group           | 更改默认组名或GID                    |
| -s   shell           | 更改默认的登陆shell                  |

```shell
#useradd -D -s /bin/tsch
```

### 删除用户

```shell
$/usr/sbin/userdel -r user_name
#user_name为用户名，不一定是登录名
#有-r参数，删除用户HOME目录以及邮件目录
#不加-r参数，仅删除/etc/passwd的用户信息
```

> 使用-r参数前，要检查该用户目录下是否保存了其他用户的文件

### 修改用户

1. useradd: 修改/etc/passwd文件的大部分字段，参数与useradd一致，还有一下参数

- -l 修改用户账户的登录名
- -L 锁定账户，使用户无法登陆
- -p 修改账户密码
- -U 解锁账户，使账户能够登录

2. passwd 和 chpassed: 修改密码

```shell
$passwd user_name
#改自己的密码，root用户修改其他密码
#-e 强制用户下次登陆时修改密码

$chpasswd < users.txt
#为系统的大量用户修改密码
#从标准输入自动读取登录名和密码，用:分割
#也可使用重定向命令
```

3. chsh、chfn 和 chage: 修改特定的账户信息

```shell
$chsh -s /bin/csh user_name
#修改user_name的默认的用户登陆shell
#使用时，必须用shell的路径全名做参数

$chfn user_name
#chfn 会将用于Unix finger的信息存储进备注字段，而不是存入随机字段或者将备注字段留空
#不使用参数，会询问将那些合适的字段加进备注
#finger查看Linux的用户信息（出于安全，Linux默认不安装，管理员会禁用finger命令）
$finger user_name
$chage
#管理用户账户的有效期（日期格式YYYY-MM-DD）
#参数
### -d	设置上次密码修改到现在的天数
### -E  设置密码过期日期
### -I  设置密码过期到锁定的天数
### -m  设置密码修改之间的天数
### -W  设置密码过期前多久开始出现提醒信息
#过期的账户存在但无法登陆
```

## group ID 操作

```shell
$/usr/sbin/groupadd shared
#创建新组；默认没有用户添加到组中的选项

$/usr/sbin/usermod -G shared user_name1
$/usr/sbin/usermod -G shared user_name2
#-G参数将该组添加到用户的属组列表，不影响默认组
#-g参数，指定组名会替换掉默认组
```

> 如果更改了已登录系统账户所属的用户组，该用户必须退出系统重新登登陆，组关系才能生效

```shell
$/usr/sbin/groupmod -n new_group_name old_group_name
#可以修改已有组的GID（加-g)或组名（加-n）
```

```shell
#--- 权限 ---
$chmod options mode file
#mode参数可以使用八进制模式或符号模式进行安全性设置
$chmod 760 newfile
#八进制模式:直接按期望赋予文件3位八进制权限码
$[ugoa...][[+-=]][rwxXstugo...]
#u代表用户，g代表组，o代表其他，a代表上述所有
#在现有权限上+或-权限；也可等于
#第三个符号代表作用到设置上的权限；
#额外的设置：
#	X如果对象是目录，赋予执行权限
#	s运行时重新设置UID或GID
#	t保留文件或目录
#	u将权限设置与属主一致
#	g将权限设置与属组一致
#	o将权限设置与其他用户一致
```

```shell
#--- 所属关系 ---
$chown options owner[.group] file
#改变文件的属主;可用登录名或UID指定文件的新属主;可同时改变属主和属组
#options：-R 配合通配符可以递归的改变子目录和文件的所属关系
#		  -h 改变文件的所有符号链接文件的所属关系
$chgrp
#改变文件或目录的默认属组
```

> 只有root才能改变文件的属主。任何属主都可改变文件的属组，前提是属主必须是原属组和目标属组的成员

