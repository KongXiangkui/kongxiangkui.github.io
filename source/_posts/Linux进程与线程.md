---
title: Linux进程与线程
date: 2020-04-12 19:05:33
tags:
- Linux
categories:
- Linux
toc: true
---
本文介绍 Linux 进程Processes 与 线程Threads 的相关内容。
<!-- more -->
## 基本概念

**程序Program**：存储在硬盘上的可执行文件executable file，程序program可以被读到内存memory中，被内核kernel执行。

**进程Processes**：程序的执行实例executing instance。有的操作系统也称作任务task。UNIX系统使用一个独一无二的non-negative integer作为**Process IDs**。

UNIX主要用于控制Process的函数
- fork
- exec
- waitpid

**线程Threads**：一个进程的线程共享 same address space, file descriptor, stacks。**Threads IDs** are local to a process. A thread ID from one process has no meaning in another process.


## Process Environment

Problems:
- How the main function is called when the program is executed
- How command-line arguments are passed to the new program
- What the typical memory layout looks like
- How to allocate additional memory
- How the process can use environment variables and various ways for the process to terminate
- Look at the longjmp and setjmp functions and their interaction with the stack.

```C
/* A C program starts excution with a function called main.
 * When a C program is executed by the kernel--by one of the exec functions, a special start-up routine(often coded in assembly language) is called before the main funciton is called.
 * C语言程序都是从调用main()开始 */
// The prototype for the main function
// argc is the number of command-line arguments
// argv is an array of pointers to the arguments
int main(int argc, char *argv[]);   
```

### Process Termination

Normal termination:

- Return from main
- Calling exit: perform certain cleanup processing(the fclose function is called for all open streams, this causes all buffered output data to be flushed(written to the file)) and then returns to the kernel.
- Calling _exit or _Exit:: return to the kernel immediately.
- Return of the last thread from its start routine.
- Calling pthread_exit from the last thread

Abnormal termination:

- Calling abort
- Receipy of a signal
- Response of the last thread to a cancellation request.

```C
// All three exit funtions expect a single integer argumnet, which we called the exit status.
#include <stdlib.h>

void exit(int status);          // be specified by ISO C

void _Exit(int status);         // be specified by ISO C

#include <unistd.h>

void _exit(int status);         // be specified by POSIX.1
```

> Returning an integer value from the main function is equivalent to calling exit with the same value. Thus exit(0); is the same as return(0); from the main function.

如果不指定返回值或者退出值，这个进程的exit status 是未定义的， 一般取决于结束时栈的内容和寄存器的内容

```shell
$ gcc hello.c
$ ./a.out
hello, world
$ echo $?   # print the exit status
13
```

```C
#include <stdlib.h>

// Returns: 0 if OK, nonzero on error
// We pass the address of a function as the argument to atexit. When the function is called, it is not passed any arguments and is not expected to return a value.
// any arguments and is not expected to return a value
int atexit(void (*func)(void));
```

> With ISO C and POSIX.1, exit first calls the exit handlers and then closes(via fclose) all open streams.


### Comman-Line Arguments

```C
int main(int argc, int argv)
{
    int i;
    for(i=0; i<argc; i++)               // echo all command-line args
    // for(i=0; argv[i] != NULL; i++)
        printf("argv[%d]: %s\n", i, argv[i]);
    exit(0);
}
```

### Environment List and Environment variables

Each program is passed an environment list, an array of character pointers, with each pointer containing the address of a null-terminated C string. The address of the array of poiters is contained in the global variable environ:]

```C
extern char **environ;
```

Many UNIX systems have provided a third argument to the main function that is the address of the environment list:

```C
int main(int argc, char *argv[], char *envp[]);
```

用全局变量environ传递不好，应尽可能使用getenv()和putenv()

Environment strings are usually of the form name=value

> The UNIX kernel never looks at these strings; their interpretation is up to the various applications.

