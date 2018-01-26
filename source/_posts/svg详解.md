---
title: svg 详解
date: 2018-1-14 22:47:17
tags: [svg,html5]
categories:
  - svg
---


> SVG 是使用 XML 来描述二维图形和绘图程序的语言。

 SVG 的优势在于：

- SVG 可被非常多的工具读取和修改（比如记事本）
- SVG 与 JPEG 和 GIF 图像比起来，尺寸更小，且可压缩性更强。
- SVG 是可伸缩的
- SVG 图像可在任何的分辨率下被高质量地打印
- SVG 可在图像质量不下降的情况下被放大
- SVG 图像中的文本是可选的，同时也是可搜索的（很适合制作地图）
- SVG 可以与 Java 技术一起运行
- SVG 是开放的标准
- SVG 文件是纯粹的 XML

一个简单的 SVG 文件的例子。SVG 文件必须使用 .svg 后缀来保存：

```

<!-- XML 声明,standalone属性，若为yes表示当前文件独立，这里为no说明不独立，此SVG 文档会引用一个外部文件,如下是 dtd 文件-->
<?xml version="1.0" standalone="no"?>

<!--这里说明引用的dtd文件 位于 “http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd”，该文件归属于W3C，含有所有允许的 SVG 元素。-->
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">

<!--SVG 代码以 <svg> 元素开始，包括开启标签 <svg> 和关闭标签 </svg> ，这是根元素。-->
<!--width 和 height 属性可设置此 SVG 文档的宽度和高度。version 属性可定义所使用的 SVG 版本，xmlns 属性可定义 SVG 命名空间。-->
<svg width="100%" height="100%" version="1.1"
xmlns="http://www.w3.org/2000/svg">

<!--<circle> 用来创建一个圆。cx 和 cy 属性定义圆中心的 x 和 y 坐标。如果忽略这两个属性，那么圆点会被设置为 (0, 0)。r 属性定义圆的半径。-->
<!--stroke 和 stroke-width 属性控制如何显示形状的轮廓。我们把圆的轮廓设置为 2px 宽，黑边框。-->
<!--fill 属性设置形状内的颜色。我们把填充颜色设置为红色。-->
<circle cx="100" cy="50" r="40" stroke="black"
stroke-width="2" fill="red"/>

<!--关闭标签的作用是关闭 SVG 元素和文档本身。所有的开启标签必须有关闭标签！-->
</svg>
```

SVG 文件可通过以下标签嵌入 HTML 文档： embed 、 object  或者 iframe。

1.  使用 embed 标签，embed 标签被所有主流的浏览器支持，并允许使用脚本。但是如果需要创建合法的 XHTML，就不能使用 <embed>，任何 HTML 规范中都没有 <embed> 标签，所以我个人一般不推荐这种做法。


```
<embed src="rect.svg" width="300" height="100"
type="image/svg+xml"
pluginspage="http://www.adobe.com/svg/viewer/install/" />
```
注意：  HTML 页面中嵌入 SVG 时使用 embed 标签是 Adobe SVG Viewer 推荐的方法，pluginspage 属性指向下载插件的 URL。

2.  使用 object 标签， object 标签是 HTML 4 的标准标签，被所有较新的浏览器支持。它的缺点是不允许使用脚本。我一般也不推荐使用这种方法。


```
<object data="rect.svg" width="300" height="100"
type="image/svg+xml"
codebase="http://www.adobe.com/svg/viewer/install/" />
```
注意： codebase 属性指向下载插件的 URL。

3.  使用 iframe 标签，iframe 标签可工作在大部分的浏览器中。但是iframe标签一直被诟病有安全性问题，也是慎用。

```
<iframe src="rect.svg" width="300" height="100"></iframe>
```

4.  使用 img 标签，img 标签几乎所有的浏览器都支持。而且img标签来引入svg也更合理，因为我们通常使用svg也是用做图形，更符合img的语义化。

