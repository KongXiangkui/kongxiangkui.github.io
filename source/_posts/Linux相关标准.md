---
title: Linux相关标准
date: 2020-04-13 11:36:01
tags:
- Linux
categories:
- Linux
toc: true
---
本文介绍 Linux/UNIX 的相关标准及实现。
<!-- more -->
## ISO C


## POSIX

1. POSIX(Portable Operating System Interface, 1003.1-1998) is a family of standards initially developed by the IEEE(Institute of Electrical and Electronics Engineers). POSIX was later extended to include many of the standards and draf standards with the 1003 designation, including the shell and utilities(1003.2)
2. IEEE 1003.1-1990 (also ISO/IEC9945-1:1990): POSIX.1
3. IEEE 1003.1 working group continued to make changes to the standard. IEEE 1003.1 (ISO/IEC 9945-1:1996) standard included the 1003.1-1990 standard, the 1003.1b-1993 real-time extensions standard, and the interfaces for multithreaded programming, called *pthreads* for POSIX threads.
4. ...
5. ISO/IEC 9945:2009: be based on several other standards(IEEE Standard 1003.1, 2004 Edition; Open Group Technical Standard, 2006, Extended API Set, Parts 1-4; ISO/IEC 9899:1999, including corrigenda)

## The Single UNIX Specification

- a superset of the POSIX.1
- specifies additional interfaces that extend the functionality provided by the POSIX.1 specification.

The *X/Open System Interfaces(XSI)* option in POSIX.1 describes optional interfaces and defines which optional of POSIX.1 must be supported for implementation to be deemed *XSI conforming*.
- file synchronization
- thread stack address and size attributes
- thread process-shared synchronization
- _XOPEN_UNIX symbolic constant

> only XSI confirming implementations can be called UNIX systems.
> The Open Group owns the UNIX trademark and use the Single UNIX Specificationto define the interfaces an implementation must support to call itself a UNIX system.(生厂商必须经过一致性测试，才有权力使用UNIX商标)

The single UNIX Specification
1. The first version(Spec 1170) was published by X/Open in 1994.
2. The second version was published by The Open Group in 1997.
3. The third version(SUSv3) was published by The Open Group in 2001. 
4. The fourth version(SUSv4) was published in 2010.

## 其他标准

- FIPS, Federal Information Processing Standard(by the U.S. government)


## 实现

### UNIX　System V Release 4

by AT&T's UNIX System Laboratiries.

System V Interface Definition(SVID)

### BSD, Berkeley Software Distribution

by the Computer Systems Research Group(CSRG) at the University of California at Berkeley.

### FressBSD

### Linux

### Mac OS X

The core operating system is called "Darwin" and is based on a combination of the Mach kernel, the FreeBSD operating system, and an object-oriented framework for drivers and other kernel extensions.

> Darwin(达尔文）是由苹果电脑于2000年所释出的一个开放原始码操作系统。Darwin 是MacOSX 操作环境的操作系统成份。苹果电脑于2000年把Darwin 释出给开放原始码社群。

### Solaris

by Sun Microsystems(now Oracle).
based on System V Release 4.

## Limits

