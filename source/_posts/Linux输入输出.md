---
title: Linux Standard Input/Output Library
date: 2020-04-10 16:51:53
tags:
- Linux
- C
categories:
- [Linux]
- [C]
toc: true
---
本文介绍Linux Standard I/O Library，这是ISO C 的标准库, 是跨平台的（在多个操作系统上实现），一些对于标准库的扩展属于The Single UNIX Specification。
在[Linux文件](./Linux文件.md)中，是以File Descriptor为中心，在Standard I/O Library中，是以stream为中心的。
<!-- more -->
> Standard I/O tern strem 和 STREAM SI/O system不同，后者是System V的一部分，属于The Single UNIX Spcecification中XSI STREAMS option.(即将过时)


- open() 
- read()
- write()
- lseek()
- close()

POSIX constant:
- STDIN_FILENO, STDOUT_FILENO: 定义在<unistd.h>头文件中,说明file description for standard input and standard output 

standard I/O function
- fgets(): read an entire line.
- read(): read a specified number of bytes.
- printf()


## 概念

当我们使用Standard I/O Library打开一个文件时，我们已经将一个stream与这个文件关联。

With the ASCII character set, a single character is repressented by a single byte.
With international character sets, a character can be represented by more than one byte.

> Standard I/O file streams can be used with both single-byte and multibyte("wide") character sets.

A stream's orientation determines whether the characters that are read and written are single byte or multibyte.(在stream被创建时是没有方向的)

```C
#include <stdio.h>
#include <wchar.h>

// Returns: positive if stream is wide oriented, negative if stream is byte oriented, or 0 if stream has no orientation
int fwid(FIFE *fp, int mode);
```

当我们使用标准库函数fopen() 打开一个streaml, 会返回一个FILE对象。这个对象包含了所有标准I/O库所要求的对象，以实现对stream的管理。
- the file descriptor used for actual I/O
- a pointer to a buffer for the stream
- the size of the buffer
- a count of the number of characters currently in the buffer
- an error flag
- ...

To reference the stream, we pass its FILE pointer as an argument to each standard I/O function. We will reser to a pointer to a FILE object, the type FILE *, as a file pointer.

Three streams, predefined and automatically available to a process, refer to the same file as the file descriptor:
| Streams | File Descripter |
| ------- | --------------- |
| Standard Input | STDIN_FILENO |
| Standard Output | STDOUT_FILENO |
| Standard Error | STDERR_FILENO |

Three types of buffering are provided:
- Fully buffered: In this case, actual I/O takes place when the standard I/O buffer is filled.
- Line buffered: In this case, the standard I/O library performs I/O when a newline character is encountered on input or output.
- Unbuffered: The standard I/O library does not buffer the characters.

ISO C requires the following buffering characteristics:
- Standard input aand standard output are fully buffered, if and onlu they do not refer to an interactive device.
- Standard error is never fully buffered.

Most implementations default to the following types of buffering:
- Standard error is always unbuffered.
- All other streams are line buffered if they refer to a terminal device; otherwise, they are fully buffered.

```C
#include <stdio.h>

// change the buffering by calling either the setbuf or seetvbuf function.
// These function must be called after the stream has been opened(obviously, since each requires a valid file pointer as its first argument) but before any other operation is performed on the stream.
void setbuf(FILE *restrict fp, char *restrect buf);

// Returns: 0 if OK, nonzero on error
int setvbuf(FILE *restrict fp, char *restrict buf, int mode, size_t size);
```


In general, we should let the system choose the buffer size and automatically allocate the buffer. When we do this, the standard I/O library automatically releases the buffer when we close the stream.

At any time, we can force a stream to be flushed.
```C
#include <stdio.h>

// The fflush function causes any written data for stream to be passed to the kernel. As a special case, ig fp is NULL, fflush causes all output streams to be flushed.
// Returns: 0 if OK, EOF on error.
int fflush(FILE *fp);
```

### Opening a stream

```C
#include <stdio.h>

// All three return: file pointer if OK, NULL on error.

/* Open a special file */
FILE *fopen(const char *restrict pathname, const char *restrict type);

/* Open a special file on a specified stream, closing the stream first if it is already open. If the stream previously had an orientation, fopen clears if. 
 * This function is typically used to open a special file as one of the predefined streams: standard input, standard output, or standard error.
 */
FILE *freopen(const char *restrict pathname, const char *restrict type, FILE *restrict fp);

/* Take an existing file descriptor, which we could obtain fro the open, dup, dup2, fcntl, pipe, socket, socketpair, or accept functions, and associates a standard I/O stream with descriptor.
 */
FILE *fdopen(int fd, const char *type);
```