```
<img src="svg/iframe.svg" alt="svg图形" width="300" height="100">
```

> 除了以上3种引入外部svg文件的方式使用svg，还可以直接写在html文件中,这种仅仅通过引用 SVG 的命名空间，就能够把 SVG 元素之间添加到 HTML 代码中的方式，也是我个人比较推荐的方式。


```
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>svg</title>
</head>
<body>
<p>This is an HTML paragraph</p>
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
    <circle cx="100" cy="50" r="40" stroke="black" stroke-width="2" fill="red" />
</svg>
</body>
</html>
```
在介绍svg的语法之前，我们先来了解一些svg的css属性，svg的css属性可以像html一样，通过给标签添加style的方式添加，也可以像img的width和height一样，直接声明在标签中

常用的css属性：
- fill  定义填充背景色 （rgb 值、颜色名或者十六进制值）
- stroke-width   定义边框的宽度
- stroke 定义边框的颜色
- fill-opacity  定义填充颜色透明度（合法的范围是：0 - 1）
- stroke-opacity 定义边框颜色的透明度（合法的范围是：0 - 1）
- opacity  定义元素的透明值 (范围: 0 到 1)
- rx 和 ry   定义圆角，类似于css中的border-radius
- fill-rule属性用于指定使用哪一种方法去判断画布上的某区域是否属于该图形的“内部”（内部区域将被 填充）。fill-rule有三个属性：nonzero / evenodd / inherit，这里详细分析下fill-rule。

    1.   nonzero : 这个规则，判断一个点是否在图形内，从该点往任一方向绘制射线到无穷远，然后检查图形的线段和射线相交的点，来确定“内部区域”。从0开始计数，每次路径线段是从左到右穿过射线就加一，从右到左的就减一。通过计算交叉点，如果结果是0，则这个点在路径外部，不然，就是在内部。

    2.   evenodd : 这个规则，判断一个点是否在图形内，从该点往任一方向绘制射线到无穷远，然后计算给定图形上线段路径和该射线交叉点的数量。如果这个数是奇数，那么该点在图形内部；如果是偶数，该点在图形外部。

    3.   inherit ：这个表示继承，是css大家很常见的一个属性。

-

我们接着说说svg的常用标签:

1. 矩形 rect，rect 标签可用来创建矩形，以及矩形的变种，rect 元素的 width 和 height 属性可定义矩形的高度和宽度，这里就以一个圆角矩形为例：


```
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <rect x="50" y="20" rx="20" ry="20" width="150" height="150"
  style="fill:red;stroke:black;stroke-width:5;opacity:0.5"/>
</svg>
```


2. 圆形 circle， circle 标签可用来创建一个圆，cx和cy属性定义圆点的x和y坐标，r属性定义圆的半径。


```
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <circle cx="100" cy="50" r="40" stroke="black"
  stroke-width="2" fill="red"/>
</svg>
```

3. 椭圆 ellipse，ellipse 元素是用来创建一个椭圆，CX，CY分别为圆心点的x轴和y轴坐标，RX，RY分别代表水平方向的半径和垂直方向的半径。


```
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <ellipse cx="200" cy="50" rx="100" ry="50" style="fill:purple" />
</svg>
```

4. 直线 line ，line 元素是用来创建一个直线，x1 定义线条x 轴的开始，y1定义线条y 轴的开始，x2 定义线条 x 轴的结束，y2 定义线条 y 轴的结束。


```
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <line x1="0" y1="0" x2="200" y2="200"
  style="stroke:rgb(255,0,0);stroke-width:2"/>
</svg>
```

5. 多边形  polygon ，polygon 标签用来创建含有不少于三个边的图形。

多边形是由直线组成，其形状是"封闭"的（所有的线条 连接起来）。


```
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <polygon points="100,10 40,180 190,60 10,60 160,180"
  style="fill:lime;stroke:purple;stroke-width:5;fill-rule:nonzero;" />
</svg>
```


6. 曲线 polyline ，polyline 元素是用于创建任何只有直线的形状。


```
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <polyline points="0,40 40,40 40,80 80,80 80,120 120,120 120,160" style="fill:white;stroke:red;stroke-width:4" />
</svg>
```

7. 路径 path ，path 元素是用于定义一个路径。path元素的形状是通过属性d定义的，属性d的值是一个“命令+参数”的序列。大写表示绝对定位，小写表示相对定位。在绘制路径时比较复杂，可以议使用SVG编辑器来创建复杂的图形。

- M = moveto，需要两个参数，分别是需要移动到的点的x轴和y轴的坐标。类似于移动画笔的位置。
- L = lineto，需要两个参数，分别是一个点的x轴和y轴坐标，L命令将会在当前位置和新位置（L前面画笔所在的点）之间画一条线段。
- H = horizontal lineto 绘制平行线， 这个命令只带一个参数，标明在x轴或y轴移动到的位置，因为它们都只在坐标轴的一个方向上移动
- V = vertical lineto 绘制垂直线，这个命令只带一个参数，标明在x轴或y轴移动到的位置，因为它们都只在坐标轴的一个方向上移动
- C = curveto  三次贝塞尔曲线。(x,y)表示的是曲线的终点，(x1,y1)是起点的控制点，(x2,y2)是终点的控制点。
控制点描述的是曲线起始点的斜率，曲线上各个点的斜率，是从起点斜率到终点斜率的渐变过程。
- S = smooth curveto 当一个点某一侧的控制点是它另一侧的控制点的对称（以保持斜率不变），可以使用S命令。简写的贝塞尔曲线命令。
如果S命令跟在一个C命令或者另一个S命令的后面，它的第一个控制点，就会被假设成前一个控制点的对称点。如果S命令单独使用，前面没有C命令或者另一个S命令，那么它的两个控制点就会被假设为同一个点。
- Q = quadratic Bézier curve 二次贝塞尔曲线Q，只需要一个控制点，用来确定起点和终点的曲线斜率。因此它需要两组参数，控制点和终点坐标。
- T = smooth quadratic Bézier curveto 与S命令相似，是Q命令的简写命令。
与S命令相似，T也会通过前一个控制点，推断出一个新的控制点。这意味着，在你的第一个控制点后面，可以只定义终点，就创建出一个相当复杂的曲线。
- A = elliptical Arc 创建SVG曲线的命令。


```
A rx ry x-axis-rotation large-arc-flag sweep-flag x y
a rx ry x-axis-rotation large-arc-flag sweep-flag dx dy
```
弧形命令A的前两个参数分别是x轴半径和y轴半径，

- Z = closepath Z命令会从当前点画一条直线到路径的起点。不区分大小写


```
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <path d="M150 0 L75 200 L225 200 Z" />
</svg>
```

8. 文本 text ，text 元素用于定义文本。
- textPath 规定文本书写的方向


```
<svg xmlns="http://www.w3.org/2000/svg" version="1.1"
xmlns:xlink="http://www.w3.org/1999/xlink">
   <defs>
    <path id="path1" d="M75,20 a1,1 0 0,0 100,0" />
  </defs>
  <text x="10" y="100" style="fill:red;">
    <textPath xlink:href="#path1">I love SVG I love web</textPath>
  </text>
</svg>
```

9. Stroke 属性，可应用于任何种类的线条，文字和元素就像一个圆的轮廓

- stroke    属性定义一条线，文本或元素轮廓颜色
- stroke-width    属性定义了一条线，文本或元素轮廓厚度
- stroke-linecap  属性定义不同类型的开放路径的终结
- stroke-dasharray  属性用于创建虚线

```
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <g fill="none">
    <path stroke="red" d="M5 20 l215 0" />
    <path stroke="blue" d="M5 40 l215 0" />
    <path stroke="black" d="M5 60 l215 0" />
  </g>
</svg>
```