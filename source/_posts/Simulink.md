---
title: Simulink
date: 2020-03-13 14:27:34
tags: 
- MATLAB
- Simulink
categories:
- [MATLAB]
toc: true
---
本文介绍Simulink的基本操作和相关的库
<!--more-->

- Simulink是基于数据流的，任何输入必须对应相应的输出；处理动态系统的连续变化
- Stateflow可以建立状态机，也可以进行流程设计；处理动态系统的瞬时变化

1. 建立系统仿真模型，设置模块参数Block Parameters
2. 设置仿真参数Model Configuration Parameters：仿真时间，仿真算法（固定步长或变步长） ，输出模式等
3. 启动仿真并分析仿真结果

## Stateflow

描述 MATLAB算法和 Simulink模型如何对输入信号、事件和基于时间的条件作出反应，设计和开发调度控制、任务调度、故障管理、通信协议、用户界面和混合系统
*Stateflow图 将状态、转移和数据组合起来实现有限状态机，对事件驱动的系统进行建模和仿真*

图形化语言：

- 状态转换图状态转移图
- 流程图
- 状态转换表状态转移表
- 真值表

对组合和时序决策逻辑建模，将其作为 Simulink 模型内的一个模块进行仿真，或作为 MATLAB 中的一个对象加以执行。利用图形动画，您可以在逻辑正在执行时对其进行分析和调试。编辑时和运行的时检查可确保在实现前的设计一致性和完整性。

构建stateflow的步骤

1. 设计与simulink的连接接口
2. 定义states
3. 定义状态动作、变量
4. 定义状态的转换
5. 定义出发事件
6. 模拟系统
7. 诊断整个设计

stateflow的用途

1. 设计控制逻辑：使用状态机、流程图和真值表进行系统逻辑建模
2. 执行和调试图表：直观地显示系统行为以进行分析和调试
3. 为 MATLAB 应用程序开发可重用逻辑：使用 Stateflow 图对象为 MATLAB 应用程序开发可重用逻辑。为包括测试和测量、自主系统、信号处理和通信在内的各种应用设计状态机和时序逻辑。
4. 调度 Simulink 算法：调度在 Simulink 中建模的算法
5. 验证设计和生成代码：根据需求验证您的设计，并生成代码以便在嵌入式系统中予以实现

有限状态机：从一种工作模式（状态）转移到另一种工作模式的动态系统表示

- 作为复杂软件设计过程的高级起点
- 专注于工作模式和从一种模式转至下一种模式所需条件
- 降低复杂度，保持模型的简洁清晰

> 控制系统在很大程度上依赖于状态机来管理复杂逻辑

1. 状态：系统模式的描述

- 激活active：一旦一个状态被激活，这个状态一直处于激活状态，直到退出为止
- 非激活inactive

2. 状态动作类型（可以使用 , 组合各种状态动作类型）

- entry(en)：当状态被激活时，动作在时间步上发生
- during(du)：当状态已激活并且图为转移出该状态时，动作在时间步上发生
- exit(ex):当如转移出状态时，动作在时间步上发生

3. 结果

状态和转移动作是在转台或转移旁编写的指令，用于定义Stateflow图在仿真过程中的行为

转移动作类型——条件和条件动作

[condition]{conditonal_action}

condition 是布尔表达式，用于确定是否发生转移，如果不指定，默认为true
conditonal_action 是判断转移条件为true时执行的指令，条件动作发生在条件后，但在任何exit或entry状态动作之前

1. 转移
2. 动作
3. 动作类型
4. 结果

### 状态

层次：允许有子状态和超状态

只有子状态的上层（超状态）被激活后，下层的子状态才有可能被激活

stateflow状态图既非超状态也非子状态

同一层次里，所有状态之间的关系只有两种：

- 互斥OR：不能同时被激活，不能同时执行（实线框表示）
- 并行AND：状态的执行是独立的，同级的并行状态可以同时处于激活状态

状态图使用层次的目的：

- 将相关的对象组合在一起，构成族群

- 将一些通用的迁移路径或者动作组合成为一个迁移动作或路径

- 缩减代码量，提高可读性和执行效率

stateflow的data dictionary中，对象可以分为3个层次

1. Machine状态机对象
   当前模型中所有状态图的集合（含有stateflow图块的simulink模型）——事件、数据对象、编译目标

2. Chart状态图对象（状态机对象的子对象）——状态、图形函数、图形盒、节点、迁移、注释、数据对象、事件

3. state/function/box可以互相包含、互相嵌套——事件、数据对象、注释、迁移、连接节点

状态命名

状态属性设置

状态动作：

- 初始为非激活状态，事件驱动使其激活 entry（en）动作

- 初始为激活状态，事件驱动使其进入非激活状态 exit（ex）动作

- 初始为激活状态，事件未改变其激活状态 during（du 或 on）动作

- 处于激活状态，有驱动事件发生 on Event动作

- 处于激活状态或其子状态处于激活状态 bind动作

Name/Keyword:Action

Name
Keyword:Action

### 迁移：状态的转换

由事件和条件触发，只有在激活状态下才能执行相应的程序

方向性

有效的迁移：一种状态到另一种状态的迁移

迁移标签：
event[condition]{condition_action}/transition_action

优先级：

1. 既有事件又有条件的最先被检测

2. 仅有事件的第二个被检测

3. 仅有条件的第三个被检测

4. 不加任何限制的迁移最后被检测

迁移执行流程：

检测是否为有效迁移
执行exit动作
标记源状态为非激活状态
目标状态为激活状态
执行目标状态的entry动作
stateflow图表回到睡眠状态

### 事件：触发的发生

### 数据对象

### 条件[]]与动作{}

条件：指定一个布尔表达式，当它为真时，发生迁移

只要条件为真，无论迁移是否有效，条件动作都会执行

### 节点

用单个迁移表达多个可能发生的迁移

1. 一个源状态到多目标状态的迁移

2. 多个源状态到一个单一目标状态的迁移

3. 基于同一事件的源状态到目标状态的迁移

迁移的执行必须到达某个状态才行

### 图形化逻辑结构

if-else结构

for结构

自循环迁移

### 流程图

由节点和迁移组成

快速建立流程向导

## 嵌入式代码生成

- 使用Simulink Coder生成代码

- 使用Embedded Coder生成代码
  
## 代码效率分析——Profiler探查器

计算代码执行时间，统计各函数所消耗的时间

对于运行时间最长的函数，要评估所消耗的时间是否必要或者是否由可替代方案

```MATLAB
  profile viewer
  
```
## MBD基于模型的设计方法

Simulink提供框图建模与仿真，提供代码自动生成

MAAB，Mathworks Automotive Advisory Board
