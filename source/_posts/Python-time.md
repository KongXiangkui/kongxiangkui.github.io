---
title: Python时间模块
date: 2020-04-01 14:56:11
tags:
- Python
categories:
- [Python, 语言]
toc: true
---
本文主要介绍Python与时间相关的模块
<!-- more -->
## time module

当前时间与 Unix epoch timestamp 所过去的秒数

>  The Unix epoch is a time reference commonly used in programming: 12 Am on January 1, 1970, Coordinated Universal Time (UTC).
> Built-in time module allows your Python programs to read the system clock for the current time.

```Python 
import time

# time.time()返回值 从Unix epoch timestamp 到 time.time() 所过去的总的秒数（float value）
now = time.time()

# 使用round() 函数设置float的精度
round(now, 2)

# 延时10s
# Ctrl+C 不会中断 time.sleep(),会raise KeyboardInterrupt exception
seconds = 10
time.sleep(seconds)

```

```Python
import time
# profile your code.
def calcProd():
    # Calculate the prduct of the first 100,000 numbers.
    product = 1
    for i in range(1, 100000):
        product *= i
    return product

startTime = time.time()
prod = calcProd()
endTime = time.time()

print('The result is %s digits long.' % (len(str(prod))))
print('Took %s seconds to calculate.' % (endTime - startTime))
```

## datetime module

返回更方便的时间格式

datetime object:可以使用比较运算符进行比较

```Python
import datetime
datetime.datetime.now()     # 返回一个datetime object
# 返回值: datetime.datetime(2020, 4, 1, 16, 3, 49, 158910)

dt = datetime.datetime(2015, 10, 21, 16, 29, 0)     # 指定具体时刻
print(dt.year, dt.month, dt.day)
# 2015 10 21
print(dt.hour, dt.minute, dt.second)
# 16 29 0

datetime.datetime.fromtimestamp(1000000)
# 结果: datetime.datetime(1970, 1, 12, 5, 46, 40) 
datetime.datetime.fromtimestamp(time.time())
# 结果：datetime.datetime(2020, 4, 1, 16, 9, 58, 951464)

halloween2015 = datetime.datetime(2015, 10, 31, 0, 0, 0)
newyear = datetime.datetime(2020,1,1,0,0,0)
halloween2015 == newyear              
False
```

> Pacific Standard Time太平洋标准时间
> 以上结果在不同时区的显示不同

timedate.timedelta()
- key arguments：weeks, days, hours, minutes, seconds, milliseconds, microseconds
- 结果用days, seconds, microseconds
> 没有month, year，时间长度不确定，取决于具体的月或年

```Python
delta = datetime.timedelta(days=11, hours= 10, minutes=9, seconds=8)
print(delta.days, delta.seconds, delta.microseconds)    # timedelta的属性days存储11， seconds存储36548 (10 hours，9 minutes，8 seconds)
# 11 36548 0
delta.total_seconds()
# 986948.0
srt(delta)      # 返回值的可读性更好
# '11 days, 10:09:08'
print(delta)
# 11 days, 10:00:00

# 可以用于时间的计算
dt = datetime.datetime.now()        # datetime.datetime(2020, 4, 1, 16, 36, 27, 208680)
thousandsDays = datetime.timedelta(days=1000)
dt + thounsandDays
# datetime.datetime(2022, 12, 27, 16, 36, 27, 208680)

may1st = datetime.datetime(2015,5,1,0,0,0)
aboutThirtyYears = datetime.timedelta(days=365*30)
may1st - aboutThirtyYears
# 直接在交互shell输入的结果：datetime.datetime(1985, 5, 8, 0, 0)
print(may1st - aboutThirtyYears)
# print()的结果：2020-04-01 16:36:27.208680
print(may1st - 2 * aboutThirtyyears)
1955-05-16 00:00:00
```

### datetime Objects 和 strings 的转化

- strftime()，将datetime object 转化为 string
- strptime(), 将string 转化为 datetime object

| strftime directive | Meaning|
| ------ | ----- | -------|
| %Y | Year with century, as in '2014'|
| %y | Year without century, as in '00' to '99'(1970 to 2029) |
| %m | Month as a decimal number, '01' to '12' |
| %B | Full month Name, as in 'November' |
| %b | Abbreviated month name, as in 'Nov' |
| %d | day of the month, '01' to '31' |
| %j | day of the year, '001' to '366' |
| %w | day of the week, '0'(Sunday) to '6'(Saturday) | 
| %A | Full weekday name, as in Monday |
| %a | Abbreviated weekday name, as in 'Mon' |
| %H | Hour(24-hour clock), '00' to '23' |
| %I | Hour(12-hour clock), '00' to '12' |
| %M | Minutes, '00' to '59' |
| %S | Seconds |
| %p | 'AM' or 'PM' |
| %% | literal % character |

