---
title: Linux文件
date: 2020-04-10 10:52:27
tags:
- Linux
- C
categories:
- [Linux]
- [C]
toc: true
---
本文介绍Limux文件和文件系统的相关概念与操作。
<!-- more -->
## 概念

**File System**
*a directory is a file that contains directory entries.(we can think of each directory entry contains a filename along with a structure of information discribing the attributes of the file. The attributes of a file are such things as the type of file(regular file or directory), the size of the file, the owner of the file, permission of the file(whether other users may access this file), and when the file was last modified.)*

目录可以理解为一个文件，存储了目录的结构，包括每个文件的文件名和属性以及子目录

> UNIX file systems 并不存储文件的属性，因为很难保持同步

**Filename**
the name in a directory are called filenames

> UNIX下，文件名**不能包含**的两种字符 the slash character / 和 空格 the null character。Nevertheless, it's good practice to restrict the characters in a filename to a subset of the normal printing characters. 如果我们在文件名中使用了一些特殊字符，在shell中使用目录，必须使用引用''(shell's quoting mechanism)
> Windows下，文件名可以包含空格？？？
> POSIX recommends restricting filenames to consistof the following characters: letters(a-z, A-Z), numbers(0-9), period(.), dash(-), and underscore(_).

Two files are automatically created whenever a new directory is created: .(called dot), ..(called dot-dot). 
- Dot: 指的是当前目录
- Dot-dot: 指的是父目录

> In root directory, dot-dot is the same as dot.

> UNIX System V file system 支持14个字符
> BSD 支持255个字符
> 目前大部分UNIX支持至少255个字符

**Pathname**

> UNIX 用slash / 分隔
> Windows 用 backslash \ 分隔

- absolute pathname
- relative pathname

**Working Directory**

每一个 进程process 都有一个 working directory(sometimes called the current working directory)

使用 chdir() 改变进程的 working directory

**Home Directory**

when we log in, the working directory is set to our home directory

**虚拟目录virtual directory**：只包含一个根root目录的基础目录——协调管理各个存储设备

**根驱动器**：linux安装的第一块硬盘

文件系统层级标准（filesystem hiterarchy standard，FHS）

[FHS](http:www.pathname.com/fhs)

## 常见文件系统

### ext文件系统

ext文件系统(extended filesystem)使用**虚拟目录操作硬件设备**，在物理设备上按**定长的块存储数据**

采用**索引节点**的系统存放虚拟目录中的文件信息，索引节点系统在每个物理设备创建一个单独的表（**索引节点表**）存储这些文件的信息存放在虚拟目录的每个文件在索引节点表都有一个条目。Linux通过唯一的数值（**索引节点号**，由文件系统分配，并用于标识文件）来索引结点表中的每个索引节点

### ext2文件系统

扩展索引节点表的格式，保存系统上文件的更多信息，改变了文件在数据块中的存储方式；但产生碎片化（fragmentation，存储数据用的块分散在整个设备），降低文件系统的性能，保存文件时，按组分配磁盘，减轻碎片化。文件系统存储文件时，要更新索引节点表，有时会造成不同步。

### 日志文件系统

先将文件写入临时文件（日志，journal）数据成功写入存储设备和索引节点表后，再删除日志

文件系统日志方法

| 方法     | 描述                                     |
| -------- | ---------------------------------------- |
| 数据模式 | 索引节点和文件都会被写入日志             |
| 有序模式 | 仅索引节点写入，且只有数据成功写入才删除 |
| 回写模式 | 仅索引节点写入，且不控制文件数据何时写入 |

  （从上到下，性能升高，安全性降低）

- ext3文件系统

  给每个存储设备增加日志文件。默认采用有序模式的日志功能。无法恢复误删文件，无任何内建的压缩功能，不支持加密

- ext4文件系统

  支持数据压缩和加密，支持**区段（extent）特性**，区段在存储设备上按块分配空间，但在索引节点中只保存起始块的位置（节省索引节点列表的空间）；引入**块预分配技术（block preallocation)**,可以为文件分配所需要的可以变大的块，用0填满预留数据块