```C
#include <stdio.h>

// Returns: 0 if OK, EOF on error.
int fclose(FILE *fp);
```

> Any buffered output data is flushed before the file is closed. Any input daya that may be buffered is discard. If the standard I/O library had autpmatically allocated a buffer for the stream, that buffer is released.

> When a process terminates normally, either by calling the exist function directly or by returning from the main function, all standard I/O streams with unwritten buffered data are flushed and all open standard and all open standard I/O stream are closed.


### Reading and Writing a stream

Formatted I/O functions:

- printf()
- scanf()

Three types of unformatted I/O:

- Character-at-a-time: We can read or write one character at a time, with the standard I/O functions handling all the buffering, if the stream is buffered.
- Line-at-a-time: If we want to read or write a line at a time, we use fgets and fputs. Each line is terminated with a newline character, and we have to specify the maximum line length that we can handle when we call fgets.
- Direct I/O: The type of I/O is supported by the fread and fwrite functions. For each I/O operation, we read or write some number of objects, where each object is of a specified size. The two functions are ofthen used for binary files where we read or write a structure with each operation.

```C
#include <stdio.h>

// All three return: next character if OK, EOF on the end of file or error
// The three functions return the next character as an unsigned char converted to an int.

// getc() is implemented as a macro. => The argument to getc should not be an expression with side effects, because it could be evaluated more than once.
int getc(FILE *fp);


// Since fgetc is guaranteed to be a function, we can take its addresss. This allows us to pass the address of fgetc as an argument to another function.
// Call to fgetc probably take longer than calls to getc, as it usually takes more time to call a function.
int fgetc(FILE *fp);

// the function getchar is equivalent to getc(stdin).
int getchar(void);
```

> The constant EOF in header(stdio.h) is required to be a nagetive value. Its value is often -1.

```C
#include <stdio.h>

// Both return: nonzero(true) if condition is true, 0(false) otherwise.
int ferror(FILE *fp);

int feof(FILE *fp);

void clearerr(FILE *fp);
```

```C
#include <stdio.h>

// After reading from a stream, we can push acj characters by calling ungetc.
// Returns: c if OK, EOF on error.
int ungetc(int c, FILE *fp);
```

### Output Functions

```C
#incldue <stdio.h>

// The function is implemented as a marco.
int putc(int c, FILE *fp);

int fputc(int c, FILE *fp);

// The function putchar is equivalent to putc(c, stdout).
int putchar(int c);
```

### Line-at-a-Time I/O

```C
#include <stdio.h>

// Both return: buf if OK, NULL on the end of file or error.

// read from the specified stream
// we have to specify the size of the buffer, n. This function reads up through and including the next newline, but no more than n-1 characters, into buffer. The buffer is terminated with a null byte. If the line, inlcuding the terminating newline, is konger than n-1, only a partial line is returned, but the buffer is always null terminated.
char *fgets(char *restrict buf, int n, FILE *restrict fp);

// read from standard input
// The gets function should never be used. The problem is that it doesn't allow the caller to specify the buffer size, This allow the buffer to overflow if the line is longer than the buffer, writing over whatever happens to follow the buffer  in memory. In addition, gets fnction doesn't store the newline in the buffer, as fgets does.
char *gets(char *buf);
```

> gets funtion is marked as an obsolescent interface in SUSv4 and has been omitted from the latest version of the ISO C standard(ISO/IEC 9989:2011)


```C
#include <stdio.h>

// Both return: non-negative value if OK, EOF on error.

// Write the null-terminated string to the specified stream. The null byte at the end is not written.
// Usually, this is the case -- the last non-null character is a newline -- but it's not required.
int fputs(const char *restrict str, FILE *restrict fp);


// Write the null-terminated string to the standard output, without writing the null byte. But puts then writes a newline character to the standard output.
int puts(const char *str);
```

> 尽量避免使用gets() 和 puts()，使用fgets() 和 fputs()，我们知道我们总是要处理每一行的newline字符。

## Standard I/O Efficiency

## Binary I/O

If we are doning binary I/O, we often would like to read or write an entire structure at a time. fputs and fgets won't work correctly on input if any of data bytes are nulls or newlines.

