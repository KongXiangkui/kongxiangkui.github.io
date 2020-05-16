---
title: Python文件CSV
date: 2020-03-31 14:50:36
tags:
- Python
- CSV
categories:
- [Python, 语言]
toc: true
---
CSV, Comma-separated values
<!-- more -->
用逗号分隔spreadsheet的每行的每个cell
## CSV文件

## 读文件

Reader object: 可以迭代的操作CSV 文件的每一行
- 变量；line_num

```Python
import csv
# 读取文件
exampleFile = open('example.csv')
# don’t pass a filename string directly to the csv.reader() function. 
exampleReader = csv.reader(exampleFile) #  return a Reader object 
exampleData = list(exampleReader)
print(exampleData)
# list 
exampleFile.close()
```

```Python
import csv
# 读取文件
exampleFile = open('example.csv')
exampleReader = csv.reader(exampleFile) #  return a Reader object 

# avoid loading the entire file into memory at once
for row in exampleReader:
    print('Row #' + str(exampleReader.line_num) + ' ' + str(row))
exampleFile.close()
```
## 写文件

Writer object

```Python
import csv
# 写文件

# On Windows, you’ll also need to pass a blank string for the open() function’s newline keyword argument.  if you forget to set the newline argument, the rows in output.csv will be double-spaced
outputFile = open('output.csv', 'w', newline='')
outputWriter = csv.writer(outputFile)

# 参数为列表
# 返回值为写入文件的字符数 (including newline characters)
# '\0'是 ASCII 码表中的第 0 个字符，英文称为 NUL，中文称为“空字符”。该字符既不能显示，也没有控制功能，输出该字符不会有任何效果，唯一的作用就是作为字符串结束标志。
outputWriter.writerow(['spam', 'egg', 'bacon', 'ham'])      # 20
outputWriter.writerow(['Hello', 'world', '!!!', 'cad'])     # 21
outputWriter.writerow([1, 2, 3.12, 4])                      # 12
outputFile.close()
```

```Python
# want to separate cells with a tab character instead of a comma
# want the rows to be double-spaced
import csv
csvFile = open('example.tsv', 'w',  newline='')     # 使用tab作为分隔符，所以文件后缀名用 tsv
csvWriter = csv.writer(csvFile, delimiter='\t', lineterminator='\n\n)
csvWriter.writerow(['apples', 'oranges', 'grapes'])
csvWriter.writerow(['eggs', 'bacon', 'ham'])
csvWriter.close()
```