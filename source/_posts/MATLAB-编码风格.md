---
title: MATLAB 编程规范
date: 2020-03-12 22:26:07
tags: 
- MATLAB
- 编码规范
categories:
- [MATLAB]
toc: true 
---
本文介绍MATLAB的编码风格和习惯
<!--more-->
1. 变量命名

- Matlab区分大小写

2. 数据结构

- 当值是相同类型并代表某种意义上相同的事情时：数组
- 当值是逻辑相关的但不是相同类型不是相同的事情时：元胞数组或结构体
- 当存储不同长度的字符串时：元胞数组（而不是字符矩阵）
- 当需要遍历值时：元胞数组（而不是结构体）

3. 函数 

- 函数和M文件命名一致
  
  *建议：通常函数名是用小写字母*

4. 函数注释

- 函数名称
- 函数完整的功能描述
- 输入参数的描述
- 输出参数的描述
- 函数中使用的变量的描述
- 编程者姓名和完成日期
- 修订信息

5. 程序结构

- for 循环用于计数循环，while 循环用于条件循环
- 如果希望下使用内置常量i、j，不要使用i、j作为循环变量名
- 只要有可能矢量化代码，就不使用循环
- 尽可能使用循环代替递归
- 写包含很多函数的大程序时，先从主程序脚本开始使用函数桩，调试时再依次填写函数
  
6. 文件I/O
  
- 尝试打开文件，检查确认文件被成功打开
- 是否到文件尾（操作）
- 尝试关闭文件，检查确认文件被成功关闭
  
  ```Matlab
  fid = fopen('filename', 'r');
  if fid == -1
    disp('File open not successful');
  else
    while feof(fid)
        % Read one line into a string variable
        aline = fgetl(fid);
        %···operations
    end
    closeresult = fclose(fid);
    if closeresult == 0
        disp('File close successful')
    else
        disp('File close not successful')
    end
  end
  ```

  *建议：总是适时的关闭已打开的文件*

7. 矢量化编程
  
- 如果想得到多组不同的随机数，为rand函数设置种子
- 当指定矩阵中的元素时，应同时使用行索引和列索引
- 一般不设定任何已知数组的维度，用length函数确定一个向量中的元素数目，size 确定一个矩阵中的行列数目

  ```Matlab
  len = length(vec);
  [r c] = size(vec);
  ```

- 如果可以预先知道大小，预先分配向量和矩阵
- 把多组相关变量存在MAT文件里
- 当函数中只有一个简单的表达式使用匿名函数
- 把相关匿名函数存在MAT文件中
  
