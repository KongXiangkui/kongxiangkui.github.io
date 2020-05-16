---
title: SVG学习
date: 2020-04-30 16:18:21
tags:
- Web
- 图片
categories:
- [Web]
toc: true
---
SVG(Scalable Vector Graphics, 可缩放矢量图形)，是一种基于XML文档的图片格式，其描述基于二维矢量图形，由W3C制定标准，其涉及确保它可以与CSS、DOM以及SMIL等其他Web标准一同运作。SVG图像及其相关行为被定义于XML文本文件中，可以对它们进行搜索、索引、编写脚本以及压缩，这也意味着可以使用任何文本编辑器和绘图软件来创建和编辑它们。
> SVG 相对于图像，就好比HTML相对于文本
> SVG 主要竞争者是Flash
> SVG可以由任何文本文件创建，但往往和一个绘图程序一起使用，如**Inkscape**，可以更方便的创建。
<!-- more -->
## 基本知识

SVG 使用各种元素进行创建，分别应用于矢量图象的结构、绘制和布局。

SVG实例

```xml
<?xml version="1.0" standalone="no"?><!-- 包含XML声明，stanalone属性规定此svg文件是否是“独立的”，或含有对外部文件的引用，属性值为no一位置SVG文档会议用一个外部文件，在这里是DTD文件 -->
<!DOCTYPE svg PUBLIC "-//W#C//DTD SVG 1.1//EN" "http://www.w3.org/Grphics/SVG/1.1/DTD/svg11.dtd"> <!-- 引用外部的SVG DTD -->

<!-- SVG代码的根元素svg，其width、height属性可以设置SCG文档的宽度和高度，version属性可定义所使用的SVG版本，xmlns属性可以定义SVG的命名空间-->
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
    <!-- circle元素用于创建圆，cx，cy定义圆的中心，如果忽略，默认为（0，0），r定义圆的半径，stroke和stroke-width属性控制如何显示形状的轮廓，在此处是黑色，2px宽 fill属性设置填充颜色-->
    <circle cx="80" cy="100" r="40" stroke="black" strocke-width="2" fill="red" />
</svg>
```

SVG 嵌入HTML中

- 使用embed标签：所有浏览器都支持，并允许使用脚本，不推荐在HTML4和XHTML中使用

```HTML
<embed src="circle1.svg" type="images/svg+xml" />
```

- 使用object标签：所有浏览器都支持，但不允许使用脚本

```HTML
<object data="circle1.svg" type="images/svg+xml"></object>
```

- 使用iframe标签：所有浏览器都支持，并允许使用脚本，不推荐在HTML4和XHTML中使用

```HTML
<iframe src="circle1.svg"></iframe>
```

> 很多浏览器支持在HTML中直接嵌入svg元素

## 基本元素

### 矩形\<rect>

```xml
<svg xmlns="http://www/w3/org/2000/svg" version=1.1>
    <!-- x,y分别指定左上角的坐标，rx,ry用于指定圆角大小 style属性用来定义CSS属性，CSS的fill属性用来定义填充颜色,fill-opacity定义填充颜色的透明度0-1，stroke-opacity用于定义边框的透明度，opacity用于定义整个元素的透明度 -->
    <rect x="100" y="100" rx="20" ry="20" width="300" height="100" style="fill:rgb(0,0,255);stroke-width:1;stroke:rgb(0,0,0);fill-opacity:0.1;stroke-opacity:0.9" />
<svg>
```
### 圆\<circle>

详见示例

### 椭圆\<ellipse>

```xml
<svg xmlns="http://www/w3/org/2000/svg" version=1.1>
    <!-- cx,cy定义椭圆中心，rx,ry定义椭圆的水平半径和垂直半径 -->
    <ellipse cx="300" cy="80" rx="100" ry="50" style="fill:yellow;stroke:purple;stroke-width:3" />
</svg>
```
### 线\<line>

```xml
<svg xmlns="http://www/w3/org/2000/svg" version=1.1>
    <!-- LINE-->
    <line x1="0" y1="0" x2="200" y2="200" style="stroke:rgb(255,0,0);stroke-width:2" />
</svg>
```

### 折线\<polyline>

```xml
<svg xmlns="http://www/w3/org/2000/svg" version=1.1>
    <polyline points="20,20 40,25 60,40 230,230 200,180" style="fill:none;stroke:block;stroke-width:3" />
</svg>
```

### 多边形\<polygon>

```xml
<svg height="210" width="500">
    <!-- 多边形-->
    <polygon points="200,10 250,190 160,210" style="fill:lime;stroke:putple;stroke-width:1" />
</svg>
```

### 路径\<path>

可以构建任何高级的2D图形的基本形状。路径元素是通过一系列绘图命令来生效的，非常类似于LOGO语言。

