---
title: Python库-pySerial
date: 2020-03-16 12:04:43
tags: 
- Python
- 串口通信
- pyserial
categories:
- [Python, 第三方库]
- [网络]
toc: true
---
本文介绍pyserial的相关操作
<!--more-->
## 简介

## pySerial API

## pySerial Tools

获得串口列表
serial.tools.list_ports.comports(include_links=Fasle)
- Parameters: include_links (bool) – include symlinks under /dev when they point to a serial port
- Returns: a list containing ListPortInfo objects.(no particular order)

使用正则表达式搜索
serial.tools.list_ports.grep(regexp, include_links=False)
- Parameters: regexp(regular expression),include_links(bool)-include symlinks under /dev when they point to a serial port
- return: an iterable that yields ListPortInfo objects, see also comports().

串口信息类
class serial.tools.list_ports.ListPortInfo

- device: /dev/ttyUSB0 (使用index访问，返回的第一个元素)
- name: ttyUSB0
- description: human readable description or n/a (使用index访问，返回的第二个元素)
- hwid: technical description or n/a  (使用index访问，返回的第三个元素)

serial.tools.miniterm: A console application that provides a small terminal application

## URL Handlers

serial_for_url(url)

### rfc2217:// 

Used to connect to RFC 2217 compatible servers. All serial port functions are supported. Implemented by rfc2217.Serial.

Supported options in the URL are:
- ign_set_control does not wait for acknowledges to SET_CONTROL. This option can be used for non compliant servers (i.e. when getting an remote rejected value for option 'control' error when connecting).
- poll_modem: The client issues NOTIFY_MODEMSTATE requests when status lines are read (CTS/DTR/RI/CD). Without this option it relies on the server sending the notifications automatically (that’s what the RFC suggests and most servers do). Enable this option when cts does not work as expected, i.e. for servers that do not send notifications.
- timeout=<value>: Change network timeout (default 3 seconds). This is useful when the server takes a little more time to send its answers. The timeout applies to the initial Telnet / RFC 2271 negotiation as well as changing port settings or control line change commands.
- logging={debug|info|warning|error}: Prints diagnostic messages (not useful for end users). It uses the logging module and a logger called pySerial.rfc2217 so that the application can setup up logging handlers etc. It will call logging.basicConfig() which initializes for output on sys.stderr (if no logging was set up already).

> Warning: The connection is not encrypted and no authentication is supported! Only use it in trusted environments.

```python
import serial

serial.serial_for_url('rfc2217://localhost:7000')
serial.sertial_for_url('rfc2217://localhost:7000?poll_modem')
serial.sertial_for_url('rfc2217://localhost:7000?ign_set_control&timeout=5.5')
```

### socket:// 

The purpose of this connection type is that applications using pySerial can connect to TCP/IP to serial port converters that do not support RFC 2217.

Uses a TCP/IP socket. All serial port settings, control and status lines are ignored. Only data is transmitted and received.

Supported options in the URL are:
- logging={debug|info|warning|error}: Prints diagnostic messages (not useful for end users). It uses the logging module and a logger called pySerial.socket so that the application can setup up logging handlers etc. It will call logging.basicConfig() which initializes for output on sys.stderr (if no logging was set up already).

> Warning: The connection is not encrypted and no authentication is supported! Only use it in trusted environments.

```python
import serial

serial.sertial_for_url('socket://localhost:7777')
```

### loop://

The least useful type. It simulates a loop back connection (RX<->TX RTS<->CTS DTR<->DSR). It could be used to test applications or run the unit tests.

Supported options in the URL are:

- logging={debug|info|warning|error}: Prints diagnostic messages (not useful for end users). It uses the logging module and a logger called pySerial.loop so that the application can setup up logging handlers etc. It will call logging.basicConfig() which initializes for output on sys.stderr (if no logging was set up already).

```python
import serial

serial.sertial_for_url('loop://?logging=debug')
```

### hwgrep://

This type uses serial.tools.list_ports to obtain a list of ports and searches the list for matches by a regexp that follows the slashes (see Pythons re module for detailed syntax information).

Note that options are separated using the character &, this also applies to the first, where URLs usually use ?. This exception is made as the question mark is used in regexp itself.

Depending on the capabilities of the list_ports module on the system, it is possible to search for the description or hardware ID of a device, e.g. USB VID:PID or texts.

Unfortunately, on some systems list_ports only lists a subset of the port names with no additional information. Currently, on Windows and Linux and OSX it should find additional information.

Supported options in the URL are:
- n=N: pick the N’th entry instead of the first
- skip_busy: skip ports that can not be opened, e.g. because they are already in use. This may not work as expected on platforms where the file is not locked automatically (e.g. Posix).

```python
import serial

serial.sertial_for_url('hwgrep://0451:f432 (USB VID:PID)')
```
### spy://

Wrapping the native serial port, this protocol makes it possible to intercept the data received and transmitted as well as the access to the control lines, break and flush commands. It is mainly used to debug applications.

Supported options in the URL are:

- file=FILENAME output to given file or device instead of stderr
- color enable ANSI escape sequences to colorize output
- raw output the read and written data directly (default is to create a hex dump). In this mode, no control line and other commands are logged.
- all also show in_waiting and empty read() calls (hidden by default because of high traffic).

```python
import serial

serial.sertial_for_url('spy://COM54?file=log.txt')    
```

```python
import serial

with serial.serial_for_url('spy:///dev/ttyUSB0?file=test.txt', timeout=1) as s:
    s.dtr = False
    s.write('hello world')
    s.read(20)
    s.dtr = True
    s.write(serial.to_bytes(range(256)))
    s.read(400)
    s.send_break()

with open('test.txt') as f:
    print(f.read())
```

```bash
# on POSIX("" 用于避免在shell中，&打断URL)
python -m serial.tools.miniterm "spy:///dev/ttyUSB0?file=/dev/pts/2&color" 115200
```

### alt://

This handler allows to select alternate implementations of the native serial port.

Currently only the POSIX platform provides alternative implementations.

- PosixPollSerial
    Poll based read implementation. Not all systems support poll properly. However this one has better handling of errors, such as a device disconnecting while it’s in use (e.g. USB-serial unplugged).
- VTIMESerial
    Implement timeout using VTIME/VMIN of TTY device instead of using select. This means that inter character timeout and overall timeout can not be used at the same time. Overall timeout is disabled when inter-character timeout is used. The error handling is degraded. 

```python
import serial

serial.sertial_for_url('alt:///dev/ttyUSB0?class=PosixPollSerial')
serial.sertial_for_url('alt:///dev/ttyUSB0?class=VTIMESerial')
```