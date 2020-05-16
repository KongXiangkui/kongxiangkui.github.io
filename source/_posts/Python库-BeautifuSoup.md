---
title: Python库 BeautifuSoup
date: 2020-04-06 14:20:24
tags:
- Python
- 网络
categories:
- [Python, 第三方库]
toc: true
---
[BeautifulSoup4](https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/) 可以从HTML或XML文件中提取数据的Python库.它能够通过你喜欢的转换器实现惯用的文档导航,查找,修改文档的方式
<!-- more -->
## 安装

```shell
# 安装bs4
pip install beautifulsoup4

# 安装解析器
pip install lxml
pip install html5lib
```

> 解析器：BeautifulSoup4 支持Python标准库中的HTML解析器,还支持一些第三方的解析器(lxml、纯Python实现的 html5lib) 

- Python内置的解析器，速度较慢，容错能力强
- lxml 支持HTML、XML解析，速度快，容错能力强，需要安装C语言库
- html5lib 容错性最好，以浏览器的方式解析文档，生成HTML5格式的文档，速度慢，不依赖外部扩展

## 使用

1. 文档被转换成Unicode,并且HTML的实例都被转换成Unicode编码
2. Beautiful Soup选择最合适的解析器来解析这段文档,如果手动指定解析器那么Beautiful Soup会选择指定的解析器来解析文档.

Beautiful Soup将复杂HTML文档转换成一个复杂的树形结构,每个节点都是Python对象,所有对象可以归纳为4种:
- tag对象：与XML或HTML原生文档中的tag相同(最重要的属性：name和attributes)
- NavigableString对象：用 NavigableString 类来包装tag中的字符串
- BeautifulSoup对象：表示的是一个文档的全部内容.大部分时候,可以把它当作 Tag 对象
- Comment对象：一个特殊类型的 NavigableString 对象:

