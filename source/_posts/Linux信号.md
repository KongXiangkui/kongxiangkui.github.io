---
title: Linux信号
date: 2020-04-12 19:00:18
tags:
- Linux
categories:
- Linux
toc: true
---
本文介绍 Linux Signals 的相关信息。

Signal are software interrupts. Most nontrivial application programs need to deal with signals.

Signals provide a way of handling asynchronous events -- for example, a user at a terminal typing the interrupt key to stop a program or the next program in a pipeline terminating premeturely.

Signals are classic examples of asynchronous events. They occur at what appear to be random times to the process. The process can't simply test a variable to see whether a signal has occurred; instead, the process has to tell the kernel "if and when this signal occudrs, do the following."
<!-- more -->
## 基础
 
Process处理信号的三种选择
- Ignore the signal
- Let the default action occur
- Provide a function that is called when the signal occurs(this is called "cataching" the signal). By providing a fucntion of our own, we'll know when the signal occurs and we can handle it as we wish.

Two signal terminal keys:
- interrupt key: DELETE, Ctrl+C
- quit keys: Ctrl+\

> SIG_INT: interrupt the currently running process.

kill function 

## Signal Concepts

First, every signal has a name. These names all begin with the three characters SIG.

## signal Function

```C
#include <signal.h>     // <signal.h> is defined by ISO C, which doesn't involve multiple processes, process groups, terminal I/O and the like. Therefore, its definition of signals is vague enough to be almost useless for UNIX systems.

// Returns: desposition of signal if OK, SIG_ERR on error
void (*signal(int signo, void (*func)(int)))(int);