---
title: Python文件JSON
date: 2020-03-31 14:52:40
tags:
- Python
- JSON
categories:
- [Python, 语言]
toc: true
---
JSON, JavaScript Object Notation
- 格式化数据，便于阅读
- JavaScript程序写数据结构的原生方法
<!-- more -->
## Python json module

只能存储strings, integers, floats, Booleans, lists, dictionaries, NoneType

json.loads()
翻译 JSON Data 为 Python value

> loads() 代表load string

```Python
>>> stringOfJsonData = '{"name": "Zophie", "isCat": true, "miceCaught": 0, "felineIQ": null}'       # JSON 总是双引号
>>> import json
>>> jsonDataAsPythonValue = json.loads(stringOfJsonData)    # 返回值 dictionary(无序的)
>>> jsonDataAsPythonValue
{'name': 'Zophie', 'isCat': True, 'miceCaught': 0, 'felineIQ': None}
```

json.dumps()
翻译 Python value 为 JSON Data
> dumps() 代表dump string
> dump sth （计算机）表示为内存信息转存、转储

```Python
>>> pythonValue = {'name': 'Zophie', 'isCat': True, 'miceCaught': 0, 'felineIQ': None}     # JSON 总是双引号
>>> import json
>>> stringOfJsonData = json.dumps(pythonValue)    
>>> stringOfJsonData
'{"name": "Zophie", "isCat": true, "miceCaught": 0, "felineIQ": null}'
```