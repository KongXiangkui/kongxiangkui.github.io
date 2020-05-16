---
title: Python文件
date: 2020-03-21 21:39:00
tags: 
- Python
categories:
- [Python, 语言]
toc: true
---
本文介绍Python对文件的操作以及目录的使用
<!--more-->
## 基本概念

文件：
- filename
- directory
- path 

> directory

root folder:
- windows: C:/ 
- Unix/Linux: / 

当有新的盘如，USB，DVD等
- Windows： D:/, E:/
- Unix/Linux: /mnt folder (mount,挂载）
- OS X: /Volumes folder 

分隔符
- Windows 使用backslash \ 分隔
- Unix/Linux/OS X 使用forward slash / 分隔（网络也是使用 / 分隔）

```python
import os
os.pash.join('usr','bin', 'spm')
# windows: 'usr\\bin\\spm'
# Unix/Linux/OS X: 'usr//bin//spm'
```

```python
import os
os.getcwd()     # 获得当前工作目录current working directory
# 'C:\\Python34' 
os.chdir('C:\\Windows\\System32') # 改变当前工作目录
os.getcwd() 
# 'C:\\Windows\\System32'
os.mkdir('C:\\Windows\\NewFold')
```

## 文件目录

### 绝对目录 VS 相对目录
. 当前文件夹
.. 父文件夹

- **os.path.abspath(path)**: return a string of the absolute path of the argument. This is an easy way to convert a relative path into an absolute one.
- **os.path.isabs(path)**: return True if the argument is an absolute path and False if it is a relative path.
- **os.path.relpath(path, start)**: return a string of a relative path from the start path to path. If start is not provided, the current working directory is used as the start path.
- **os.path.dirname(path)**: return a string of everything that comes before the last slash in the path argument. 
![dir name and base name of a path](source/imgs/dir-base-name.png)


```python
import os
os.path.abspath('.') 
# 'C:\\Python34'
os.path.isabs('.') 
# False
os.path.isabs(os.path.abspath('.')) 
# True
os.path.relpath('C:\\Windows', 'C:\\spam\\eggs')
# '..\\..\\Windows' 

path = 'C:\\Windows\\System32\\calc.exe'
os.path.basename(path) 
# 'calc.exe'
os.path.dirname(path)  
# 'C:\\Windows\\System32'

os.path.split(path)     # 返回一个元组tuple
# ('C:\\Windows\\System32', 'calc.exe')

path.split(os.path.sep)     # sep用于设置正确的分隔线
# ['C:', 'Windows', 'System32']
'/usr/bin/'.split(os.path.sep)
# ['', 'usr', 'bin']
```

### 获得文件大小和文件夹内容
- **os.path.getsize(path)**: return the size in bytes of the file in the path argument. 
- **os.listdir(path)**: return a list of filename strings for each file in the path argument. (Note that this function is in the os module, not os.path.)

```python
import os
os.path.getsize('C:\\Windows\\System32\\calc.exe')
# 4096
os.listdir('C:\\Windows\\System32') 
# ['checkserialpport.py', 'ex.py', 'ref', 'SerialPortAssistant.py', 'serialportGUI.py', 'serialportGUI.ui', 'stc-isp-15xx-v6.87B', 'test', 'text2.py', '__pycache__']
```

### 检查path的合法性
- **os.path.exists(path)**: return True if the file or folder referred to in the argument exists and will return False if it does not exist. 
- **os.path.isfile(path)**: return True if the path argument exists and is a file and will return False otherwise. 
- **os.path.isdir(path)**: return True if the path argument exists and is a folder and will return False otherwise.

```python
os.path.exits('C:\\Windows')
# True
os.path.isdir('C:\\Windows\\System32')
# True
os.path.isfile('C:\\Windows\\System32\\calc.exe')
# True
```

## 文件读写
- Plaintext file
- Binary file

File object： represent a file on the computer

文件操作
1. Call the **open()** function to return a File object. 
2. Call the **read()** or **write()** method on the File object.
3. Close the file by calling the **close()** method on the File object.

### 读文件

```Python
# in reading plaintext mode
helloFile = open('C:\\User\\your_home_folder\\hello.txt')   # Windows,只读
helloFile = open('C:\\User\\your_home_folder\\hello.txt', 'r')   # Windows,只读

helloFilel = open('/User/your_home_folder/hello.txt')        # OS X

# Now you have a File object
helloContent = helloFile.read()     # 将文件的整个内容赋值给一个字符串值
helloContentLine = helloFile.readlines() # 将文件的各行赋值给一个字符串列表

helloFile.close()
```

### 写文件

```Python
# 如果没有，则创建一个新文件
helloFile = open('C:\\User\\your_home_folder\\hello.txt', 'w')   # w模式，会覆盖原来的内容
helloFile = open('C:\\User\\your_home_folder\\hello.txt', 'a')   # a模式，append text to the end of the existing file

helloFile.write('Hello, World\n')

helloFile.close() 
```
## 变量保存

### 保存变量——使用shelf模块

> 保存为binary shelf files
> 保存一些configuration settings，便于下次加载

shelf value:
- keys()
- values()


```Python
import shelve
shelfFile = shelve.open('mydata')
cats = ['Zophie', 'Pooka', 'Simon']
shelfFiel['cats'] = cats
shelfFile.close()
# Windows: current working directory: mydata.bak, mydata.dat, mydata.dir
# OS X: only s aingle mydata.db file 

shelfFile = shelve.open('mydata')
type(shelfFile)
# <class 'shelve.DbfilenameShelf'>
shelf.File['cat']
# ['Zophie', 'Pooka', 'Simon']
list(shelfFile.keys())
# ['cats']
list(shelfFile.values())
# [['Zophie', 'Pooka', 'Simon']]
shelf.close()
```

### 保存变量——使用ppriint.pformat() 函数

> 可以把这些内容保存到py文件中，之后直接import
```Python
# 保存
import pprint
cats = [{'name': 'Zophie', 'desc': 'chubby'}, {'name': 'Pooka', 'desc': 'fluffy'}]
pprint.pformat(cats)
# "[{'name': 'Zophie', 'desc': 'chubby'}, {'name': 'Pooka', 'desc': 'fluffy'}]"
fileObj = open('myCats.py', 'w')
fileObj.write('cats = ' + pprint.pformat(cats) + '\n')
# 83
fileObj.close()
```
```Python 
# 使用
import myCats
myCats.cats
# "[{'name': 'Zophie', 'desc': 'chubby'}, {'name': 'Pooka', 'desc': 'fluffy'}]"
```

## 文件组织

shutil (or shell utilities) module

### 复制

shutil.copy(source, destination)
功能：复制文件从source到destination
参数：source，destination都是string类型

shutil.copytree()
功能：复制整个文件夹（包括内部的文件）

```Python 
import shutil, os
os.chdir('C:\\')
shutil.copy('C:\\spam.txt', 'C:\\delicious')
# 'C:\\delicious\\spam.txt'
shutil.copy('C:\\spam.txt', 'C:\\delicious\\eggs.txt')
# 'C:\\delicious\\eggs.txt'

os.chdir('D:\\')
shutil.copytree('D:\\bacon', 'D:\\bacon_backup')
# 'D:\\bacon_backup'
```

### 移动和重命名

shutil.move(source, destination)
返回：新位置的绝对路径的字符串

```Python 
import shutil
shutil.move('C:\\bacon.txt', 'C:\\eggs')    # 与此同时，可以重命名
# 'C:\\eggs\\bacon.txt'
shutil.move('C:\\sea.txt', 'C:\\new')  # 如果没有new文件夹，则重命名
```

### （永久）删除

- **os.unlink(path)**: 删除path中的文件
- **os.rmdir(path)**： 删除path中的文件夹（文件夹必须是空的）
- **shutil.retree(path)**: 删除path中的文件夹和其中的文件

### 案例

1. 删除所有txt文件

```Python
import os
for filename in os.listdir():
    if filename.endswith('.txt'):
        os.unlink(filename)
```

## 安全删除

send3trash module （第三方库）

```Python
import send2trash
baconFile = open('bacon.txt', 'a')  # create the file
baconFile.write('Bacon is not a vegetable.')
# 25
baconFIle.close()
send2trash.send2trash('bacon.txt')
```

### 目录树Directory Tree

os.walk
返回值：
- A string of the current folder's name
- A list of strings of the folders in the current folder
- A list of strings of the files in the current folder

```Python
import os

for foldername, subfolders, filenames in os.walk('C:\\delicious'):
    print('The current folder is ' + foldername)
    for subfolder in subfolders:
        print('SUBFOLDER OF ' + foldername + ': ' + subfolder)
    for filename in filenames: 
        print('FILE INSIDE ' + folderName + ': ' + filename)
    print('')
```

## ZIP文件

zipfile module

ZipFile object：代表了完整的打包对象
methods:
- namelist(): 返回一个字符串列表，包含ZIP文件中的文件和文件夹
- getinfo(): 返回一个ZipInfo object, 关于这个具体的文件
- extractall() 解压ZIP文件

### 获取ZIP文件信息

```Python
import zipfile, os
os.chdir('C:\\')
exampleZip = zipfile.ZipFile('example.zip')
exampleZip.namelist()
# ['spam.txt', 'cats/' , 'cats/catname.txt', 'cats/zophie.jpg']
spamInfo = exampleZip.getinfo('sapm.txt')
sampInfo.file_size
# 13908
spamInfo.compress_size
# 3828
exampleZip.close()
```
### 解压ZIP文件

```Python 
import zipfile, os
os.chdir('C:\\')
exampleZip = zipfile.ZipFile('example.zip')
exampleZip.extractall()     # 可以指定路径 exampleZip.extractall('C:\\ delicious')
exampleZip.close()
```
### 创建并添加到ZIP文件

```Python
import zipfile
newZip = zipfile.ZipFile('new.zip', 'w')    # 必须以w模式打开
newZip.write('spam.txt', compress_type=zipfile.ZIP_DEFLATED)
newZip.close()
```