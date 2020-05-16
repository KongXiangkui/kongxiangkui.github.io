---
title: Python库 requests
date: 2020-03-29 11:16:57
tags: 
- Python
- 网络
- requests
categories:
- [Python, 第三方库]
- [网络]
toc: true
---
本文介绍Python第三方库requests的相关内容
<!--more-->
## 基础

requests基于 urllib，采用 Apache2 Licensed 开源协议的 HTTP 库。它比 urllib 更加方便，可以节约我们大量的工作，完全满足 HTTP 测试需求。

```shell
# 安装
pip install requests
```

## 使用

```Python
import requests 
r = requests.get('')
print('文字编码: ', r.encoding)
print('响应状态码: ', r.status_code)
print('字符串方式的响应体: ', r.text)
```

