---
title: Python编码规范
date: 2020-03-23 09:49:25
tags: 
- Python
- 编码规范
categories:
- [Python, 开发]
toc: true
---
未完待续
<!--more-->
## 标识符命名

命名规则
- 标识符是由字符（A ~ Z 和 a ~ z）、下划线和数字组成，但第一个字符不能是数字。
- 标识符不能和 Python 中的保留字相同。有关保留字，后续章节会详细介绍。
- Python中的标识符中，不能包含空格、@、% 以及 $ 等特殊字符

> 标识符中的字母是严格区分大小写的
> Python 允许使用汉字作为标识符

下划线的隐含意义
- 以单下划线开头的标识符（如 _width），表示不能直接访问的类属性，其无法通过 from...import* 的方式导入；
- 以双下划线开头的标识符（如__add）表示类的私有成员；
- 以双下划线作为开头和结尾的标识符（如 __init__），是专用标识符。

规范：
- 当标识符用作模块名时，应尽量短小，并且全部使用小写字母，可以使用下划线分割多个字母，例如 game_mian、game_register 等。
- 当标识符用作包的名称时，应尽量短小，也全部使用小写字母，不推荐使用下划线，例如 com.mr、com.mr.book 等。
- 当标识符用作类名时，应采用单词首字母大写的形式。例如，定义一个图书类，可以命名为 Book。
- 模块内部的类名，可以采用 "下划线+首字母大写" 的形式，如 _Book;
- 函数名、类中的属性名和方法名，应全部使用小写字母，多个单词之间可以用下划线分割；
- 常量命名应全部使用大写字母，单词之间可以用下划线分割；

## PEP8

风格指南是关于一致性的。风格指南的一致性是重要的，项目的一致性更为重要，一个Module或者function的一致性最为重要。

### Code Lay-out代码布局

#### 缩进indentation

每一级缩进采用4个空格(Tab的使用:替换为4个空格)

Continuation lines should align wrapped elements either vertically using Python's implicit line joining inside parentheses, brackets and braces, or using a hanging indent [7]. When using a hanging indent the following should be considered; there should be no arguments on the first line and further indentation should be used to clearly distinguish itself as a continuation line.

Yes:

```python
# Aligned with opening delimiter.
foo = long_function_name(var_one, var_two,
                         var_three, var_four)

# Add 4 spaces (an extra level of indentation) to distinguish arguments from the rest.
def long_function_name(
        var_one, var_two, var_three,
        var_four):
    print(var_one)

# Hanging indents should add a level.
foo = long_function_name(
    var_one, var_two,
    var_three, var_four)
```

No:

```python
# Arguments on first line forbidden when not using vertical alignment.
foo = long_function_name(var_one, var_two,
    var_three, var_four)

# Further indentation required as indentation is not distinguishable.
def long_function_name(
    var_one, var_two, var_three,
    var_four):
    print(var_one)
```

The 4-space rule is optional for continuation lines.

Optional:

```python
# Hanging indents *may* be indented to other than 4 spaces.
foo = long_function_name(
  var_one, var_two,
  var_three, var_four)
```