```Python
import datetime
may1st = datetime.datetime(2020,5,1,0,0,0)
may1st.strftime('%Y-%m-%d %H:%M:%S)
# 返回值： '2020-05-01 00:00:00'
may1st.strftime('%B of %y)
# 返回值：'May of 20'
datetime.datetime.strptime('October 21, 2015', '%B %d, %Y')
# 返回值：datetime.datetime(2015, 10, 21, 0, 0)
```


## Multithreading

创建线程thread

thread object

```Python
import time, datetime

def takeANap():
    time.sleep(5)
    print('Wake ip!')

print('Start of program.')

threadObj = threading.Thread(target=takeANap)   
# keyword argument：target 
# 使用takeNap而不是takeNap()，是因为哟啊传递函数本身作为参数，而不是调用函数传递其返回值
threadObj.start()

print('End of program')
```
> 只有所有的线程threads终止后，程序才会终止

```Python
print('Cat', 'hel', 'name', sep=' & ')
# Cat & hel & name
```

```Python
import threading
threadObj = threading.Thread(target=print, args=['Cat','hel','name'], kwargs={'sep': ' & '})
threadObj.start()
# Cat & hel & name
```

### 多线程存在的问题——Concurrency Issues

解决措施：从来不要让多个线程读写同一个变量（一个新的Thread object 只使用局部变量）

[多线程](http://nostarch.com/automatestuff "A beginner’s tutorial on multithreaded programming")

### Launching Other Programs from Python——subprocess模块

Popen object的方法：
- poll(): 如果进程依然在运行，call poll()会返回None；如果进程结束，返回process's integer exit code(to indicate whether the process terminated without errors——进程结束码为0；有error为1，取决于系统)
- wait(): Wait for child process to terminate. 


Popen() 函数
返回值为 Popen object类型

> built-in subprocess module
> P代表process
> 每一个process可以有多个threads。thread之间可以相互可以直接读写，process之间不直接读写

```Python
import subprocess
# Windows
subprocess.Popen('C:\\Windows\\System32\\calc.exe')
# shell交互结果： <subprocess.Popen object at 0x000001649BAE3508>
# Linux
subprocess.Popen('/usr/bin/gnome-calculator')

# OS X

```

```Python
import subprocess
# 传递命令行参数(list), 会作为sys.argv的值传递给the launched program
subprocess.Popen(['C:\\Windows\\System32\\notepad.exe', '.\\hello.txt'])
# <subprocess.Popen object at 0x000001649BADFC88>
```

### Running Other Python Scripts

程序运行其他脚本，程序和其他脚本是两个分开的进程(不能共享变量)

```Python
import subprocess

# Window
subprocess.Popen(['C:\\Users\\20499\\AppData\\Local\\Programs\\Python\\Python37\\python.exe', 'hello.py'])

# Linux
subprocess.Popen(['/usr/bin/python3', 'hello.py'])

# OS X
subprocess.Popen(['/Library/Frameworks/Python.framework/Versions/3.7/bin/ python3', 'hello.py'])
```

### Open Websites with Python

webbrowser模块
webbrowser.open() 打开浏览器进入某个具体的网站

### Open Files with Default Applications

双击文件用默认程序打开(每一个系统都有这样的程序)
- start program(Windows)
- see program(Linux)
- open program(OS X)

```Python
import subprocess

subprocess.Popen(['start', '.\hello.txt'], shell=True)  # 'start' for Windows(shell=True只在Windows系统需要)

subprocess.Popen(['open', '/Applications/Calculator.app/']) # 'open' for OS X
```
## 计算机任务自动化

[计算机的任务自动化](http://nostarch.com/automatestuff/)

- Task Scheduler on Windows
- launchd on OS X
- cron scheduler on Linux

The UNIX Philosophy——KISS

Programs well designed to be launched by other programs become more powerful than their code alone . The Unix philosophy is a set of software design principles established by the programmers of the Unix operating system (on which the modern Linux and OS X are built) . It says that it’s better to write small, limited- purpose programs that can interoperate, rather than large, feature-rich applications . The smaller programs are easier to understand, and by being interoperable, they can be the building blocks of much more powerful applications .

> 不重复造轮子

