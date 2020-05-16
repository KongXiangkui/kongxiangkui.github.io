---
title: CSS笔记
date: 2020-03-31 21:14:32
tags:
- Web
- CSS
- 设计
categories:
- [Web]
- [设计]
toc: true
---
本文记录关于CSS3学习笔记
<!-- more -->

CSS, A style sheet language  

主要研究问题：
-  How to organize your code in order to create maintainable structures with a long lifespan.
-  An excellent code base has to deal with the insane amount of devices, screen sizes, capabilities, and user preferences.(accessibility, internationalization, and browser support)

CSS历史：
- CSS发布1996
- CSS2发布1998
- CSS3目前

[Code Validation](jigsaw.w3.org/css-validator/)

HTML元素：
- 块级元素block level, it takes up space on the page and can have margin and other spacing values applied to it.
- 内联元素inline element:(e.g. image. So to apply margins to the image, we have to give the image block-level behavior using display: block;)

理解CSS：设想HTML元素周围有一个盒子box model
CSS创建规则，控制每个盒子及盒子里的内容的呈现方式（包括size, color, position, etc.）
CSS将样式规则与HTML元素相关联

- padding, the space just around the content (e.g. around paragraphs text).
- border, the solid line that sits just outside the padding.
- margin, the space around the outside of the element.
![](CSS/%E7%85%A7%E7%89%87%202020%E5%B9%B42%E6%9C%8815%E6%97%A5%20%E4%B8%8B%E5%8D%8861535.jpg)
in this section we also use:
- width
- background-color, the color behind an element’s content and padding.
- color, the color of  an element’s content.
- text-shadow, sets a drop shadow on the text inside an element.

CSS规则的组成：
- 一个选择器：表明要应用规则的元素，同一条规则可以应用在多个元素上，需要用逗号隔开。
- 一条声明：用于表明如何显示选择器所指的元素。声明位于花括号中，包括两个部分（属性和值），用冒号作为分隔符。可在一条声明内指定多个属性，各个属性用分号隔开。

**Anatomy（解剖） of a CSS ruleset**
- Selector: The HTML element name at the start of the rule set. It selects the element(s) to be styled (in this case, \<p> elements). To style a different element, just change the selector.
- Declaration: A singular rule like color: red
- Property
- Value

## CSS 使用方式

### 使用外部CSS

\<link>元素（空元素）：在HTML文档中使用，告诉浏览器如何寻找用于定义样式的css文件。位于\<head>元素中。

特性：
- href特性：css文件的路径（通常位于css或styles文件夹中）
- type特性：表明页面所链接的文档类型，特性值为text/css
- rel特性：表明HTML文档与被链接文件的关系（当链接到一个css文件时，特性值为stylesheet

> 一个HTML文档可以是使用多个css样式表，页面要为所使用的每个css文件添加一个\<link>元素。如一个css控制表现（字体和颜色），一个css控制布局  

### 使用内部CSS

\<style>元素，在HTML页面天假css规则，置于此元素内，此元素位于\<head>中
- type特性：表明这些样式在css中指定的，特性值text/css
> 当建立一个包含多个页面的网站时应该使用外部样式表（允许所有页面使用同样的样式规则；将页面与样式分离；可以通过修改一个样式，修改所有页面的样式  

## Different type of selectors: CSS选择器（区分大小写）
- 通用选择器：应用于文档中所有元素 *{}
- 类型选择器：匹配元素名称与选择器相同的元素 h1,h2,h3{}
- 类选择器：匹配这样的元素：元素的class特性值与此选择器点符号后面的部分相同 .note{}应用于所有class特性值为note的元素 p.note{}只应用于class特性值为note的\<p>元素
- id选择器：匹配这样的元素：元素id特性值与此选择器#后面的部分相同 #introduction{} 应用于id值为introduction的元素
- 子元素选择器：匹配指定元素的直接子元素 li>a{}应用所有父元素为\<li>的\<a>元素（对页面中的其他\<a>不起作用）
- 后代选择器：匹配指定元素的后代元素（不仅是指定元素的直接子元素）p a {}应用于所有\<p>元素中的\<a>元素（不论它们之间有无潜逃其他元素）
- 相邻兄弟选择器：匹配一个元素相邻的兄弟元素 h1+p{}应用于\<h1>元素之后第一个\<p>元素
- 普通兄弟选择器：匹配一个元素的兄弟元素，不论这个元素是不是与他的兄弟元素相邻 h1～p{} 如果两个\<p>元素均为\<h1>元素的兄弟元素，那么这些规则对两个\<p>元素均起作用

