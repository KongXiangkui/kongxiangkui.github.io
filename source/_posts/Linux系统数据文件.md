---
title: Linux系统数据文件
date: 2020-04-23 17:29:33
tags: Linux
categories:
- [Linux]
toc: true
---
本文介绍Linux系统数据文件
<!-- more -->

## Password File

The UNIX System's password file, called the user database by POSIX.1, contains the fields:

| Description | Structure Passwd member | 
| ----------- | ----------------------- | 
| user name | char *pw_name |
|encrypted password | char pw_passwd |
| numberical user ID | uid_t pw_uid |
| numerical group ID | gid_t pw_gid |
| comment field | char *pw_gecos |
| initial working directory | cahr *pw_dir |
| initial shell(user program) | char *pw_shell |
| user access class | char *pw_class |
| next time to change password | time_t pw_change |
| account expiration time | time_t pw_expire |


```shell
# Print information about computername
$ finger -p computername
Login:          Name:
Directory:      Shell:
Office:         Home Phone:   
...

# Allow administrators to edit the password file
# The command serializes changes to the password filw and makes sure that any additional files are consistent with the changes made.
$ vipw
```

POSIX.1 provides two functions to fetch entries from the password file. The two functions is fine if we want to look up either a login name or a user ID.

```C
#include <pwd.h>

// Both return: pointer if OK, NULL on error.

// The getpwuid function is used by ls program to map the numerial userID contained in an i-node into a user's login name.
struct passwd *getpwuid(uid_t uid);

// The getpwnam function is used by login program when we enter out login name.
struct passwd *getpwnam(const char *name);
````

Some programs need to go through the entire password file.

```C
#include <pwd.h>

// Returns: Pointer if OK, NULL on error or end of file
// return the entry in the password file. 
struct passwd *gerpwent(void);

// rewind whatever files it uses
void setpwent(void);

// close these files
void endpwent(void);
```

> The three functions are not part of the base POSIX.1 standard. They are defined as part of the XSI option in the Single UNIX Specification.

## Shadow Passwords

```C
#include <shadow.h>

// Both return: pointer if OK, NULL on error.
struct spwd *getspnam(const char *name);

struct spwd *getspent(void);

void setspent(void);
void endspent(void);
```

## Group File

The UNIX system's group file is called the group database by POSIX.1.

| Description | struct group member |
| ----------- | ------------------- |
| group name | char *gr_name |
| encrypted password | char *gr_passwd |
| numerical group ID | int gr_uid |
| array of pointers to individual user names | char **gr_mem |


```C
#include <grp.h>

// Both return: pointer if OK, NULL on error.

struct grooup *getgrgid(gid_t gid);

struct group *getgrnam(const char *name);
```

If we wabt to search the entire group file, we need some additional functions

```C
#include <grp.h>

// read the next entry from the group file, open the file first if is's not already open
struct group *getgrent(void);

// Open the group file if it's not already open, and rewinds it
void setgrent(void);

// Close the group file
void endgrent(void);
```

## Supplementary Group IDs

```C
#include <unistd.h>

// Returns: number of supplementary group IDs if OK, -1 on error
int getgroups(int gidsetsize, gid_t grouplist[]);

#include <grp.h>    // On linux
#include <unistd.h> // On FreeBSD, Mac OS X, and Solaris

int setgroups(int ngroups, ocnst gid_t grouplist[]);

#include <grp.h>    // On Linux and Solaris
#include <unistd.h> // On FreeBSD and Mac OS X

int initgroups(cost char*username, gid_t basegid);
```

## Implementation

On many systems, the user and group databases are implemented using Network Information Service(NIS). This allows administrator to edit a master copy of the databases and distribute them automatically to call servers in an organization

## Other Data Files

Numerous other files are used by UNIX systems in normal day-to-day operation.
The general principle is that every data file has at least three cuntions:

- get function: read the next record, opening the file if necesssary. These function normally return a pointer to a structure. A null pointer is returned when the end of file is reached. Most of the get functions return a pointer to a static structure, so we always have to copy the structure if we want to save it.

- set funtion: open the file, if not alread open, and rewinds the file. We use this function when we know we want to start again at the beginning of the file.
- end function: close the data file. When we are done, we call this function to close all the files.

## Login Accounting

The utmp file: keep track of all the users currently logged in.
The wtmp file: keep track of all logins and logouts.

The who program read the utmp file and printed its contents in a readable form.
Later versions of the UNIX systems provided the last command, which read through the wtmp file and printed selected entries.

## System Indetification

```C
#include <sys/utsname.h>

// Returns: non-negative value if OK, -1 on error
int uname(struct utsname *name);
```

```C
#include <unistd.h>

// Returns: 0 if OK, -1 on error
// The namelen argument specifies the size of the name buffer
int gethostname(char *name, int namelen);
```

## Time and Date

```C
#include <time.h>

// Returns: value of time if OK, -1 on error
time_t time(time *calptr);
```

略-------