- Resister文件系统

  只支持操作回写日志模式；可以在线调整系统的大小；**尾部压缩（tailpacking)**技术,可以将一个文件的数据填进另一个文件的数据块的空白空间

- JFS文件系统（Journaled filesystem最古老）

  采用有序日志模式,IBM开发；采用基于区段的文件分配，为每个写入存储设备的文件分配一组块，减少碎片

- XFS文件系统

  采用回写日志模式，允许在线调整文件系统大小，只能扩大

COW（copy-on-write)文件系统

兼顾安全性和性能，如要修改数据，使用克隆或可写快照。数据修改后不会覆盖原有数据

- ZFS文件系统: Sun研发用于OpenSolaris操作系统
- Btrf文件系统（B树文件系统）: Oracle研发用于OpenSUSE系统

## Linux目录名称及描述

| 目录名 |                    描述                    |
| :----: | :----------------------------------------: |
|   /    |             根目录，不存储文件             |
|  /bin  |       二进制目录，存放用户级GNU工具        |
| /boot  |           启动目录，存放启动文件           |
|  /dev  |        设备目录， 用于创建设备节点         |
|  /etc  |              系统配置文件目录              |
| /home  |            主目录，创建用户目录            |
|  /lib  |     库目录，存放系统和应用程序的库文件     |
| /media |    媒体目录，可移动媒体设备的常用挂载点    |
|  /mnt  |                  挂载目录                  |
|  /opt  |         可选目录，存放第三方软件包         |
| /proc  | 进程目录，存放现有硬件及当前进程的相关信息 |
| /root  |               root用户主目录               |
| /sbin  |   系统二进制目录，存放管理员级的GNU工具    |
|  /run  |     运行目录，存放系统运作时的运行数据     |
|  /srv  |      服务目录，存放本地服务的相关文件      |
|  /sys  |    系统目录，存放系统硬件信息的相关信息    |
|  /tmp  |                  临时目录                  |
|  /usr  |  用户二进制目录，用户级GNU工具和数据文件   |
|  /var  | 可变目录，存放经常变化的文件，如：日志文件 |

## 文件 I/O

> *unbuffered I/O*: each read() and write() invokes a system call in the kernel.
> These unbuffered I/O functions are not part of ISO C, but are part of POSIX.1 and the the Single UNIX Specification.

多进程之间共享资源——原子操作atomic operation

问题：
文件如何在多进程之间共享？涉及到哪一种内核的数据结构


**File Description** are normally small non-negative integers that the kernel uses to identify the files accessed by a process. 
当打开一个文件或者创建一个新文件，内核会给进程返回一个File Descriptor。当我要读写一个文件时，会用open()或者create()返回的文件描述符作为read()或者write()的参数。

> 每一个文件描述符会与一个打开文件相对应，同时，不同的文件描述符也会指向同一个文件。相同的文件可以被不同的进程打开也可以在同一个进程中被多次打开。系统为每一个进程维护了一个文件描述符表，该表的值都是从0开始的.

UNIX shell会将file descriptor 0 与一个进程的 standard input 关联，file descriptor 1 与 standard output 关联，file descriptor 2 与 standard error 关联。(只是shell或者application的特性，而不是UNIX kernel的)

### 文件操作

打开和创建文件

```c
#include <fcntl.h>    // 定义了很多宏和open,fcntl函数原型

/* 打开文件 
 * Both return: File Descriptor if OK，-1 on error
 */
int open(const char *path, int oflag, ... /* mode_t mode */);
int openat(int fd, const char *path, int oflag, .../* mode_t mode */);

/* 创建文件 
 * 早期，open() 的第二个参数只能是0，1，2，所以无法打开一个不存在的文件
 * Returns: File descriptor opened for write-only if OK, -1 on error 
 */
int create(const char *path, mode_t mode);    //等价于 open(path, O_WRONLY | O_CREAT | O_TRUNC, mode); 

/* 使用 open() 更好 */
```

关闭文件