规则的优先级原则：
- 就近原则：如果两个选择器完全相同，后出现的选择器的优先级高
- 具体性原则：如果一个选择器比其他选择器更具体，更具体的选择器优先级更高
- 重要性：可以在任意属性值后面加!important强调

属性的继承
background-color属性和border属性不会被子元素继承
> 将属性值设为inherit，强制大多数元素从它的父元素中继承属性值  

> 在正式启用网站前，将其在多个浏览器中进行测试，不同的浏览器在显示上可能会有细微的差异（可使用在线工具测试）  

注释/* comments */
有助于CSS文档的理解，将长文档分为几部分，有利于组织

## 颜色

color属性：指定元素中文本的颜色（前景色）
background-color属性：背景色（未指定，默认透明）

计算机显示器由成千上万个像素组成，每个像素的颜色按照红绿蓝三种颜色混合
- RGB值：rgb（100, 100, 100）
- 十六进制编码（每两位一个值，红绿蓝）#ee3e80
- 颜色名称：浏览器支持147种预定义的颜色名称
> CSS3引入HSLA方式来指定颜色  

HSL颜色：色调、饱和度（百分数表示）、明度（又称为辉度，一种颜色中白色（明亮）或者黑色（黑暗）的含量，0%黑色，50%标准色，100%白色）
HSLA颜色：增加一个透明度

- 色调：在HSL颜色中，用一个色环表示，一个角度对应一个色调（0～360）
- 饱和度：颜色中灰色的含量，饱和度到达最大时，颜色中灰色的含量为0，饱和度最小时，基本是灰色。
- 亮度：颜色中黑色的含量，亮度最大时，颜色中黑色的含量为0，最小时，颜色非常暗
- 对比度：为使文本清晰易读，前景色与背景色一定要有足够的对比度；对于大量文本的网页，过高的对比度反而不好，可以适当降低对比度，提升可读性，可以在白色背景使用深灰色文本或黑色背景使用灰白色文本（降低对比度）
- 透明度：opacity属性（属性值：0.0～1.0）

> 可以使用rgba属性指定颜色，增加了一个透明度，只会影响到它的元素，不会影响到子元素  

> 考虑到旧浏览器不支持，要添加额外的规则（旧规则在前，新规则在后）  

## 文本

字体术语
- 衬线字体SERIF：在字母笔画的末端有一些额外的装饰（衬线）～便于阅读，应用于长篇文本
- 无衬线字体SANS-SERIF：字母拥有更笔直的线条，简洁～屏幕分辨率低于打印分辨率，如果文本非常小，更清晰
- 等宽字体MONOSPACE：每个字母等宽，非常容易对齐～便于文本跟随，常用于代码显示
- 草书字体CURSIVE：加入笔画或者其他草书特定，比如：手写风格
- 虚幻字体FANTASY～通常作为装饰字体，用于标题，不适合长篇幅文本

- 粗细Light、Medium、Bold
- 样式Normal、Italic、Oblique（倾斜）
- 伸缩：Condensed、Regular、Extended（压缩版本，祖母更窄、间距更小；反之、更大）

> 可以指定多种字体，并为其创建一个优先次序（防止用户没有安装指定首选字体），称之为字体堆栈  

> 字体会受到版权的制约  

扩大字体的选用范围：
- 字体系列FONT-FAMILY：要求用户已经安装了相应的字体
问题：用户安装的字体种类有限
许可：由于并未分发字体，不涉及许可问题
- 服务器端字体FONT-FACE：使用CSS指定字体下载地址
问题：用户必须首先下载字体，会减缓网页的加载速度
许可：必须得到字体使用许可，允许使用@font-face分发字体（选用范围有限）
- 基于服务的服务器端字体SERVICE-BASED FONT-FACE：
问题：需要向字库支付费用获得许可

图像中的alt文本：可以使用计算机上任何具有使用权限的字体

font-family属性：字体的选用，属性值为希望使用的字体，可以使用一组字体，用逗号隔开，字体堆栈。如果一个字体名称由多个单词组成，需要将字体名放在双引号种
> 最好不要在同一个页面使用三种以上的字体  

