---
title: MATLAB GUI
date: 2020-03-13 14:25:34
tags: 
- MATLAB
- GUI
categories:
- [MATLAB]
- [GUI]
toc: true
---
本文介绍使用MATLAB进行GUI设计的过程
<!--more-->
## Matlab GUI

每一个图形都有一个唯一的句柄Handle和定义图形图像外观的属性Properties
Key：句柄图形（通过属性来定义行为和外观）

每一个组件都与一个或多个用户编写好的程序作为回调函数相关联；
每一个回调函数的执行都是由用户的一个行为所触发；

GUI编程：事件驱动型编程（回调函数的执行受事件外部软件控制而不同步）

计算机窗口系统（GUI）

- 菜单——uimenu对象
  用户选择命令和选项
  下拉式菜单；可嵌套
  菜单属性

- 控制框——uicontrol对象
  用户操作或设置选项
  按钮键
  无线按钮
  复选框
  静态文本框
  可编辑文本框
  滚动条
  弹出式菜单
  框架（提供视觉分隔性）
  控制框属性

- 对话框
  dialog函数
  本身不是句柄图形对象，而是由一系列句柄图形对象构成的M文件
  对话窗口是图形，包括与框架、编辑、按钮uicongtrol对象共存的坐标轴
  公共对话框
  专用对话框

## 图形界面设计工具——GUIDE

- 布局编辑器Layout Editor
- 对象位置调整工具Align Objects
- 菜单编辑器Menu Editor
- Tab顺序编辑器Tab Order Editor
- M-file编辑器M-file Editor
- 对象属性编辑器Property Inspector
- 对象浏览器Property Browser