```c
#include <unistd.h>   // 定义了更多的函数原型

/* 关闭文件
 * Closing a file release any record locks that the process may have on the file.
 * When a process terminates, all of its open files are closed automatically by the kernel.(许多程序利用这一点不明确关闭文件)
 * Returns： 0 if OK, -1 on error
 */
int close(int fd);
```

移动文件的读写位置

```c
#include <unistd.h>
/* 移动文件的读写位置
 * Every open file has an associated "current file offset," normally a non-negative integer that measures the number of bytes fro the beginning of the file.
 * 每一个打开的文件都会和 "current file offset" 关联。这是一个非负的整数从文件开始的总的bytes
 * Read and write operations normally start at the current file offset and cause the offset to be incremented by the number of bytes read or written.
 * By default, this offset is initialized to 0 when a file is opened, unless the O_APPEND option is specfied.
 * 目前，大多数平台提供 32-bits file offset 和 64-bits file offset. The single UNIX Specification 可以通过sysconf()判断支持哪一种环境
 * 参数说明
 * The interpretation of the offset depends on the value of the whence argument
 * if whence is 
   SEEK_SET: the file's offset is set to offset bytes from the beginning of the file.
   SEEK_CUR: the file's offset is set to its current value plus the offset.(the offset can be positive or negative.)
   SEEK_END: the file's offset is set to the size of the file plus the offset.(the offset can be positive or negative.)
 * Returns: new file offset if OK, -1 on error(只有-1才说明是错误，而不是小于0)
 */
off_t lseek(int fd, off_t offset, int whence);

/* 应用：seek当前的位置 */
off_t currpos;
currpos = lseek(fd, 0, SEEK_CUR);


/* 应用：判断是否可以seek 
 * If the file descriptor refers to a pipe, FIFO, or socket, lseek sets errno to ESPIPE and returns -1
 */
if (lseek(STDIN_FILEENO, 0, SEEK_CUR) == -1)
  print("cannot seek\n");
else 
  print("seek OK\n");
```

读文件

```c
#include <unistd.h>

/* 读一个打开的文件
 * The read operation starts at the file's current offset
 * Returns: numbers of bytes read, 0 if end of file, -1 on error
 */
ssize_t read(int fd, void *buf, size_t nbytes);

/* POSIX.1 changed the prototype for this function inseveral ways. */
int read(inf fd, char *buf, unsigned nbytes);
```

写文件

```c
#include <unistd.h>

/* 写一个打开的文件
 * The return value is usually equal to the nbytes argument; otherwise, an error has occurred.
 * 常见的写文件的错误：filling up a disk or exceeding the file size limit for a given process.
 * The write operation starts at the file's current offset
 * Returns: number of bytes written if OK, -1 on error
 */
ssize_t write(int fd, const void *buf, size_t nbytes);
```

### I/O Efficiency

Most file system support some kind of read-ahead to improve performance. When sequential reads are detected, the system tries to read in more data than an application requests, assuming that the application will read it shortly.

编写程序进行测试

cache


### FIle Sharing

Kernel data structures for open files

读:
Two independent processes with the same file open

写:
原子操作atomic operations：use when multiple process append to the same file, create the same file.
暂时挂起一个进程

```c
#include <unistd.h>

//原子操作atomic operations

/* 等价于call lseek followed by a call to read, with the following exceptions
 * These is no way to interrupt the two operations that occur when we call pread.
 * The current file offset is not upddated.
 * Returns: number of bytes read, 0 if end of file, -1 on error.
 */
ssize_t pread(int fd, void *buf, size_t nbytes, off_t offset);  

/* 等价于call lseek followed by a call to write, with similar exceptions
 * Returns: number of bytes written if OK, -1 on error.
 */
sszie_t pwrite(int fd, const void *buf, size_t nbytes, off_t offset);
```

```c
#include <unistd.h>

/* An existing file descriptor is duplicated by either of the folloeing functions
 * Both reuturn: new file descriptor if OK, -1 om error
 */


int dup(int fd);

int dup2(int fd, int fd2);    // an atomic operation
```