font-size属性：指定字体大小
- 像素：可以对文本占用空间进行精确控制，在像素值后面加px
- 百分数：文本在浏览器中默认大小16px，75%相当于12px，200%相当于32px。（父元素大小的百分数）
- EM值：1em相当于1个m的宽度
> 要保证字体以希望的大小出现，最好使用像素为单位对字体进行设置。  
> 可以使用pt代替px，便于打印。  

> 欧洲印刷业的刻度和比例设置的字体的大小  

选用更多的字体
@font-face指定字体的下载地址，src属性指定字体路径，format属性指定所提供字体的格式（许多字体不允许这样使用，开源字体可以）
@font-face{
	font-family: ‘ChunkFiveRegular’;/* 字体 */
	src: url(‘font/chunkfive.net’);}
h1, h2 {
	font-family:ChunkFiveRegular, Georgia, serif;}

字体格式：
- eot
- woff
- ttf/otf
- svg

font-weight属性：粗体，特性值normal和bold
> \<body>元素内的所有文本要以粗体显示，但需要提供一个选项，以便使文本可以以普通粗细显示  

font-style属性：斜体，特性值norml，italic（使文本以斜体显示），oblique（文本倾斜显示，普通文本倾斜一定角度）

text-transform属性：大小写，值为uppercase、lowercase、captitalize（首字母大写）
> 如果确实需要大写，有必要使用letter-spacing属性以增加每个字母间的间距，提高可读性  

text-decoration属性：下划线和删除线，值为none（把应用在文本上的装饰线删除）、underline（下划线）、overline（文本顶部增加一条实线）、line-through（实线穿过文字）、blink（文本动态闪烁，不建议使用）
> 通常应用来删除链接的下划线  

line-height：文本行的高度，增加会使文本行在垂直方向上的间隙增大，以提高可读性（行间距应大于文字间距，行间距初始值最好设定在1.4em～1.5em）
> 最好将line-height属性以em值给出，而不用像素值（使行间距根据用户选择的文本大小来设定。  

> 对于一种字型，位于基准线以下的部分叫字母下缘descender，字母的最高点称为字母上缘ascender。行间距leading是指某行字母下缘的底端到下一行字母上缘的顶端之间的距离。  

> 字距是值字母之间的空隙，标题采用大写形式，需要增加字距，普通文本不要。  

letter-spacing：字母间距
word-spacing：单词间距
> 值应该以em形式指定，所指定的值会加到字体的默认单词间距上。  
> 单词的默认间距由字型设定（一般为2.5em），一般不修改  
> 粗体或者已经增加了字母间距，可以通过增加单词间距，增加可读性  

text-align对齐方式，属性值：left、right、center、justify（文本两端对齐，即段落中除了末行以外，其他每行都要走宽度上占满文本所在容器，主要用于行宽不够或者文本中含有很长的单词）

vertical-align垂直对齐，更多的用于内联元素，比如\<img>元素、\<em>元素\<strong>元素，实现类似\<img>元素上的align特性。属性值：baseline、sub、super、top、text-top、middle、bottom、text-bottom，还可以选用长度值（通常以像素或em值指定）或行高的百分数

text-indent文本锁进，允许元素中首行文本进行缩进，缩进量通常用像素值或em值指定（可以使用负值，向左缩进）？？？？

text-shadow投影，比文本颜色更暗的版本，位于文本的后方，并略有偏移，还可以通过添加亮度与文本稍高的阴影创建浮雕效果。需要指定三个长度值和一种颜色

- 第一个长度值：表明阴影向左或向右延伸的距离
- 第二个长度值：表明阴影向上或向下延伸的距离
- 第三个长度值：可选项，用于指定投影的模糊程度
- 颜色：投影的颜色值

:first-letter首行字母

:first-line首行文本
为首行字母或文本另外指定一个值（不是属性，而是伪元素～在代码中加入的额外的元素，类特性的额外值）在选择器末尾处指定为元素，然后按照与其他元素一样的方式声明）

:link（尚未访问的）,:visited（已访问的）链接样式（伪类），允许为已访问的和尚未访问的链接定义不同的样式，常用于控制链接的颜色以及是否显示下划线。

> 默认情况下，浏览器以蓝色显示连接并附带下划线，浏览器还会改变用户已经访问过的链接的颜色  

:hover用户将某个定位设备（比如光标）悬停在某个元素上时生效。当光标悬停在链接或按钮上上时，改变链接或按钮的外观（对触屏设备不起作用）

:active用户在元素上进行操作时，生效，如单击链接时，改变链接外观

