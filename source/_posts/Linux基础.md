---
title: Linux基础
date: 2020-02-28 23:04:41
tags: Linux
---
## Linux Distributes

查看系统信息

```shell
  uname -a          # 显示计算机及操作系统相关信息
  lsb_release -a    # 查看系统版本信息
  cat /proc/version # 查看系统正在运行的内核版本
  cat /etc/issue    # 查看发行版本信息
  neofetch          # 查看计算机配置
```
## Linux桌面环境

- X Window
- KDE（K Desktop Environment）
- GNOME（the GNU Network Object Model Environment）
- Unity


## Linux 启动过程

1. 内核的引导：BIOS自检，操作系统接管硬件，读入/boot目录下的内核文件

2. 运行init（所有进程的起点）：
   - init进程
   > Windows叫服务service；Linux叫守护进程daemon

   - 运行级别runlevel：不同场合需要不同的开机启动程序，根据运行级别确定需要运行哪些程序
     - 运行级别0：系统停机状态，无法正常启动
     - 运行级别1：单用户工作状态，root权限，用于系统维护，禁止远程登陆
     - 运行级别2：多用户状态（没有NFS）
     - 运行级别3：完全的多用户状态（有NFS），登陆后进入控制台命令模式
     - 运行级别4：系统未使用，保留
     - 运行级别5：X11控制台，登陆后进入GUI模式
     - 运行级别6：系统正常关闭并重启，默认运行级别不能为6，否则无法正常启动

3. 系统初始化：激活交换分区，检查磁盘，加载硬件模块以及其他需要优先执行的任务

  > 保证当init改变运行级别时，所有相关守护进程都将重启

4. 建立终端,以便用户登录

5. 用户登录系统

   - 命令行登录
   - ssh登录
   - 图形界面登录

  > 默认tty1，Ctrl+Alt+F1~F6切换tty1~tty6，Ctrl+Alt+F7返回图形界面

## Linux 关机过程

1. sync：将数据由内存同步到硬盘中
2. shutdown：关机指令
3. reboot：重启（等同于shutdown -r now）
4. halt：关闭系统（等同于shutdown -h now 和 poweroff）