delay write: When we write data to a file, the data is normally copied by the kerel into one of its buffers and queued for writing to disk at some later time. The kernel eventually writes all the delayed-writed blocks to disk, normally when it needs to reuse the buffer for some other disk block.
(a buffer cache or page chache in the kernel)

```c
#include <unistd.h>

// 同步

/*
 * This function refers to a single file, specified by the file descriptor fd, and waits for the disk writes to complete before returning.
 * 涉及文件的整体包括数据和属性
 * 应用场景: 当一个App如数据库等需要确保修改的block已经写入到硬盘
 * Returns: 0 if OK, -1 on error
 */
int fsync(int fd); 

/* (部分平台不支持)
 * 只影响一个文件的数据部分
 * Returns: 0 if OK, -1 on error
 */
int fdatasync(int fd);

/*
 * 
 * queue all the modified block buffers for writing and returns.(It does not wait foe the disk writes to take place.)
 * This function will be called periodically(usually every 30 seconds) from a system daemon, often called update.
 */
void sync(void);
```
### fcntl() and ioctl()

```c
#include <fcntl.h>

/* Change the properties of a file that is already open.
 * fcntl()的作用：
 * 1. Duplicate an existing descriptor (cmd = F_DUPFD or F_DUPFD_CLOEXEC)
 * 2. Get/Set file descriptor flags (cmd = F_GETFD or F_SETFD)
 * 3. Get/Set file status flags (cmd = F_GETFL or F_SETFL)
 * 4. Get/Set asynchronous I/O ownership (cmd = F_GETOWN or F_SETOWN)
 * 5. GetSet record locks (cmd = F_GETLK, F_SETLK, or SETLKW)
 * Returns: dependes on cmd if OK, -1 on error
 */
int fcntl(int fd, int cmd, ... /* int arg */ );
```

```c
#include <unistd.h>     //System V
#include <sys/ioctl.h>  //BSD and Linux

/* this function has been the catchall of I/ operations.(Terminal I/O was the biggest user of this function)
 * POSIX.1 has replaced the terminal I/O operations with seperate functions.
 * Each device can define its own set of ioctl commands. The system, however, provides generic ioctl commands for different classes of devices.
 * Returns: -1 on error, something else if OK
 */
int ioctl(int fd, int represent, ...);
```

### /dev/fd

/dev/fd whose entries are files named 0, 1, 2, and so on. Opening the file /dev/fd/n is equivalent to duplicating descriptor n, assuming that descriptor n is open.

```c
fd = open("/dev/fd/0", mode);  // most systems ignore the specified mode???
```

> The Linux implementation of /dev/fd is an exception. It maps file descriptors into symbolic links pointing to the underlying physical files.
> When you open /dev/fd/0, you are really opening the file associated with your sandard input.
> Thus the mode of the new ifle descriptor returned is unrelated to the mode of the /dev/fd file descriptor.

一些系统提供路径 /dev/stdin, /dev/stdout, 和 /dev/stderr, 分别等价于 /dev/fd/0, /dev/fd/1, /dev/fd/2.

```shell
# the cat program specifically looks for an input filename of - and ses it to mean standard input.
# First, cat reads file1, then its stand input (the output of the iflter program on file2), and then file3.
$ filter file2 | cat file1 - file3 | lpr    # The special meaning of - as a command line argument to refer to the standard input or the standard output is a 

# if /dev/fd is supported, the special handling of - can be removed from cat
$ filter file2 | cat file1 /dev/fd/0 file3 | lpr

```

## Files and Directories

```c
#include <sys/stat.h>

// All four return: 0 if OK, -1 on error

/* Given a pathname, the stat function returns a structure of information about the named file.
 *
 */
int stat(const char *restrict pathname, struct stat *restrict buf);

/* Given a pathname, the fstat function obtains information about the file that is already open on the descriptor fd.
 *
 */
int fstat(int fd, struct stat *buf)

/* Given a pathname, the lstat function returns a structure of information about the named file, but when the named file is referenced by the symbolic link, lstat returns information about the symbolic link, not the file referenced by the symbolic link.
 *
 */
int lstat(const char *restrict pathname, struct stat *restrict buf);

/* Given a pathname, the fstatat function provides a way to return the statistics for a pathname relative to an open directoory re[resented by the fd argument.]
 * 参数：
 * flag: control whether symbolic links are followed; when the AT_SYMLINK_NOFOLLOW is set, fstatat will not follow symbolic links, but rather returns information about the file to which the symbolic link points. Otherwise, the default is to follow symbolic links, returning information about the file to which symbolic link points 
 */
int fstatat(int fd, const char *restrict pathname, struct stat *restrict buf, int flag);
```

