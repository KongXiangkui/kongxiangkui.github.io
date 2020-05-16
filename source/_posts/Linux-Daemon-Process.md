---
title: Linux Daemon Process(守护进程)
date: 2020-04-24 23:22:18
tags:
- Linux
categories:
- Linux
toc: true
---
Daemons are proesses, which started when the system is bootstrapped ad terminate only when the system is shut down.(Because they don't have a cntrolling terminal, they run in the backgraoud)
<!-- more -->

## Daemon Characteristics



```shell
$ ps -axj   # print the status of various processes in the system.
# -a option: shows the status of processes owned by others
# -x option: show processes that don't have a controlling termianl.
# -j option: display the job-related information(the session ID, process group ID, controlling terminal, and terminal proccess group ID)
```

> 出于安全考虑，UNIX并不允许我们使用ps去查看不属于我们自己的进程