:focus在元素拥有焦点时生效。所有交互性元素（比如可单击的链接及所有表单控件）都可以拥有焦点。（当浏览器发现你准备与页面中某个元素进行交互式，这个元素就获得焦点

> 当使用多个伪类时，遵循:link、:visited、:hover、:fovus、:avtive顺序  

特性选择器

## 盒子

每个HTML元素就像是位于各自的盒子之中，使用CSS控制盒子的外观

- width、height盒子的大小，单位常用像素值（更精确），百分数（盒子的大小与浏览器的窗口大小相关，相对于外部盒子的大小而言的），em值（以盒子中文本的大小为基准）

> 默认盒子的大小刚好容下其中内容，并根据内容变化而变化  
> 传统采用像素值确定，想再为了针对不同大小的屏幕，更多采用百分数和em值  

- min-width，max-width宽度限制

> 为了适应屏幕大小，有时会使度展开或缩放页面大小，做出限制，以保证页面的内容清晰易读或不至于过宽  

- min-height，max-height高度限制

> 合理控制，避免出现内容溢出盒子，使用overflow属性，告诉浏览器，当盒子溢出时，盒子本身如何显示，属性值：hidden把溢出的内容隐藏；scroll在盒子上添加一个滚动条，用户可以使用滚动条查看  

- BORDER边框，将盒子的边缘与另外的盒子隔开（不一定可见）
- MARGIN外边距，位于边框边缘之外，可以通过设置外边距的宽度在两个相邻盒子之间创建间隙
- PADDING内边距，盒子边框与盒子所包含的内容之间的空隙，提高内容的可读性
- 空白区：页面上项目之间的间隙

> 在页面上的不同项目之间增加空隙  

- border-width属性，控制边框的宽度，值为像素值，也可以是thin、medium、thick

 通过border-top-width、border-right-width、border-bottom-width、border-left-width对边框的大小进行控制  
> 在一个属性中设置：border-width: 2px 1px 1px 2px;（上右下左）  

- border-style边框样式，solid实线、dotted一串方形点（2px则是指方向点的边长为2px）、dashed虚线、double两条实线（width属性值为两条实线宽度的和）、groove雕入页面效果、ridge在页面上凸起效果、inset嵌入页面效果、outset看起来像是要凸出屏幕、hidden（或者none）不显示任何边框

> 可以通过border-top-style、border-right-style、border-bottom-style、border-left-style单独设置边框样式  

- border-color边框颜色，采用RGB值、十六进制码、CSS颜色名、HSL值指定

> 可以通过border-top-color、border-right-color、border-bottom-color、border-left-color单独设置边框样式  
> 在一个属性中设置：border-color: darkcyan deeppink darkcyan deepplink;  

- border属性中，快捷指定边框的宽度、样式、颜色
> border: 2px dotted #0088dd  

- padding属性，指定元素内容和元素边框之间的空隙，值使用像素值（em值和百分数也可以）
> 可以通过padding-top、padding-right、padding-bottom、padding-left分别指定  
> 在一个属性中设置：padding:2px 3px 2px 3px;  
> padding: 10px 20px;（左右为10px，上下为20px）  
> 不会被其子元素继承，每一个都需要指定  

- margin属性，指定盒子之间的间隙（即element在页面上所占的空间）

> 可以通过margin-top、margin-right、margin-bottom、margin-left分别指定  
> 在一个属性中设置：margin:2px 3px 2px 3px;  
> margin: 10px 20px;（左右为10px，上下为20px）  

> 之前版本，盒子的宽度包含内边距和外边距  
> HTML5直接会加上内边距和外边距  

内容居中：将margin-right和margin-left属性设置为auto，需要为盒子设置宽度（否则会占满整个页面宽度）
> text-align属性，设置为center，可以被继承  

- display属性，内联元素与块级元素的转换，属性值为inline（使一个块级元素表现得像内联元素）、block（使一个内联元素表现得像块级元素）、inline-block（可以是一个块级元素相内联元素那样浮动并保持其它的块级元素特征、none（将一个元素从页面上隐藏）
> 使用此属性，不应该在内联盒子中创建块级元素  

- visibility盒子的隐藏，但保留了元素原来占用的空间，属性值hidden（隐藏）、visible（显示）
> 如果隐藏不希望显示空白，在display属性中，设置为none  