用于路径数据的命令
- M=moveto
- L=lineto
- H=horizontal lineto
- V=vertical lineto
- C=curveto
- S=smooth curveto
- Q=quadratic Bezier curve
- T=smooth quadratic Bezier curve
- A=elliptical Arc
- Z=closepath

> 注意：以上命令允许小写字母，大写表示绝对定位，小写表示相对定位

```xml
<svg xmlns="http://www/w3/org/2000/svg" version=1.1>
    <!-- 定义一条路径，开始于位置150,0到达75,200最后在150,0关闭路径 -->
    <path d="M150 0 L75 200 L225 200 Z" />

    <path id="lineAB" d="M 100 350 l 150 -300" stroke="red" stroke-width="3" fill="none" />
    <path id="lineBC" d="M 250 50 l150 300" stroke="red" stroke-width="3" fill="none" />
    <path d="M 175 200 l 150 0" stroke="green" stroke-width="3" fill="none" />
    <path d="M 100 350 q 150 -300 300 0" stroke="blue" stroke-width="5" fill="none" />
    <!-- Mark relevant points -->
    <g stroke="black" stroke-width="3" fill="black">
        <circle id="pointA" cx="100" cy="350" r="3" />
        <circle id="pointB" cx="250" cy="50" r="3" />
        <circle id="pointC" cx="400" cy="350" r="3" />
    </g>
    <!-- Label the points -->
    <g font-size="30" font="sans-serif" fill="black" stroke="none" text-anchor="middle">
        <text x="100" y="350" dx="-30">A</text>
        <text x="2500" y="50" dy="-10">B</text>
        <text x="400" y="350" dx="-30">C</text>
    </g>
</svg>
```

### 文本

```xml
<svg xmlns="http://www/w3/org/2000/svg" version=1.1>
    <!-- Example 1 -->
    <text x="0" y="15" fill="red">I Love You</text>

    <!-- Example 2 -->
    <text x="100" y="115" fill="red" transform="rotate(30 120,140)">I LOVE YOU</text> 
</svg>
```

```xml
<svg xmlns="http://wwww.w3.org/200/svg" version="1.1" xmlns:xlink="http://www.org/1999/xlink">
    <!-- Example 3: 路径上的文字 -->
    <defs>
      <path id="path1" d="M75,20 a1,1 0 0,0 100,0" />
    </defs>
    <text x="10" y="100" style="fill:red;">
      <textPath xlink:href="path1">I Love SVG I love SVG<textPath>
    </text>
</svg>
```

## SVG 的 stroke属性

所有stroke属性，可以应用于任何种类的仙台哦，文字和元素（就像一个图形都轮廓一样）

- stroke定义一条线、文本或元素的轮廓颜色
- stroke-width定义一条线、文本或元素的轮廓宽度
- stroke-linecap定义不同类型的开放路径的终结
- stroke-dasharray用于创建虚线

> stroke的意思是“描边”

```html
<!DOCTYPE html>
<html>
    <body>
        <svg xmlns="http:www.w3.org/2000/svg" version="1.1">
        <g fill="none" stroke="black" stroke-width="12">
            <!-- stroke-linecap Example -->
            <path stroke-linecap="butt" d="M20 20 l300 0" />
            <path stroke-linecap="round" d="M20 50 l300 0" />
            <path stroke-linecap="square" d="M20 80 l300 0" />
            <!-- stroke-dasharray Example -->
            <path stroke-dasharray="5,5" d="M5 100 l300 0" />
            <path stroke-dasharray="10,10" d="M5 120 l300 0" />
            <path stroke-dasharray="20,10,5,5,5,10" d="M5 140 l300 0" />
        </g>
        </svg>
    </body>
</html>
```

## SVG 的 滤镜

SVG使用滤镜来增加图形都特殊效果

- feBlend
- feColorMatrix
- feComponentTransfer
- feComposite
- feConvolveMatrix
- feDiffuseLighting
- feDisplacementMap
- feFlood
- feGaussianBlur
- feImage
- feMerge
- feMorphology
- feOffset
- feSpecularLighting
- feTile
- feTurbulence
- feDistantLight
- fePointLight
- feSpotLight

> 可以在每个SVG元素上使用多个滤镜

所有互联网的SVG滤镜定义在\<defs>元素种，\<defs>元素定义一短的并含有特殊元素（如滤镜）定义。\<filter>标签用于定义SVG滤镜，使用必需的id用于图形指定滤镜。


## 元素

### 形状元素

### 动画元素

### 容器元素

### 描述性元素

### 滤镜元素

### 字体元素

### 渐变元素

### 图形元素

### 图像渲染元素

### 光源元素

### 非渲染元素 

### 绘制服务器元素

### 可渲染元素

### 结构元素

### 文本内容元素

### 文本子内容元素

### 其他元素