```C
#include <stdio.h>

// Both retrn: number of objects read or written.

size_t fread(void *restrict ptr, size_t size, size_t nobj, FILE *restrict fp);

size_t fwrite(const void *restrict ptr, size_t size, size_t nobj, FILE *restrict fp);

/* Example */
// Read or write a binary array.
float data[10];

if (fwrite(&data[2], sizeof(float), 4 fp) != 4)
    err_sys("fwrite error");


/* Example */
// Read or write a structure.
strcut {
    short count;
    long total;
    char name[NAMESIZE];
} items;

if (fwrite(&item, sizeof(item), 1, fp) != 1)
    err_sys("fwrite error");
```

> Because norm today is to have heterogeneous systems connected together with networks. It is common to want to write data on one system and process it on another. These two functions won't work, for two reasons:
> - The offset of a member within a structure can differ between compilers and systems because of different alignment requirement. Indeed, some compilers have an option allowing structures to be packed tightly, to save space with a possible runtime performance penalty, or aligned accurately, to optimize runtime access of each member. This means that even on a single system, the binary layout of a structure can differ, depending on compiler options.
> - The binary formats used to store multibyte integers and floating-point values differ among machine architectures.
> The real solytio for exchanging binary data among different systems is to use an agreed-ipon canonical format.


## Positioning a Stream

Three ways to position a standard I/O stream:

- The two functions ftell and fseek.(自打Version7这两个函数就存在了，他们默认文件的位置可以被存储在一个long整型中)
- The two functions ftello and fseeko.(The Single UNIX Specification, 允许file offset超过一个long整型，用off_t取代了long整型)
- The two functions fgetpos and fsetpos.(ISO C, 使用一个抽象的数据类型Abstract Data Type——fpos_t记录 a file's position)=>当移植程序到非UNIX系统时，使用此方案。

```C
#include <stdio.h>

// Returns: current file position indicator if OK, -1L on error.
long ftell(FILE *fp);

// Returns: 0 if OK, -1 on error.
int fseek(FILE *fp, long offset, int whence);


// 功能是将文件内部的指针重新指向一个流的开头
void rewind(FILE *fp);
```

```C
#include <stdio.h>

// Returns: current file position indicator if OK, (off_t)-1 on error
off_t ftello(FILE *fp);

// Returns: 0 if OK, -1 on error
int fseeko(FILE *fp, off_t offset, int whence);
```

```C
#include <stdio.h>

// Both return: 0 if OK, nonzero on error

// The fgetpos function stores the current value of the file's position indicator in the object pointed to by pos.
int fgetpos(FILE *restrict fp, fpos_t *restrict pos);

int fsetpos(FILE *fp, const fpos_t *pos);
```

## Formatted I/O

**注意文件描述符File Descriptor和File Pointer的转化**

### Formatted Output

```C
#include <stdio.h>

// the printf family
// All three return: number of characters output if Ok, negative value if output error.
int printf(cosnt char *restrict format, ... );

int fprintf(FILE *restrict fp, const char *restrict format, ... );

int dprintf(int fd, const char *restrict format, ... );

// Returns: number of characters stored in array if OK, negative value if encoding error
int sprintf(char *restrict buf, cosnt char *restrict format, ... );

// Returns: number of characters that would have been stored in array if buffer was large enoufh, negative value if encoding error.
int snprintf(char *restrict buf, size_t n, const char *restrict format, ... );
```

The format specification controls how the remainder of the arguments will be encoded and ultimately displayed. Each argument is encoded according to a conversion specification that starts with a percent sign(%). Expect for the conversion specifications, other characters in the format are copied unmodified. 
A conversion specification has four optional components:
```C
/* 格式说明符
 * Flag见下表
 * fldwidth: sppecify a minimum field width for the conversion.
 * precision: specify the minimum number of the digits to appear for integer conversions; the minimum number of digits to appear to the right of the decimal point for floating-point conversions; the maximum number of bytes for string conversions. The precision is a period(.) followed by a optional non-negative decimal integer or an asterisk.
 * lenmodifier(length modifier) 见下表
 * convertype: control how the argument is interpreted.详见下表
 */
%[falgs][fldwidth][precision][lenmodifier]convtype
```

| Flags | Description |
| ----- | ----------- |
| ' | (apostrophe) format integer with thousands grouping characters |
| - | left-justify the output in the field |
| + | always display sign og a signed conversion |
| (space) | prefix by a space if no sign is generated |
| # | conver using alternative form (include 0x prefic for hexadecimak format, for example)|
| 0 | prefic with leading zeros instead of padding with spaces |


| Length Modifier | Description |
| --------------- | ----------- |
| hh | signed or unsigned char |
| h | signed or unsigned short |
| l | signed or unsigned long or wide character |
| ll | signed or unsigned long long |
| j | intmax_t or uintmax_t |
| z | size_t |
| t | ptrdiff_t |
|L | long double |


| Conversion Types | Description |
| ---------------- | ----------- |
| d,i | signed decimal |
| o | unsigned octal |
| u | unsigned decimal | 
| x,X | unsigned hexadecimal |
| f,F | double floating-point number |
| e,E | double floating-point number in exponential format |
| g,G | interpreted as f, F, e, or E depending on value converted |
| a,A | double floating-point number in hexadecimal exponential format |
| c | character(with 1 length modifier, wide character) |
| s | string(with 1 length modifier, wide character string) | 
| p | pointer to a void |
| n | pointer to a signed integer into which is written the number of characters written so far |
| % | a % character |
| C | wide character (XSI option, equivalent to lc) |
| S | wide character string (XSI option, equivalent to ls) |

文件描述符按照参数出现的顺序去转化，也可以使用 %n$ 代表第n个参数

```C
#include <stdarg.h>
#include <stdio.h>

// Five variants of the printf family
int vprintf()

int vfprintf()

int vdprintf()

int vspprintf()

int vsnprintf()
```

### Formatted Input

```C
#include <stdio.h>

// The scanf family is used to parse an input string and convert character sequences into variables of specified types.

int scanf(const char *restrict format, .... );
int fscanf(FILE *restrict fp, const char *restrict format, ... );
int sscanf(const char *restrict buf, const char *restrict format, ... );
```

```C
/* 
 *three optional components to a conversion specification
 */
%[*][fldwidth][m][lenmodifier]convertype
````

```C
#include <stdarg.h>
#include <stdio.h>

int vscanf()

int vfscanf()

int vsscanf()
```

FILE object 

获得stream关联的file descriptor

```C
#include <stdio.h>

// Every standard I/O stream has an associated file descriptor, we use the fileno function to obtain the descripyor for a stream.(we need the function if we want to call  the dup or fcntl functions)
// Returns: the file descriptor associated with the sream
int fileno(FILE *fp);
```

The implementations of the standard I/O library vary.

## Temporary Files

```C
#include <stdio.h>
// The tmpnam function generate a string that is a valid pathname and that dows not match the name of anyexisting file.
// This function generates a different pathname each time it is called, up to TMP_MAX times.(TMP_MAX is defined in <stdio.h>)
// Th tmpnam function is marjed obsolescent in SUSv4, but the ISO C standard continues to support it.
// Returns: pointer to unique pathname
char *tmpnam(char *ptr);

// The tmpfile function creates a temporary binary file(type wb+) that is automatically removed when it is closed or on proogram termination.
// Returns: file pointer if OK, NULL on error
FILE *tmpfile(void);
```

The Single UNIX Specification defines two additional functions as part of the X part of the XSI option for dealing with temporary files

```C
#include <stdlib.h>

// The mkdtemp function creates a directory with a unique name.
// The mkstemp function creates a regular file with a unique name.
// The name is selected using the template string. The string is a pathname whose last six characters are set to XXXXXX.

// Returns: pointer to directory name if OK, NULL on error.
char *mkdtemp(cahr *template);

// Unlike tmpfile, the temporary file created by mkstemp is not removed automatically for us. If we want to remove it from the file system namespace, we need to unlink it ourselves.
// Returns: file descriptot if OK, -1 on error.
int mkstemp(char *template);
```

> Older versions of the Single UNIX Specification defined the tempnam function as a way to create a temporary file in a caller-specified location. It is marked obsolecent in SUSv4.

> Use of tmpnam and tempnam does have at least one drawback: a window exists between the time that the unique pathname is returned and the time that am application creates a file with that name. During this timing window, another process can create a file of the same name. The tmpfile and mkstemp functions should be used instead, as they don;t suffer from this problem.


## Memory Streaams

```C
#include <stdio.h>

// Create memory streams.(To provide a buffer to be used for the memory stream.)
// Returns: stream pointer if OK, NULL on error.
FILE *fmemopen(void *restrict buf, size_t size, const char *restrict type);
```

```C
// Both return: stream pointer if Ok, NULL on error

#include <stdio.h>

// Create a stream that is byte oriented
FILE *open_memstream(char **bufp, size_t *sizep);

#include <wchar.h>
// Create a stream that is wide oriented.
FILE *open_wmemstream(wchar_t **bufp, size_t *sizep);
```



## Alternatives to Standard I/O

standard I/O is not perfect--some in the basic design, but most in the various implementations.
