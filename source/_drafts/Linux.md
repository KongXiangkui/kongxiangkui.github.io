---
title: Linux 系统编程
tags: 
- Linux
categories:
- [C]
- [Linux]
toc: true
---
本文介绍使用C语言进行 Linux/UNIX 进行编程的相关内容。
<!-- more -->

## 文件处理：

- opendir(): return a pointer to a DIR structure
- readdir(): return a pointer to a dirent structure or, when it's finished with the directory, a null pointer.
- closedir():

handle the errors:
- err_sys(): prints an informative message describing what type of error was encountered
- err_quit()

exit(): terminate a program. eixt(0) 表示正常结束，1~225表示有错误

## Processes 和 Threads

- getpid()

- fork()
- exec()
- waitpid

## 错误处理Error Handling

The file <errno.h> defines the symbol errnoand constant for each value that errono can assume.(constants通常以字母E开始)

关于错误处理的规则
1. Its value is never cleared by a routine if an error doesnot occur. Therefore, we should examine its value only when the return value from a function indicates that an error occurred.
2. The value of errno is never set to 0 by any of functions, and none of the constants defined in <errno.h> has a value of 0

<errno.h>中定义的error类型：
- fatal errors: no recovery action.
- nonfatal errors: print an error message on the user's screen or to a log file, and then exit. more rubustly.

## 用户管理

- getuid()
- getgid()

## Time values

UNIX 系统有两种不同的时间值
- Calendar Time: 计算从Epoch(00:00:00 January 1, 1970), COordinated Universal Time(UTC)到现在的总秒数。
- Process Time(CPU Time): 使用计算机的时钟计算一个进程所使用的Central Processor Resource。

UNIX 主要维护一个进程的三个时间值
- CLock time(wall clock time): 进程执行的总时间
- User Clock time: the CPU time attributed to user instructions.
- System Clock time: the CPU time attributed to the kernel when it executes on behalf of the process.

> 进程使用内核进行读写的时间也计算在上述时间当中。
> 使用time命令输出的格式取决于所使用的shell

## System Calls and Library Functions

All operating systems provide service points through which programs request services froom the kernel.

UNIX system call interfaces's definition is in the C language. Other operating system traditionally define the kernel entry points in the assembly language of the machine. 

> C 标准库有的会调用 kernel service， 但不是进入内核的接口。

系统调用sbrk(), increase or decrease the address space  of the process.(the system call in the kernel allocates an additional chunk of space on behalf of the process)
库函数malloc(), memory allocation. (manage this space from user level)

> 许多函数使用系统调用sbrk() 定义自己的malloc函数

system call与library function的区别：
- 系统调用提供最小的接口，库函数更多的功能