The Shells uses numerious environment variables: HOME and USER are automatically at login, others are left for us to set.(We normally set the environment variables in a shell start-up filr to control the shell's actions.)

```C
#include <stdlib.h>

// Returns : pointer to a value associated with name, NULL if not found.
cahr *getenv(const char *name);
```

> 仅使用getenv去获取环境变量具体的值，而不是直接使用变量environ 

> ISO C doesn't define any enviroment variables.

```C
#include <stdlib.h>

// Take a string of the form name=value and places it in the environment lsit. If name already exists, its old definition is first removed.
int putenv(char *str);

// Set name to value. If name already exists in the environment, then (a)if rewrite is nonzero, the existng defintion for name is first removed; or (b) if rewrite is 0, an existing definition for name is not removed, the name is not set to the new value, and no error occurs.
int setenv(const char *name, const char *value, int rewrite);

// Remove any definition of name. It is not an error if such a definition does not exist.
int unsetenv(const char *name);
```

## Memory Layout of a C program

Historically, a C program has been composed of the following pieces:

- Text segment, consisting of the machine instructions that the CPU executes. Usually, the text segment is sharable so that only a single copy needs to be in memory for frequently executed programs, such as text editors, the C compiler, the shells, and so on. Also, the text segment is often read-only, to prevent a program from accidentally modifying its instructions.
- Initialized data segment, usually called simply the data segment, containing variables that are specifically initialized in the program. Foe example, the C declaration
  ```C
  int maxcount = 99;
  ```
  appearing outside any function causes this variable to be stored in the initialized data segment with its initial value.
- Uninitialized data segment, often called the "bss" segment, named after an acient assembler operator that stood for "block started by symbol". Data in this segment is initialized by the kernel to arithmetic 0or null pointers before the program starts executing. The C declaration
  ```c
  long sum[100];
  ```
  appearing outside any function causes this variable to be stored in uninitialized data segment.
- Stack, where automatic variables are stored, along with information that is saved each time a function is called. Each time a function is called, the address of where to return to and certain information about the caller's environment, such as some of the machine registers, are saved on the stack. The newly called function then allocates room on the stack for its automatic and temporary variables. This is how recursive functions in C can work. Each time a recursive function calls itself, a new stack frame is used, so one set of variables doesn't interfere with variables from another instance of the function.
- Heap, where dynamic memory allocation usually takes place. Historically, the heap has been located between the unintialized data and the stack.

> The contents of the uninitialized data segment are not stored in the program file on disk, because the kernel sets the contents to 0 before the program staarts running. The only portions of the program that need to be saved in the program file are the text segment and the initialized data.


```shell
# The size command reports the sizes(in bytes) of the text, data, and bss segment.
$ size /usr/bin/cc
  text    data    bss     dec     hex     filename
346919    3576   6680 3517175   57337     /usr/bin/cc
```

## Shared Libraries

Shared libraries remove the common library routines from the executable file, instead maintaining a single copy of the library routine somewhere in memory that all processes reference. This reduce the size of each executable file but may add some runtime overhead, either when the program is first executed or the first time each shared library function is called. Another advantage of the shared library is that library functions can be replaced with new versions without having to relink edit every program that uses the library(assuming that the number and type of arguments haven't changed).

> 不同的系统提供不同的方法去决定程序是否使用shared library

```shell
$ gcc -static hello1.c      # prevent gcc fro using shared libraries.

$ gcc hello1.c          # compile this progra to use shared libraries, the text and data size of the executable file are greatly decreased.

```

## Memory Allocation

ISO C provides three functions for memory allocation:

```C
#include <stdlib.h>

// allocate a specified number of bytes of memory. 
// The initial value of the memory is indeterminate
void *malloc(size_t size);

// allocates space for a specified number of objects of a specified size.
// The soace is initialized to all 0 bits.
void *calloc(size_t nobj, size_t size);

// increase or decrease the size of a previously allocated area.
// When the size increases, it may ivolve moving the prevously allocated area somewhere else, to provide the additional room at the end.
// (Also, the initial value of the space between the old contents and the end of the new area is indeterminate.)
// If ptr is a null pointer, reallocate behaves like malloc annd allocates a region o the specified newsize.
void realloc(void *ptr, size_t newsize);

// The free function causes the space pointed to by ptr to be deallocated. 
// This freed space is usually put into a pool of availbale memory and can be allocated in a later call to one of the three alloc functions.
// 释放的空间不会返回给kernel，而实留在保留在malloc pool中
void free(void *ptr);
```

> The allocation routines are usually implemented with the sbrk system call.
> Most implementations allocate more space than requested and use additional space for record keeping -- the size of the block, a poiter to the next allocated block, and the like. As a consequence, writing past the end or before the start of an allocated area could overwrite this record-keeping information in another block. These types of errors are often catastrophic, but difficult to find, because the error may not show up until much later.


alternative memory allocators

```C
libmalloc

vmalloc

quick-fit

jemalloc

TCMalloc

alloca Function
```


## 其他函数

### setjmp and longjmp Functions

In C, we can't goto a label that's in another function. Instead, we must use the setjmp and longjmp functions to perform this type of branching.

> 对于处理出现在很深的嵌套中的error，很有帮助


```C
#include <setjmp.h>
// We call setjmp from the location that we want to return to, which in this example is in the main function. In this case, setjmp returns 0 because we called it directly.
// Returns: 0 if called directly, nonzero if returning from a call to longjmp
int setjmp(jmp_buf env);

//
void longjmp(jmp_buf env, int val);
```

If you  have an automatic variable that you don't want rolled back, define it with the **volatile** attribute.

variables:

- automatic variables: an automatic variable can never be referenced after the function that declared it returns
- global
- register
- static
- volatile


### getrlimit and setrlimit Functions

Each process has a set of resource limits, some of which can be queried and changed by the getrlimit and setrlimit functions.

```C
#include <sys/resource.h>

int getrlimit(int resource, struct rlimit *rlptr);

int setrlimit(int resource, const struct rlimit *rlptr);

struct rlimit { 
    rlim_t rlim_cur;        // soft limit: current limit
    rlim_t rlim_max;        // hard limit: maximum value for rlim_cur
}
```

Three rules govern the changing of the resource limits.

- A process can cahnge its soft limit to a value less than or equal to its hard limit.
- A process can lower its hard limit to a value greater than or equal to its soft limit. This lowering of the hard limit is irreversible for normal users.
- Only a superuser process can raise a hard limit.

Limits are defined by the Single UNIX Specification and supported by each implementation.

| Limits | Description |
| ------ | ----------- |
| RLIMIT_AS | |
| RLIMIT_CORE | |
| RLIMIT_CPU | |
| RLIMIT_DATA | |
| RLIMIT_FSIZE | |
| RLIMIT_MEMLOCK | |
| RLIMIT_MSGQUEUE | |
| RLIMIT_NICE | |
| RLIMIT_NOFILE | |
| RLIMIT_NPROC | |
| RLIMIT_NPTS | |
| RLIMIT_RSS | |
| RLIMIT_SBSIZE  | |
| RLIMIT_SIGPENDING | |
| RLIMIT_STACK | |
| RLIMIT_SWAP | |
| RLIMIT_VMEN | |

## Process Control

contents：

- creation of new process
- program execution
- process termination

### Process Identifiers

Process ID 0 is usually the scheduler process and is often known as the swapper, which is part of kernel and is known as a system process.
Process ID 1 is usually the init process and is invoked by the kernel at the end of the bootstrap procedure.(程序文件位置：老版本UNIX在/etc/init,新版本在/sbin/init) 

> In Mac OS X, the init process was replaced with the launched process, which performs the same set of tasks as init, but has expanded functionality.
> Each UNIX System implementation has its own set of kernel processes that provide operating system services.

```C
#include <unistd.h>

// Returns: process ID of calling process
pid_t getpid(void);

// Returns: parent process ID of calling process
pid_t getppid(void);

// Returns: real user ID of calling process
uid_t getuid(void);

// Returns: effective user ID of calling process
uid_t geteuid(void);

// Returns: real group ID of calling process
gid_t getgid(void);

// Returns: effective group ID of calling process
gid_t getegid(void);
```

> None of these function has an error return.

```C
#include <unistd.h>

// An existing process can create a new one by calling the fork function. The new process created by fork is called the child process.
// This function is called once but returns twice.(Because a process can have more than one child, and there is no function that allows)
// Returns: 0 in child, process ID of child in parent, -1 on error
pid_t fork(void);
```

Both the parent and the child continue executing with the instruction that follows the call to fork. The child is a copy of the parent. For example, the child gets a copy of the parent's data space, heap, and stack. The parent and the child do not share these portions of memory. The child and the parent do share the text segment.

Modern implementations don't perform a complete copy of the parent's data, stack and heap, since a fork is often followed by an exec.

Instead, a technique called copy-on-write(COW) is used. Thses regions are shared by the parent and the child and have their protection changed by the kernel to read-only. If either process tries to modify these regions, the kernel then makes a copy of the piece of memory only, typically a "page" in a virtual memory system.

We don;t know whether the child starts executing before the parent, or vice versa. The order depends on the scheduling algorithm used by the kernel.

If it's required that the child and parent synchronize their actions, some form of interprocess communication(IPC) is required.

```C
// 函数调用，计算一个字符串的长度，不包括'/0'
strlen()

// 编译时计算，计算buffer的长度，不包括'/0'(因为buffer)
sizeof()
```

one characteristic of fork is that all file descriptors that are open in the parent are duplicated in the child. The parent and the child share a file table entry for every open descriptor.

## Threads

We can use multiple threads of control(simply threads) to perform multiple tasks within the environment of a single process. All threads within a single process have access to the same process components, such as file descriptors and memory.


synchronizatio mechanisms ==> consistency

### Thread Identification

The process ID is unique in the system.(pid_t data type)
The thread ID has dignificance only within the context of the process to which it belongs.(pthread_t data type)


```C
#include <pthread.h>

// Returns: nonzero if equal, 0 otherwise.
int pthread_equal(pthread_t tid1, pthread_t tid2);

// Returns: thr thread ID of the calling thread
// A thread can obtain its own thread ID by calling the function
pthread_t pthread_self(void); 
```

### Thread Creation

```C
#include <pthread.h>

// Returns: 0 if OK, error number on failure
int pthread_create(pthread_t *restrict tidp, const pthread_attr_t *restrict attr, void *(*start_rtn)(void *), void *restrict arg);
```

## Interprocess Communication

classical IPC:
- pipes
- FIFOs
- meeage queues
- semaphores
- shared memory

Network IPC using the sockets mechanism

### Pipes

> the oldest form of UNIX System IPC.

Pipes have two limitations:
- Historically, they have been half duplex(i.e., data flows in only one direction). Some systems now provide full-duplex pipes, but for maximum portablility, we should never assume that this is the case.
- Pipe can be used only between processes that have a common ancestor. Normally, a pipe is created by a process, that process calls fork, and the pipe is used between the parent and the child.

> Every time you type a sequence of commands in a pipeline for the shell to execute, the shell create a seperate process for each command and links the standard output of one process to the standard input of the next using a pipe.


```C
#include <unistd.h>

// Create a pipe.
int pipe(int fd[2]);
```

> Normally, the process that calls pipe then calls fork, creating an IPC channel form the parent to the child, or vice versa.


### FIFOs

With FIFOs, unrelated processes can exchange data.

> FIFO is a type of file.

```C
#include <sys/stat.h>

int mkfifo(const char *path, mode_t mode);

int mkdidoat(int fd, const char *path, mode_t mdoe);
```


### XSI IPC


### Network IPC: Sockets