### File types

- regular file(S_ISREG()): A file contains dato of some form.

> 关于二进制文件与文本文件：
> There is no distinction to UNIX kernel whether this data is text or binary.
> Any interpretation of the contents of a regular file is left to the application processing the file.
> To executate a program, the kernel must understand its format. All binary executable files conform to a format that allows the kernel to iddentify where to load a program's' text and data.

- direcory file(S_ISDIR()): A file contains the names of the other files and pointers to information on these files. Any process that has read permission for a directory file ecan read the contents of the directory, but only the kernel can write directly to a directory file. Processes must use the functions desribed in this chapter to make changes to a directory.

- block special file(S_ISBLK()): A type of file providing buffered I/O access in fixed-size units to devices such as disk drives.(FreeBSD no longer supports block special files. All access to devices is through the character special interface.)
- character special file(S_ISCHR): A type of file providing unbuffered I/O access in variable-sized units to devices.

> All devices on a system are either block special files or character special files.

- FIFO(S_ISFIFO()): A type of file used for communication between processes. It's sometimes called a named pipe. 
- socket(S_ISSOCK()): A type of file used for network communication between processes.(A socket can also be used for non-network communication between processes on a single host. We can use sockets for interprocess communication, IPC.)
- symbolic link(S_ISLINK()): A type of file that points to another file.

> The type of a file is encoded in the **st_mode** member if the stat structure.

### Set-User-ID and Set-Grooup-ID

Every process has six or more IDs associated with it.

1. who we really are: real user ID, real group ID (be taken from our entry in the passwdord file when we log in.)
2. used for file access permission checks: effective user ID, effective group ID, supplementary group IDs (Normally, the effective user ID equals the real user ID, the effective group ID equals the real group ID)
3. saved by exec functions: saved set-user-ID, saved-group-ID(contain copies of the effective user ID and the effective group ID, respectively, when a program is executed.)

> The saved IDs are required as of the 2001 version of POSIX.1

> Every file has an owner(specified by the st_uid member if the stat structure) and a group owner(specified by the st_gid member if the stat structure)

> effective-usr-ID, effective-group-ID(进程执行时)通常和real user ID, real group ID(文件所有者)相等，但需要设置一个特殊的标志位set-user-ID, set-group-ID

### File Access Permissions

> All types of file have permissions.

nine permission bits of each file(\<sys/stat.h>): [st_mode amsk: meaning]

u(for user，not owner)
S_IRUSR: user-read
S_IWUSR: user-write
S_IXUSR: user-execute

g(for group)
S_IRGRP: group-read
S_IWGRP: group-write
S_IXGRP: group-execute

o(for other, not world)
S_IROTH: other-read
S_IWOTH: other-write
S_IXOTH: other-execute

The two owners IDs are properties of the file
The two effective IDs and the supplementary groupIDs are properties of the process.

```shell
# chown 改变文件的user ID
# 改变某个文件或目录的所有者和所属的组，该命令可以向某个用户授权，使该用户变成指定文件的所有者或者改变文件所属的组。用户可以是用户或者是用户ID，用户组可以是组名或组ID。文件名可以使由空格分开的文件列表，在文件名中可以包含通配符。
# 只有文件主和超级用户才可以便用该命令
chown root a.out  # change file's user ID to root
# chmod 改变文件的permission bits
chmod u+s a.out   # turn on set-user-ID
```

