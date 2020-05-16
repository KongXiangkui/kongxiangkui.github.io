---
title: Python库 lxml
date: 2020-04-06 14:42:27
tags: 
- Python
- XML
- HTML
categories:
- [Python, 第三方库]
toc: true
---
本文介绍Python的的解析器(解析HTML, XML).
Python 标准库中自带了 xml 模块，但是性能不够好，而且缺乏 API。第三方库 lxml 是用 Cython 实现的，而且增加了很多实用的功能，便于爬虫处理网页数据。
<!-- more -->
## 安装

```shell
pip install lxml
```

## 使用

