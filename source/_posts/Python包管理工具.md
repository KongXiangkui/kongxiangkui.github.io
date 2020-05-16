---
title: Python包管理工具
date: 2020-03-29 10:30:32
tags: 
- Python
- 包管理
categories:
- [Python, 第三方库]
toc: true
---
python的官方包管理工具pip的一些使用
<!--more-->
## pip

```bash
# pip的更新
python -m pip install --upgrade pip

# 安装包
pip install somepackage
# 更新包
pip install --upgrade somepackage
# 卸载包
pip uninstall somepackage

# 列出所有已安装的包
pip list 
# 列出所有需要更新的包
pip list --outdate

# 查看第三方库所依赖的包
pip show package
```