关于文件权限的总结
- Whenever we want to open any type of file by name, we must have execute permission in each directory mentioned in the name, including the current directory, if it is implied.(This is why we the execute permission bit for a directory is often called the search bit.)
  
  open /usr/inlcude/stdio.h: We need execute permission in the directory /, execute permission in the directory /usr, and execute permission in the directory /usr/include. We then need appropriate permission for the file itself, depending on how we're trying to open it: read-only, read-write, and so on.
  
  open ./stdio.h: We need the execute permission in the current directory to open the file stdio.h

> read permission for a directory: let us read the diectory, obtaining a list of all the filenames in the directory.
> execute permission for a directory: let us pass through the directory when it is a pathname that we are trying to access.(We need to search the directory to look for a specific filename.)

- Read permission for a file determines whether we can open an existing file for reading: the O_RDONLY and the O_RDWR flags for the open function.
- Write permission for a file determines whether we can open an exiting file for writing: the O_WRONLY and the O_RDWR flags for the open function.
- We must have write permission for a file to specify the O_TRUNC flag in the open funcion.
- We cannot create a file in a directory unless we have write permission and execute permission in the directory.
- To delete an existing we need write permission and execute permission in directory containing the file. We do not need read permission or write permission for itself.
- Execute permission for a file must be on if we want to execute the file using any og the seven exec functions. The file also has to be a reguler file.

### access() and faccessat()

```c
#include <unistd.h>

/* 
 * 参数：
 * mode:
 * F_OK: test if a file exists
 * bitwise OR of any of the flags (R_OK: test for read permission; W_OK: test for write permission; X_OK: test for execute permission)
 */
int access(const char *pathname, int mode);

int faccessat(int fd, const char *pathname, int mode, int flag);
```

### umask()

```c
#include <sys/stat.h>

/* set the file mode creation mask for the process and returns the previous value
 * Returns: previous file mode creation mask(不会返回error)
 */
mode_t umask(mode_t cmask);

```

