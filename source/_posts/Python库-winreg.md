---
title: Python winreg
date: 2020-03-23 20:51:05
tags: 
- Python
categories:
- [Python, 语言]
toc: true
---
本文介绍Python使用内置库winreg对Windows注册表的操作
<!--more-->
## winreg, windows registry access

expose the Windows registry API to Python(Windows注册表的API)

## Functions

### winreg.CloseKey(hkey)
功能: 关闭先前打开的registry key

### winreg.ConnectRegistry(computer_name, key)
功能: 建立于另一台计算机上与定义注册表句柄的连接，并返回处理object
参数: 
- computer_name: 远程计算机的name，格式为 r"\\computer_name"。如果是None, 则使用本低计算机
- key: 要连接的预定义句柄
返回值为代开的key句柄

如果失败，则引发OSERROR exception

### winreg.CreateKey(key, subkey)

功能：创建或打开指定的key，返回处理object
参数： 
- key为已经打开的key或者是预定义的HKEY_*常量之一
- subkey是一个string

### winreg.CreateKeyEx(key, subkey, reserved=0, access=KEY_WRITE)

### winreg.DeleteKey(key, subkey)
功能：删除指定key
参数：
- key为已打开的key，或者预定义的HKEY_*常量之一
- subkey是一个string，不许是可参数表示的key的子健

### winreg.DeleteKeyEx(key, subkey, access=KEY_WOW64_64KEY, reserved=0)

### winreg.DeleteValue(key, value)
功能：从注册表key中删除命名的value

### winreg.EnumKey(key, index)
功能：枚举打开注册表key的子键，或者是预定义的HKEY_*常量之一
参数：
- index：一个integer，用于表示要检索的key的索引

### winreg.WnumValue(key, index)
功能：美剧一个打开的注册表key的值，返回一个元组

### ……

## 常量Constant

### HKEY_*常数