- border-image属性，将图片应用到盒子的边框，需要的信息：图片的URL、切割图片的位置、如何处理直边（选用值：stretch伸展图片、repeat重复图片、round类似repeat，如果重复图片遇到边界时不合适，根据边框大小动态调整图片大小）
> 盒子的边框必须有一定的宽度，图片才能显示出来  

CSS3
- box-shadow盒子的投影，属性值必须包含前两项值和一个颜色值（水平偏移，负值表示左侧；垂直偏移，负值表示将阴影置于盒子上方；模糊扩展：如果省略，阴影会像实边一样；阴影扩展：正值使阴影向四周延伸，负值使阴影收缩）
> 在值的前面使用inset关键字来创建内部阴影  

- border-radius实现圆角功能，属性值表示半径
> 可以通过border-top-right-radius、border-bottom-right-radius、border-bottom-left-radius、border-top-left-radius分别指定  
> 在一个属性中设置：border-radius:2px 3px 2px 3px;  

> 可以针对一个圆角使用border-radius：80px 50px；  
> 宽和高不一样的圆角，创建椭圆形  

## 列表

list-style-type项目符号样式
- 对于无序列表，值为none、disc（实心圆）、circle（空心圆）、square（实心方块）
- 对于有序列表，值为decimal（数字1、2、3）、decimal-leading-zero（数字01、02、03）、lower-alpha（a、b、c）、upper-alpha（A、B、C）、lower-roman（i、ii、iii）、upper-roman（I、II、III）

list-style-image项目图像，将一个图像作为项目符号，属性值以url开头，后面跟一个圆括号，括号里是双引号框起来的路径

list-style-position标记的定位，表明标记显示的位置，是在包含主体内容的盒子内部还是在外部，属性值为outside，标记位于盒子左侧（默认处理方式）；inside标记位于盒子内部，同时文本块被缩进

list-style列表快捷方式，允许按任意顺序表示标记的样式、图像和位置属性

## 表格

width表格的宽度
padding每个单元格边框与其内容之间的间隙
text-transform将表格标题中的内容转换为大写
letter-spacing，font-size为表格标题的内容增加样式
border-top、border-bottom设置表格标题的上下方的边框
text-align用于将某些单元格中的书写方式设置为向左对齐或向右对齐
background-color用于交替改变表格行的背景颜色
:hover在用户把光标悬停在某个表格，将此行高亮显示

**表格常用设置**
- 设置单元格内边距：增加内边距有助于提高可读性
- 区分标题：将标题（<th>)以粗体形式显示，还可以将标题大写，为其添加背景色和下划线
- 交替改变表格行的背景色：有助于用户一行一行查看，可使用与表格行的正常颜色有细微差别的背景色
- 对齐数字：对于含有数字的列（使用class），使用text-align属性将其右对齐，有助于比较大小

empty-cells空单元格的边框：是否显示空单元格的边框，属性值show、hidden、inherit（对于嵌套表格，遵循外部表格的规则）

border-spacing控制相邻单元格之间的距离，默认情况下，单元格之间有一个间隙，可以增加或减小，属性值使用像素值（一次指定两个值，分别表示横向距离和纵向距离）
border-collapse尽可能的将相邻两个单元格的边框和为一个边框（避免两个边框叠加，宽度为两倍）（border-spacing和empty-cells会被忽略）
seperate，将相邻的边框分离（border-spacing属性会生效）

## 表单
CSS定义下列控件的样式：
- 文本输入框或文本域
- 提交按钮
- 表单中的标签，可以精确的对表单控件进行对齐
> 保持表单外观的一致性，http://formalize.me  

定义单行文本框样式
- font-size设置文本大小
- color设置文本颜色
- backgroound设置背景色
- border在文本输入框边缘增加边框
- border-radius用于创建圆角
- 伪类:foucs用于在使用输入文本框时，改变文本框的背景颜色
- 伪类:hover用于在用户将光标悬停在文本框时，改变文本框背景色
- background-iamge为盒子增加背景图像（由于每个输入框的图像各不相同，使用特性选择器，通过查找每个输入框的id特性值来确定文本输入框）

定义提交按钮样式
- color控制按钮上的文本颜色
- text-shadow在浏览器中展示3D效果的文本
- border-bottom使按钮的下方边框稍粗一点，具有3D效果
- background-color使提交按钮从周围的项目凸显出来
- 伪类:hover当用户光标悬停在按钮上，改变按钮的外观（背景会改变，文本变暗，按钮上方的边框会变粗

