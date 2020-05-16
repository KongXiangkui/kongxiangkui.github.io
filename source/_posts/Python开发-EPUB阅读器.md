---
title: Python开发-EPUB阅读器
date: 2020-03-26 21:52:01
tags: 
- Python
categories:
- [Python, 开发]
- [GUI]
toc: true
---
未完待续
<!--more-->
## 目标
1. 可以打开EPUB格式的书
2. 可以朗读
3. 可以统计阅读量
4. 支持笔记、标准，但不允许修改（考虑中）
5. 支持文档标签
6. 支持多种格式PDF等

## 调研
库：
- json
- pypub(待定)

## EPUB文档

EPUB文档式一种基于XML的对开发者有好的格式。一个EPUB就是一个简单的ZIP格式，按照预先定义的方式排列文件。

EPUB档案的目录和文件结构
```
mimetype
META-INF/
   container.xml
OEBPS/
  content.opf
  title.html    (可忽略)
  content.html  (可忽略) 
  stylesheet.css
  toc.ncx 
  images/       (可忽略)
     cover.png
```

### mimetype
位于EPUB项目的根目录
内容：
```
application/epub+Zip
```
> 注意：不包含新行或者回车，且mimetype必须作为ZIP档案中的第一个文件

### META-INF
位于EPUB项目的根目录，其中要有一个文件 container.xml ，EPUB阅读系统首先查看该文件，它指向数字图书元数据的位置
```xml
<?xml version='1.0' encoding='utf-8'?>
<container xmlns="urn:oasis:names:tc:opendocument:xmlns:container" version="1.0">
  <rootfiles>
    <rootfile media-type="application/oebps-package+xml" full-path="OEBPS/content.opf"/>
  </rootfiles>
</container>
```

### OEBPS

#### content.opf
指定了图书中所有 内容的位置，如文本和图像等其他媒体。它还给出了另一个元数据文件，内容的 Navigation Center eXtended (NCX) 表。该 OPF 文件是 EPUB 规范中最复杂的元数据。

#### toc.ncx
内容NCX表
定义了数字图书的目录表，包括嵌套的内容、章节
> 本质还是xml文档

#### 具体的内容

- title.html:图书的标题页，创建该文件并在其中包含引用封面图片的img元素，src属性值为images/cover.png
- images文件夹
- content.html: 图书的实际文字内容
- stylesheet.css: 将该文件放在和 XHTML 文件相同的 OEBPS 目录中。该文件可以包含任意 CSS 声明，比如设置字体或者文字颜色。
