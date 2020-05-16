---
title: Python图像处理
date: 2020-04-01 22:50:26
tags:
- 图像处理
- Python
- 设计
categories:
- [Python, 第三方库]
toc: true
---
本文介绍使用Python进行图像处理的相关内容(基于pillow模块)
<!-- more -->

图像处理工具
1. Paintbrush, Paint
2. Photoshop 支持批处理batch process
3. 编程

## 图像与pillow基础知识

pixel

RGBA, Red Green Blue Alpha(transparency透明度, 255表示不透明，0表示透明)
CMYK, Cyan(blue) Magenta(red) Yellow Black

> pillow中RGBA value用四元组表示
> pillow使用标准的颜色名(与HTML一致)

PIL, Python Image Library
> The module name of Pillow is PIL to make it backward compatible with an older module called Python Imaging Library.
> Must use the from PIL import Image form of import statement, rather than simply import PIL. 

```Python
from PIL import ImageColor
# 返回颜色的RGBA值
ImageColor.getcolor('red', 'RGBA')  # (255, 0, 0, 255)
ImageColor.getcolor('chocolate', 'RGBA')    # (210, 105, 30, 255)
```
Pillow函数处理 **box tuple argument**(a tuple of four integer coordinates that represent a rectangular region in an image)
- Left: The x-coordinate of the leftmost edge of the box. 
- Top:  The y-coordinate of the top edge of the box. 
- Right:  The x-coordinate of one pixel to the right of the rightmost edge of the box. This integer must be greater than the left integer. 
- Buttom:  The y-coordinate of one pixel lower than the bottom edge of the box. This integer must be greater than the top integer.


## Manipulating Images with Pillow

Image object data type

### 修改图片

```Python
from PIL import Image
catImg = Image.open('cat.jpg')  # load the image, 从文件创建Image object
print(catImg.size)      # width & height
# (2048, 2048)
print(catImg.filename)
# cat.jpg
print(catImg.format)
# JPEG
print(catImg.format_description)
# JPEG (ISO 10918)
catImg.save('NewCat.jpg')

# 裁剪照片
croppedImg = catImg.crop(10, 12, 20, 21)
croppedImg.save('cropped.png')
```

```Python
from PIL import Image
catImg = Image.open('.\cat.jpg')
catCopyImg = catImg.copy()      # 复制Image object
croppedImg = catImg.crop((100, 120, 300, 300))    # 裁剪的部分
catCopyImg.paste(croppedImg, (0,0,))    # 将裁剪下来的块放到catCopyImg
catCopyImg.paste(croppedImg, (440,550))
catCopyImg.save('pasted.png')

catImgWidth, catImgHeight = catImg.size
croppedImgWidth, croppedImgHeight = croppedImg.size
catCopyTwo = catImg.copy()

for left in range(0, catImgWidth, croppedImgWidth):
    for top in range(0, catImgHeight, croppedImgHeight):
        print(left, top)
        catCopyTwo.paste(croppedImg, (left, top))
catCopyTwo.save('tiled.png')
```

```Python
from PIL import Image
catImg = Image.open('.\cat.jpg')
width, height = catImg.size
# risize() 不是在原图上修改，而是返回一个新的对象
quartersizedImg = catImg.resize((int(width/2), int(height/2)))  # 等比例
quartersizedImg.save('quartersized.png')
svelteImg = catImg.resize((width, height + 300))        # 不等比例
svelteImg.save('svelte.png')

# 旋转图片
rotateImg1 = catImg.rotate(45)      # Windows：旋转之后用黑色背景填充
rotateImg1.save('rotateImg.png')
rotateImg2 = catImg.rotate(-10)
rotateImg2.show()
# rotate有一个keyword argument：expand（设置为True，旋转之后依然能够显示完整的图片）
catImg.rotate(6).save('rotate6.png')
catImg.rotate(6, expand=True).save('rotate6_expand.png')

# 镜面对称
catImg.transpose(Image.FLIP_LEFT_RIGHT).save('horizontal_flip.png')
catImg.transpose(Image.FLIP_TOP_BOTTOM).save('vertical_flip.png')
```

```Python
from PIL import Image
# 直接创建Image object
img = Image.new('RGBA', (100, 200), 'purple')
img.save('purpleImage.png')
```

### 操作单个像素点

- getpixel()
- putpixel()

```Python
from PIL import Image, ImageColor
img = Image.new('RGBA', (100, 100))     # 创建一个100x100 tranparent square
img.getpixel((0, 0))
# 返回值(0, 0, 0, 0)
for x in range(100):
    for y in range(50):
        img.putpixel((x, y), (210, 210, 210))   # 设置pixel的RGBA值

for x in range(100):
    for y in range(50, 100):
        img.putpixel((x, y), ImageColor.getcolor('darkgray', 'RGBA'))  # darkgray是一个非标准色，putpixel()不能接收，所以使用ImageColor.getvolor()去得到darkgray的RGBA值

img.getpixel((0, 0))
# (210, 210, 210, 210)
img.getpixel((0, 50))
# (169, 169, 169, 255)
img.save('putPixel.png')
```

### Drawing on Images

ImageDraw object

```Python
from PIL import Image, ImageDraw
img = Image.new('RGBA', (200, 200), 'white')
draw = ImageDraw.Draw(img)  # 传递Image object，返回ImageDraw object


# draw shapes
# point(xy, fill)
# xy: a list of x- and y-coordinates tuples or a list of x- and y-coordinates without tuples
# fill: the color of the points or an RGBA tuple or a string of color name

# line(xy, fill, with)
draw.line([(0, 0), (199, 0), (199, 199), (0, 199), (0, 0)], fill='black')

# rectangle(xy, fill, outline)
# xy: a box tuple of the form(left, top, right, bottom)
# outline: the color of the rectangle's outline
draw.rectangle((20, 30, 60, 60), fill='blue')

# ellipse(xy, fill, outline)    椭圆
draw.ellipse((120, 30, 160, 60), fill='red')

# polygons(xy, fill, outline)
draw.polygon(((57, 87), (79, 62), (94, 85), (120, 90), (103, 113)), fill='brown')

for i in range(100, 200, 10):
    draw.line([(i, 0), (200, i-100)], fill=green)

img.save('drawing.png')
```
### Drawing Text

ImageDraw object:
text(xy, text, fill, font): 在图片上写文字
textsize(): 返回给定字体的文本的宽和高

ImageFont object

TrueType file字体文件: 
.tff file

- Windows: C:\Windows\Fonts
- OS X: /Library/Fonts and /System/Library/Fonts
- Linux: /usr/share/fonts/truetype

> 实际操作不需要知道字体文件的具体位置

> Pillow 创建PNG图像： 每英寸72pixels； 一个点是1/72英寸

```Python
from PIL import Image, ImageDraw, ImageFont
import os
img = Image.new('RGBA', (200,200), 'white')
draw = ImageDraw.Draw(img)

draw.text((20,150), 'HELLO', fill='purple')

fontsFolder = 'FONT_FOLDER'   # e.g. '/Library/Fonts'
arialFont = ImageFont.truetype(os.path.join(fontsFolder, 'arial.ttf'), 32)
draw.text((100,150), 'Howdy', fill='gray', font=arialFont)

img.save('text.png')
```