定义字段集及其说明的样式
- 字段集：确定一个表单的边缘，在一个长表单中，用来将信息分组
- \<legend>用于说明控件组中需要何种信息
以上元素常用属性：
- width控制字段集宽度，强制将表单元素换行显示在合适的位置
- color用于控件文本的颜色
- background-color属性用于改变这些颜色的背景色
- border控制字段集周围边框的外观
- border-radius
- padding增加元素的内边距

表单控件的对齐
- 当表单中只包含文本输入框，可以将所有文本输入框设置为相同宽度，并将表单中的内容设置为右对齐
- 使用元素的class特性，设置为相同的值，在这些元素上设置width属性

cursor光标样式
- auto
- crosshair
- default
- pointer
- move
- text
- wait
- help
- url(“cursor.gif”)
> 尽可能复合用户的习惯  

## Web开发工具条（扩展）
当把光标悬空在某个元素上，该扩展显示出应用到该元素的CSS样式和HTML结构（轮廓线、HTML结构、CSS样式）
www.chrispederick.com/work/web-developer
View Style Information


## 布局
- 块级元素block：开始新的一行，并且尽可能充满容器
- 内联元素inline：（行内元素）在周围的文本之间流动，img，span

display属性：CSS中用于控制布局
每一个与element都有一个默认的display值，与元素类型有关，大多数默认值为block或inline。
display可以取none值，通常用于JavaScript在不删除元素的情况下隐藏（不会占据本来应该显示的空间，visibility:hidden，不显示但会占用空间）或显示元素。
display还可以取list-item，table等。

设置块级元素的width可以防止它从左到右撑满整个容器，可以设置左右外边距margin为auto使其水平居中。（元素会占据指定宽度，剩余宽度一分为二为左右边距）。当浏览器窗口比元素width窄，浏览器会显示一个水平的滚动条来显示页面。
**改进**：使用max-width代替width，会自动换行

盒模型
> 盒模型不直接  
box-sizing属性
当设置一个元素为 box-sizing: border-box;时，此元素的内边距和边框不会再增加它的宽度。

如果想要页面上所有元素都这样（在不同的浏览器）：
{
-webkit-box-sizing: border-box;
-moz-box-sizing: border-box;
box-sizing: border-box;
}

position属性
- 默认值：static
> 任意position: static;的元素都不会被特殊定位。一个static元素表示它不会被positioned，一个元素的position属性被设置为其他值表示它会被positioned。  
- relative的表现与static一样，除非添加额外属性（top、right、bottom、left），其他元素不会受该元素位置的影响
- fixed固定定位，相对于视窗，即便页面滚动，元素还是会停留在相同的位置
- absolute绝对定位，与fixed的表现类似，不是相对于视窗，而是相对于最近的postioned父元素，如果没有，则相对于文档的body元素，并且，它会随着页面滚动而移动

> 如果使用一个固定定位的页眉或页脚，确保有足够的空间显示，在body里面加入了margin-bottom  

float属性：用于实现文字环绕图片
img {
	float: right;
	margin: 0 0 1em 1em;
}
clear属性：用于控制浮动
clear: left /*清楚元素元的向左浮动*/
> right则是清除向右浮动  
> both同时清除向左向右浮动  

百分比宽度：一种相对于包含块的计量单位，对于图片很有用。

媒体查询工具

inline-block布局

columns属性

flexbox布局模式被用来重新定义CSS的布局方式


关于元素定位的核心概念
> CSS采用盒子模型处理每个HTML元素，盒子可以是块级盒子也可以是内联盒子，可以通过设置每个盒子的宽度高度来控制其占用的空间。要将不同的盒子分开，使用边框、外边距、内边距和背景颜色  
- 块级盒子另起一行显示，在布局中就像是主体构建块
- 内联盒子在其周围文本间浮动
> 一个块级元素位于另一个块级元素内，外部的框就称之为包含元素或父级元素（只能是直接父元素）  

控制页面布局的定位机制
- 普通流：每个块级元素都换行显示，即使指定盒子的宽度，并且有足够的空间显示两个盒子，它们还是不会并排在一起（一个接一个的向下排列）
- 相对定位：将一个元素从它在普通流的位置上移动，在它原来的位置向左、向右、向上、向下移动，不会影响周围元素的位置，他们还在普通流中的位置
- 绝对定位：完全脱离了普通流，不会影响到周围任何元素的位置（直接忽略掉它所占据的空间，会随着页面的滚动而移动