```shell
# umask command can be used to set or print the current file mode creation mask.
$ umask   # print the current file mode creation mask
002
# if we want to ensure that anyone can read a file, we should set the umask to 0
```
> whenever the process creates a new file or a new directory(open() or create() accepts a mode argument that specifies the new file's access permission bits)

User can set the umask value to control the default permissions on the files they create. The value is octal, with one bit representing one permission to be masked off

| Mask bit | Meaning |
| :------: | :-----: |
| 0400 | user-read |
| 0200 | user-write |
| 0100 | user-execute |
| 0040 | group-read |
| 0020 | group-write |
| 0010 | group-execute |
| 0004 | other-read |
| 0002 | other-write |
| 0001 | other-execute |

The Single UNIX Specification requires that the umask command support a symbolic mode of operation.

```shell
$ umask -S  # 以字符形式显示当前的掩码
u=rwx,g=rwx,o=rx
```

> umask设置的是权限“补码”
> chmod设置的是文件权限码

> 你的系统管理员必须要为你设置一个合理的 umask值，以确保你创建的文件具有所希望的缺省权限，防止其他非同组用户对你的文件具有写权限。在已经登录之后，可以按照个人的偏好使用umask命 令来改变文件创建的缺省权限。相应的改变直到退出该shell或使用另外的umask命令之前一直有效。一般来说，umask命令是在/etc /profile文件中设置的，每个用户在登录时都会引用这个文件，所以如果希望改变所有用户的umask，可以在该文件中加入相应的条目。如果希望永久 性地设置自己的umask值，那么就把它放在自己$HOME目录下的.profile或.bash_profile文件中。

### chmod(), fchmod(), and fchmodat()

```c
#include <sys/stat.h>

/* 修改某一具体文件的mode
 *
 */
int chmod(const char *pathname, mode_t mode);

/* 修改已打开的文件的mode
 * 
 */
int fchmod(int fd, mode_t mode);

int fchmodat(int fd, const char *pathname, mode_t mode, int flag);
```


关于Sticky bit：

现在的系统，大多数使用a virtual memory system and a faster file system.
sticky bit被扩展

The Single UNIX specialfication allows the sticky bit to be set for a directory.


### chown(), fchown(), fchownat() and lchown()

```c
#include <unistd.h>

// All four return: 0 if OK, -1 on error

/*
 *
 */
int chown(const char *pathname, uid_t owner, gid_t group);

/* change the ownership of the open file referenced by the fd argument.
 *
 */
int fchown(int fd, uid_t owner, gid group);

/*
 *
 */
int fchown(itn fd, const char *pathname, uid_t owner, git_t group);

/*
 *
 */
int lchown(const char *pathname, uid_t owner, gid_t group);
```
> BSD-based systems have enforced the restriction that only the superuser can change ownership of a file.(可能会改硬盘空间的分配定额)
> System V has allow to all user to change the ownership of any files they own.


### File size

The st_size member of the stat structure contains the size of the file in bytes. This field is meaningful only for regular files, directories, and symbolic links.

holes in the file: be created by seeking past the current end of file and writing some data.


### File truncation

```c
#include <unistd.h>

// Both return: 0 if OK, -1 on error.
// truncate an existing file to length bytes. If the previous size of the file was greater than length, the data beyong length is no longer accessible. otherwise, if the previous size was less than length, the file size will increase and the data between the old end-of-file and the new end-of-file wil read as 0. 
int truncate(const char *pathname, off_t length);

int ftruncate(int fd, off_t length);
```

## File Systems

UNIX File System:
- BSD-drived UNIX file system (called UFS)
- PCFS that can read and write DOS-formatted diskettes
- HSFS that can read CD file systems

We can think of a disk drive being divided into one or more partitions. Each partition can contain a file system.

The i-nodes are fixed-length entries that contains most of the information about a file. 
- the file type
- thefile's access permission bits
- the size of the file
- pointers to the file's data blocks 
- ...

> Most of the information in the stat structure is obtained fron the i-node.

Only two items of interest are stored in the directory entry:
- the filename
- the i-node number

```c
#include <unistd.h>

// Both return 0 if OK, -1 on error.
// create a link to an existing file.(create a directory entry, bewpath, that references the existing file existingpath. If the newpath already exists, an error is returned.)
// The creation of the new directory entry and the increasement of the link count must be an atomic operation.

int link(const char *existingpath, const char *newpath);

// 参数:
// when the existing file is a symbolic linkm the flag argument controls whether the linkat function creates a link to the symbolic link or to the file which the symbolic link points. AT_SYMLINK_FOLLOW
int linkat(int efd, const char *existingpath, int nfd, const char *newpath);


// To remove an existing directory entry and discrement the link count of the file referenced by pathname.
// To unlink a file, we must have write permission and execute permission in the directory containing the directory entry
int unlink(const char *pathname);

int unlinkat(int fd, const char *pathname, int flag);
```

> 尽管POSIX.1允许跨文件系统的link，但大多数实现要求pathname在相同的文件系统

> Many file system implementations disallow hard links to directories for this reason.(hard links can cause loops in file system)

```c
#include <stdio.h>

// For a file, remove is identical to unlink.
// For a directory, remove is identical to rmdir
// Returns: 0 if OK, -1 on error
int remove(const char *pathname);
```

### rename and renameat Functions

```c
#include <stdio.h>

// Both return: 0 if OK, -1 on error

int rename(const char *oldname, const char *newname);

int renameat(int oldfd, const char *oldname, int newfd, const char *newname);
```

> The rename function is defined by ISO C for files.(The C standard dowsn't deal with directories). POSIX.1 expanded the definition to include directories and symbolic links.

### Symbolic Links

A symbolic link is an indirect pointer to a file, unlike the hard links, which pointed directly to the i-node of the file. Symbolic links were introduced to get around limitations of hard links.
- Hard links normally require that the link and the file reside in the same system.
- Only the superuser can create a hard link to a directory(when supported by the underlying file system).

> There are no file system limitations on a symbolic link and what it points to, and anyone can create a symbolic link to a directory.
> Symbolic links are typically used to "move" a file or an entire directory hierarchy to another location on a system.


```C
#include <unistd.h>

// Create a symbolic link.
// Both returns: 0 if OK, -1 on error
// actualpath and sympath need not reside in the same file system.
int symlink(const char *actualpath, const char *sympath);

int symlinkat(const cha *actualpath, int fd, const chat *sympath);


// Because the open function follows a symbolic link, we need a way to open the link itself and read the name in the link. These functions combine the actions of open, read, and close.
// Both return: number of bytes read if OK, -1 on error
ssize_t readlink(const char* restrict pathname, char *restrict buf, size_t bufsie);

ssize_t readlinkat(int fd, const char* restrict pathname, char *restrict buf, size_t bufsize);
```

### File Times

Three time fields are maintained for each file.

| Field | Description |
|-------|-------------|
| st_atim | last-access time of file data |
| st_mtim | last-modification time of file data |
| st_ctim | last-change time of i-node status |

```shell
$ ls   # display or sort only on one of the three time values.
# By default, -t or -l options uses the modification time of a file.
# -u option uses the access time.
# -c option uses the changed-status time.
```

```C
#include <sys/stat.h>

//Both returns: 0 if OK, -1 on error.
// 参数
// The times argument is a null pointer: In this case, both timestamps are set to the current time.
// The times argument points to an array of two timespec structures. If either tv_nsec filed has the special value UTIME_NOW, the corresponding timestamp is set to the current time, The corresponding tv_sec field is ignored
// The times argument points to an array of two timespec structures. If either tv_nsec field has the special value UTIME_OMIT, then the corresponding timestamp is unchanged. The corresponding tv_sec field is ignored.
// The times argument points to an array of two timespec structures and the tv_nsec field contains a value other than UTIME_NOW or UTIME_OMIT. In this case, the correspondingn timestamp is set to the value specified by the corresponding tv_sec and tv_nsec fields.

// you need to open the file to change its times.
// be included in POSIX.1
int futimens(int fd, const struct timespec times[2]);

// the utimensat function provides a way to change a file's times using the file's name.
// be included in POSIX.1
int utimensat(int fd, const char *path, const struct timespec timess[2], int flag);
```

```C
#include <sys/time.h>

// Returns: 0 if OK, -1 on error
// 参数
// The times argument is a pointer to array of two timestamps(access time and modification time, but they are expresssed in seconds and microseconds)
int utimes(const char *pathname, const struct timeval times[2]);


struct timeval {
  time_t tv_sec;  /* seconds */
  long   tv_Usec; /* microseconds */
}
```

```C
#include <sys/stat.h>

// Create a  new, empty directory. The entries for dot and dot-dot are created automatically.
// Both return: 0 if OK, -1 on error
int mkdir(const char *pathname, mode_t mode);

int mkdirat(int fd, const char *pathname, mode_t mode);
```

```C
#include <unistd.h>

// Returns: 0 if OK, -1 on error.
int rmdir(const char *pathname);
```

```C
#include <dirent.h>

// Both return: pointer if OK, NULL on error
DIR *opendir(const char *pathname);
DIR *fdopendir(int fd);

// Returns: pointer if OK, NuLL at end of directory or error
strcut dirent *readdir(DIR *dp);

void rewinddir(DIR *dp):

// Returns: 0 if OK, -1 on error
int closedir(DIR *dp);

// Returns: current location in directory associated with dp
long telldir(DIR *dp);

void seekdir(DIR *dp, long loc);
```

```C
#include <unistd.h>

// Both return: 0 if OK, -1 in error
int chdir(const charr *pathname);

int fchdir(int fd);

// Returns: buf if OK, NULL on error.
char *getcwd(char *buf, size_t size);
```

## Device Special Files

每一个文件系统都有最大和最小 device numbers，这个被编码在主要的系统数据类型 dev_t中。

系统的每一个文件的st_dev值是这个文件系统的device number, 包含了文件名和相应的i-node

只有character special files 和 block special files有一个st_rdev值,这个值包含了真是设备的device number

Normally, the only type of devices that are block special files are those that can contain random file systems--disk drivers, floppy disk drivers, and CD_ROMs.
## Reference

1. Richard Blum等 Linux命令行与shell脚本编程大全(Linux Command Line and Shell Scripting)
2. W. Richard Stevens等 UNIX环境高级编程(Advanced Programming in the UNIX Environment)
