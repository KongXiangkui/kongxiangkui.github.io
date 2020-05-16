---
title: Python爬虫
date: 2020-04-06 11:53:59
tags:
- 网络
- 爬虫
categories:
- [Python, 开发]
toc: true
---
本文使用Python进行爬虫
<!-- more -->
## 基础

### 爬虫的合法性


## 爬虫

### 爬虫的流程

1. 获取网页：给网址发送请求，网址会返回整个网页的数据

- bs4
- request
- urllib
- selenium(模拟浏览器)

- 多进程抓取
- 登录抓取
- 突破IP封禁和服务器抓取

2. 解析网页（提取数据）：从整个网页的数据中提取所需要的数据

- re正则表达式
- BeautifulSoup
- xml


3. 存储数据

- 存储到文件：txt、csv
- 存储到数据库：MySQL、MongoDB 

### 静态网页抓取

静态网页所有的数据都存储在HTML代码中，容易获取


```Python
import requests
from bs4 import BeautifulSoup   

link = 'https://www.baidu.com/'

# 伪装成浏览器访问
headers = {'User-Agent': 'Mozilla/5.0 (Windows; U; Windows NT 6.1; en-US; rv:1.9.1.6) Gecko/20191201 Firefox/3.5.6'}

# 返回requests的Response回复对象
r = requests.get(link, headers=headers)

# r.text为获取的网页内容代码
print(r.text)

soup = BeautifulSoup(r.text, 'lxml')    # 使用BeautifulSoup解析这段代码
print(soup.find("h2", class_="post-title"))
title = soup.find("h2", class_="post-title").text.strip()
print(title)

with open('title.txt', 'a+') as f:  # 数据存储
    f.write(title)
    f.close()
```

### 动态网页

使用AJAX动态加载网页的数据