使用盒子的位移属性将盒子与上下左右的距离告诉浏览器

- 固定定位：绝对定位的一种形式，与相对于包含元素定位不同，将元素相对于浏览窗口进行定位，不会影响周围元素的位置，当页面上下滚动时，它不会移动
- 浮动元素：让其脱离普通流，并定位到其包含盒子的最左边或最右边的位置，会成为一个周围可以浮动其他内容的块级元素
> 任何元素从普通流脱离时，都会产生重叠，可以使用z-index属性控制那个盒子显示在上层  

* position: static普通流
> 可以不必使用CSS属性表明，是默认的  
* position: relative相对定位，以其在普通流中所处的位置为起点进行移动，将position属性设置为relative，然后使用位移属性top、bottom、left、right，指定该元素需要从普通流位置移动多少距离（属性值为像素值、百分数、em值）
* position: absolute绝对定位，脱离普通流，不再影响页面其他元素的位置，盒子的位移属性指定元素相对于它的包含元素应该显示的位置
> 默认下，浏览器会在\<h1>元素上方增加一个外边距，浏览器窗口顶端与\<h1>元素的盒子之间存在空隙的原因（删除间隙margin: 0px）  
* position: fixed固定你定位，元素相对于浏览器窗口定位，脱离普通流，不影响周围元素的位置（用户滚动页面，此元素不动）
- z-index属性（堆叠上下文），值是数字，数值越大，元素的层次越靠前
- float浮动元素，将普通流中的元素在它的包含元素内，尽可能的向左或向右排列，位于包含元素内的其他内容会在浮动元素周围流动
> 使用浮动元素要用width属性控制宽度，保证一致性  

使用浮动元素实现并排
每个元素使用float属性，设置每一个元素的width属性
- 将所有段落的高度设置为最高高度，可以保证排列整齐（但文本长度可能发生变化，不好确定）
- 使用clear属性，清除浮动，指定某个盒子的左侧或右侧不允许出现浮动元素（在同一个包含元素内），属性值：left（左侧不能接触同一个包含元素内的其他任何元素）、right、both（左侧和右侧都不能）、none（盒子两侧都可以接触元素）
> 如果一个包含元素只有一个浮动元素，浏览器将其高度看成0像素  
> 传统解决方法：在最后一个浮动盒子后面加一个额外的元素，为此元素增加一条CSS规则，将其clear属性值设为none（在HTML中加入一个元素，以调节包含元素的高度）  
> 新解决方法：在包含元素的样式中设置overflow属性为auto，width属性设置为100%  

利用浮动创建多列式布局
设置元素的width（设置列宽）、float、margin（设置列与列之间的间隙）

考虑问题：
1. 屏幕大小
2. 屏幕分辨率（一个屏幕在每英寸面积内所能显示的点数，分辨率越高，显示的文本越小）
3. 页面大小：常创建960～1000像素的页面（大多数设备可以浏览这个宽度的设计）
> 许多设计者设法让用户在顶部（高度方向的）570～600像素内了解网站的主题  

布局：
- 固定宽带布局：不会因用户扩大或缩小浏览器窗口而发生变化（以像素作为设计的衡量单位）～精确控制，不考虑用户窗口大小；页面边缘可能会存在较大的空白区，如果用户屏幕分辨率过高，看起来会很小
- 流体布局：随着用户对浏览器窗口的变化而扩大或所小（使用百分数作为设计单位）～页面会伸展到整个浏览器窗口，用户窗口很小，会收缩，不需要使用进度条；某些项目挤压在一起
> 有些流体布局只让页面的一部分进行伸缩，其他部分限制最大最小宽度  

布局网络
> 任何视觉艺术的构成都是对视觉元素的整理或布置  
借助网格结构在页面组织元素

960像素网格（960像素宽的12列网格，每一个灰色的列是60像素，列的外边距是10像素，两列之间是20像素）——网格在项目间设置统一的比例和间隔
![](CSS/%E7%85%A7%E7%89%87%202019%E5%B9%B411%E6%9C%8817%E6%97%A5%20%E4%B8%8B%E5%8D%8872445.jpg)
![](CSS/%E7%85%A7%E7%89%87%202019%E5%B9%B411%E6%9C%8817%E6%97%A5%20%E4%B8%8B%E5%8D%8872455.jpg)

- 在具有不同设计风格的页面间建立连贯性
- 帮助用户在各种各样的页面中判断在哪里查找信息
- 按照统一的方式向网站中增加新的内容
- 有助于按照统一的方式设计网站

CSS框架：为常见任务提供框架，避免为同一任务重复编码，便于页面设计，包含的代码超出特定网页的需求，需要在HTML中使用类名称
- 创建布局网格
- 设置表单样式
- 创建面向打印的页面版本

www.960.gs（960网格系统CSS框架）
blueprintcss.org
lessframework.com

@import多个样式表，采用模块化方式，几个单独的样式表控制页面（分别控制布局、印刷排版、表单、表格、字体和颜色、网站每个子栏目的不同样式）

在一个页面中添加多个样式表的方法
- HTML页面链接到一个样式表，在此样式表CSS文件中，使用@import规则导入（@import规则出现在其他规则前面）其他样式表
- 在HTML页面使用多个\<link>元素，分别引用样式表
> 如果两条规则应用于同一个元素，后出现的规则的优先级高于先出现的规则  
> @import 导入规则的地方就是规则出现的地方  

## 图像

> 为图像指定大小，有助于更流畅的加载页面  

许多网站在大量页面中使用相同大小的图像，使用CSS控制图像大小，不必将其添加到HTML代码中
- 小型肖像220x360
- 小型景观：330x210
- 专题照片：620x400

> 对于全站使用的通用的图像大小，为每种大小指定一个名称small、medium、large，在HTML中作为class特性值使用。在CSS中为每个类名增加特性选择器，使用width和height指定大小  


使用align属性对齐图像（传统）

使用float对齐图像：减肥float添加到控制图像大小的类中，使用诸如align-left和align-right的名称创建新类，将这些类的名称与表示图像大小的类的名称一起使用
> 为了确保文本不会与图像的边缘接触，给图像增加一个外边距  

使用CSS将图像居中

> 图像属于内联元素，与周围文本一起流动，为使其居中，应将其转化为块级元素，通过display属性的值将其设置为block  
居中方式

- 对于图像的包含元素，使用text-align属性值设置为center
- 对于图像本身，使用margin属性，将其左右外边距设置为auto
- 使用<figure>元素指定图像大小并对齐图像

background-image背景图像，在HTML元素之后放置图像，背景图像可以填满整个页面或页面的一部分（默认重复填满整个盒子）
指定图像的路径，跟在url字母之后，并位于圆括号和引号之中
> 背景图像一般最后加载  

background-repeat重复图像，属性值：repeat（水平方向和垂直方向都重复）、repeat-x（在水平方向重复）、repeat-y（在垂直方向重复）、no-repeat（背景图像只显示一次）
background-attachment指定背景图像在用户滚动页面时的移动方式，属性值：fixed（位置固定不变）、scroll（虽用户上下滚动而移动页面）

background-position背景图像定位，属性值：
left top、left center、left bottom
center top、center center、center bottom
right top、right center、right bottom
（如果只指定一个值，第二个值默认center）
还可以使用一对像素值或百分数（到浏览器窗口或盒子左上角的距离）

background快捷属性，必须按照如下顺序指定
1. background-color
2. background-image
3. background-repeat
4. background-attachment
5. background-position
> CSS3支持重复使用background，以此应用多个图像  

图像翻转与子画面
- 翻转：用户光标悬停在一个链接或按钮上时，按钮或链接创建另一种样式，还可以在用户点击时，创建第三种样式（通过为链接或按钮设置背景图像来实现，背景图像包含3种样式，只有显示一种样式的空间，当光标悬停或点击时，背景图像的位置移动，从而显示相关图像）
- 子画面spirte：当一个单独的图像应用在多个不同的部位时
> 用户只需要请求一个图像，反应迅速  
伪类
:hover光标悬停
:active单击

渐变
通过background-image属性创建
> 不同的浏览器需要不同的属性来创建，有的浏览器甚至支持指定渐变角度设置不同类型的渐变（如放射性渐变）  
线性渐变：需要指定渐变两端的颜色

背景图像的对比度：
如果要在一个背景图像上覆盖文本，为了保证文本易读性，背景图像必须是**低对比度的**或者通过在文本的下层插入半透明背景色来提高可读性


## CSS框架

![](CSS/%E7%85%A7%E7%89%87%202020%E5%B9%B42%E6%9C%8818%E6%97%A5%20%E4%B8%8B%E5%8D%8861730.jpg)
