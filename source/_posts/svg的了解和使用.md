---
title: svg的了解和使用
date: 2024-08-02 11:36:16
tags:
---


1. 什么是SVG
SVG 时使用 XML 格式定义图像，可以用来绘制矢量图形。
比起其他的图像格式（例如jpg，gif），SVG的优点是：
- 可以使用任何文本编辑器来创建绘画SVG。
- 可以搜索、索引、脚本化、压缩SVG图像。
- SVG图像可以扩展，可以在任何分辨率上高质量显示。
- SVG图像支持缩放，且不会失去任何质量。
- SVG是开放标准，是纯XML文件。
<!-- more -->

SVG 是 W3C 推荐标准：
- SVG 在 2003 年一月，SVG 1.1 被确立为 W3C 标准。
svg 可以使用CSS 来设置样式，也可以使用 JS 对 SVG 进行操作。
2.SVG的一些基本使用
1.前端7种使用方法
2.在代码中，CSS大小样式可以覆盖SVG代码中的写的长宽。通过 fill属性，可以修改svg 的填充色
3.兼容性：
1.
4.基本特性： 参考文档
a.笔画特性
属性值stroke笔画颜色，默认值为 nonestroke-width笔画宽度，可用用户坐标或者指定单位的方式指定。笔画的宽度会相对坐标网格 线居中。默认值为 1stroke-opacity数字，从 0.0 到 1.0。0.0 是完全透明，1.0 是完全不透明(默认值)stroke-dasharray用一系列的数字来指定虚线和间隙的长度。这些数字只能使用用户坐标，默认值为 nonestroke-linecap线头尾的形状，值为 butt(默认值)、round 或 squarestroke-linejoin图形的棱角或者一系列连线的形状，取值为 miter(尖的，默认值)、round 或者 bevel(平的)stroke-miterlimit相交处显示宽度与线宽的最大比例，默认值为 4
b.填充特性
属性值fill填充颜色，默认值为 blackfill-opacity从 0.0 到 1.0 的数字，0.0 表示完全透明，1.0(默认值)表示完全不透明fill-rule属性值为 nonzero(默认值)或 evenodd。该属性决定判断某个点是否在图形内部的方法。只有当边线交叉时或者内部有“洞”时才有效
3.SVG的图形的实现
常见的 SVG 形状：矩形(rect)、圆形(circle)、椭圆形(ellipse)、线段(line)、折线(polyline)、多边形(polygon)
1.矩形(rect)：
a.x: 左上角x轴坐标y: 左上角y轴坐标width: 宽度height: 高度rx: 圆角，x轴的半径ry: 圆角，y轴的半径
b.效果：默认填充黑色。
c.
 **矩形-rect**
 
 `<svg` `width="310"` `height="110"` `xmlns="<http://www.w3.org/2000/svg>"` `version="1.1"` `style="border: 1px solid red">`
 
 `<rect` `width="300"` `height="100"` `rx="50"` `ry="50"` `x="10"` `y="10"></rect>`
 
 `</svg`
 
 ---

d.虽然可以使用 rect 画圆，但是有 circle 标签，专门画圆
2.圆形(circle)：
a.cx: 圆心在x轴的坐标cy: 圆心在y轴的坐标r: 半径
b.
 **矩形-rect**
 
 `<svg` `width="100"` `height="100"` `xmlns="<http://www.w3.org/2000/svg>"` `version="1.1"` `style="border: 1px solid red">`
 
 `<circle` `cx="50"` `cy="50"` `r="30"></circle>`
 
 `</svg>`
 
 ---

3.椭圆形(ellipse)：
a.cx: 圆心在x轴的坐标cy: 圆心在y轴的坐标rx: x轴的半径ry: y轴的半径
b.
 **矩形-rect**
 
 `<svg` `width="200"` `height="80"` `xmlns="<http://www.w3.org/2000/svg>"` `version="1.1"` `style="border: 1px solid red">`
 
 `<ellipse` `cx="100"` `cy="40"` `rx="80"` `ry="30"></ellipse>`
 
 `</svg>`
 
 ---

4.线段(line)：
a.x1: 起始点x坐标y1: 起始点y坐标x2: 结束点x坐标y2: 结束点y坐标stroke: 颜色（不加颜色显示不出来）
b.
 **矩形-rect**
 
 `<svg` `width="200"` `height="200"` `style="border: 1px solid red;">`
 
 `<line` `x1="10"` `y1="10"` `x2="190"` `y2="190"` `stroke="#000000"></line>`
 
 `</svg>`
 
 ---

5.折线(polyline)：
a.points: 点集（两两一组，代表xy，中间可以用“,”分开，便于读写）stroke: 描边颜色fill: 填充颜色，不写none的话，会默认填充
b.
 **矩形-rect**
 
 `<svg` `width="100"` `height="100"` `style="border: 1px solid red;">`
 
 `<polyline` `points="10 10, 40 80, 100 100"` `stroke="#000000"` `fill="none"></polyline>`
 
 `</svg>`
 
 ---

6.多边形(polygon)：
a.points: 点集stroke: 描边颜色fill: 填充颜色
b.
 **矩形-rect**
 
 `<svg` `width="100"` `height="100"` `style="border: 1px solid red;">`
 
 `<polygon` `points="10 10, 40 80, 100 100"` `fill="none"` `stroke="#000000"></polygon>`
 
 `</svg>`
 
 ---

c.和折线不一样的地方就是，多边形是首尾闭合的
7.路径(Path):
a.重点标签，所有图形都是可以由Path 绘制。它最主要的是d 属性
指令参数名称描述Mx,y"moveto移动到"移动虚拟画笔到指定的（x,y）坐标，仅移动不绘制。每个路径都必须以 M 开始。M 传入 x 和 y 坐标，用逗号或者空格隔开。Lx,y"lineto连直线到"从当前画笔所在位置绘制一条直线到指定的（x,y）坐标Hx"horizontal lineto水平连线到"绘制一条水平直线到参数指定的x坐标点，y坐标为画笔的y坐标Vy"vertical lineto垂直连线到"从当前位置绘制一条垂直直线到参数指定的y坐标Cx1,y1 x2,y2 x,y"curveto三次方贝塞尔曲线"从当前画笔位置绘制一条三次贝兹曲线到参数（x,y）指定的坐标。x1，y1和x2,y2是曲线的开始和结束控制点，用于控制曲线的弧度Sx2,y2 x,y"shorthand平滑三次方贝塞尔曲线"从当前画笔位置绘制一条三次贝塞尔曲线到参数（x,y）指定的坐标。x2,y2是结束控制点。开始控制点和前一条曲线的结束控制点相同Qx1,y1 x,y二次方贝塞尔曲线从当前画笔位置绘制一条二次方贝塞尔曲线到参数（x,y）指定的坐标。x1,y1是控制点，用于控制曲线的弧度Tx,y平滑的二次贝塞尔曲线从当前画笔位置绘制一条二次贝塞尔曲线到参数（x,y）指定的坐标。控制点被假定为最后一次使用的控制点Arx,ry x-axis-rotation large-arc-flag,sweepflag x,y椭圆弧线从当前画笔位置开始绘制一条椭圆弧线到（x,y）指定的坐标。rx和ry分别为椭圆弧线水平和垂直方向上的半径。x-axis-rotation指定弧线绕x轴旋转的度数。它只在rx和ry的值不相同是有效果。large-arc-flag是大弧标志位，取值0或1，用于决定绘制大弧还是小弧。sweep-flag用于决定弧线绘制的方向Z无闭合路径从结束点绘制一条直线到开始点，闭合路径
b.例子1:
1.
 **矩形-rect**
 
 `<svg` `width="100"` `height="100"` `xmlns="<http://www.w3.org/2000/svg>"` `version="1.1"` `style="border: 1px solid red">`
 
 `<path` `d="M 0 0 L 30 30 L 60 0 L 100 30 Z"` `stroke="#000000"` `fill="none"></path>`
 
 `//<path` `d="M 0 0 30 30 60 0 100 30 Z"` `stroke="#000000"` `fill="none"></path>`
 
 `</svg>`
 
 ---
 
 Z 就是闭合，如果全是L描述点点位置，可以去掉简写。

i.大写L是绝对位置， 小些l 是相对位置（相对于上一个点的位置）
c.例子2:
1.
 **矩形-rect**
 
 `<svg` `width="100"` `height="100"` `xmlns="<http://www.w3.org/2000/svg>"` `version="1.1"` `style="border: 1px solid red">`
 
 `<path` `d="M 10 10 H 90 V 90 H 10 Z"` `stroke="#000000"` `fill="none">`
 
 `</svg>`
 
 ---
 
 H 是绝对位置，h相对位置， V也一样。 H代表水平线，V是垂直线。

d.例子3:
1.
2. 
矩形-rect
 `<svg` `width="100"` `height="100"` `xmlns="<http://www.w3.org/2000/svg>"` `version="1.1"` `style="border: 1px solid red">`
 
 `<path` `d="M 10 10 C 70 40 70 70 10 90"` `stroke="#000000"` `fill="none"></path>`
 
 `</svg>`
 
 ---

e.例子4:
i.A(rx, ry, xr, laf, sf, x, y)，
- rx: 椭圆X轴半径
- ry: 椭圆Y轴半径
- xr: 椭圆旋转角度
- laf: 是否选择弧长较长的那一段。0: 短边（小于180度）; 1: 长边（大于等于180度）
- sf: 是否顺时针绘制。0: 逆时针; 1: 顺时针
- x: 终点X轴坐标
- y: 终点Y轴坐标


!https://wiki.jiduauto.com/download/thumbnails/734335142/image2023-9-27_16-35-3.png?version=1&modificationDate=1695803704000&api=v2
ii.矩形-rect
<svg width="300" height="120" xmlns="<http://www.w3.org/2000/svg>" version="1.1" style="border: 1px solid red">
<path d="M 50 70 A 100 40 0 1 1 200 100" stroke="#000000" fill="none" />
</svg>
8.图片(image):
1.2. 矩形-rect
 `<svg` `width="200"` `height="118"` `style="border: 1px solid red;">`
 
 `<image` `width="200"` `xlink:href="<https://static.jiduapp.cn/optimus/user-upload/c8d35140bb.png>"></image>`
 
 `</svg>`
 
 ---

9.文本(text):
1.
 **矩形-rect**
 
 `<svg` `width="200"` `height="120"` `xmlns="<http://www.w3.org/2000/svg>"` `version="1.1"` `style="border: 1px solid red">`
 
 `<text` `x="10"` `y="20"` `fill="#000000"` `font-size="20"` `font-weight="bold">Hello world</text>`
 
 `<text` `fill="#555555">`
 
 `<tspan` `x="10"` `y="40"` `>hello</tspan>`
 
 `<tspan` `x="10"` `y="70"` `>world</tspan>`
 
 `</text>`
 
 `<a` `xlink:href="[<https://www.jiyue-auto.com>](<https://www.jiyue-auto.com/>)"` `xlink:title="极越官网"` `target="_blank">`
 
 `<text` `x="10"` `y="100">极越官网</text>`
 
 `</a>`
 
 `</svg>`
 
 ---

4.SVG 其他标签使用
参考网站
1.<g> 分组
a.g 是'group'的简写，它能把多个元素放在一组里，对 g 标记实施的样式和渲染会作用到这个分组内的所有元素上。组内的所有元素都会继承 g 标记上的所有属性。用定义的分组还可以使用 use 进行复制使用。
b.
c.矩形-rect
<svg width="200" height="100" viewBox="0 0 200 100">
<g fill="pink" id="myClip">
<circle cx="30" cy="30" r="20"/>
<circle cx="100" cy="70" r="30"/>
</g>
</svg>
2.<use> 深度复用
a.use 标记的作用是能从 SVG 文档内部取出一个节点，克隆它，并把它输出到别处。跟‘引用’很相似，但它是深度克隆。
b.
c.矩形-rect
<svg width="200" height="200" viewBox='0 0 60 60'>
<defs>
<g id="Port">
<circle style="fill:inherit" r="5"/>
</g>
</defs>
<use x="10" y="10" xlink:href="#Port" style="fill:orange" />
<use x="30" y="10" xlink:href="#Port" style="fill:pink"/>
<use x="50" y="10" xlink:href="#Port" style="fill:green"/>
</svg>
3.<defs> 模版
a.defs 元素用于预定义一个元素使其能够在 SVG 图像中重复使用，和 g 结合使用。
b.
c.矩形-rect
<svg width="300" height="300" viewport="0 0 300 300">
<defs>
<g id="shape">
<rect x="25" y="50" width="25" height="25" />
<circle fill="pink" cx="25" cy="50" r="25" />
<circle fill="orange" cx="25" cy="50" r="5" />
</g>
</defs>
<use xlink:href="#shape" x="50" y="25" />
<use xlink:href="#shape" x="150" y="25" />
<use xlink:href="#shape" x="50" y="100" />
<use xlink:href="#shape" x="150" y="100" />
</svg>
4.<symbol> 模版
a.symbol 标记的作用是定义一个图像模板，相当于 g 和 defs 的结合，可以使用 use 标记实例化它，然后在 SVG 文档中反复使用，这种用法非常的高效。symbol 本身不会输出任何图像，只有使用 use 实例化后才会显示。
b.
c.矩形-rect
<svg viewBox="0 0 150 150" height="300">
<symbol id="sym01" viewBox="0 0 150 110">
<circle cx="50" cy="50" r="40" stroke-width="8" stroke="#2ECC71" fill="#A3E4D7" />
<circle cx="90" cy="60" r="40" stroke-width="8" stroke="pink" fill="white" />
</symbol>
<use xlink:href="#sym01" x="0" y="0" width="100" height="50" />
<use xlink:href="#sym01" x="0" y="50" width="75" height="38" />
<use xlink:href="#sym01" x="0" y="100" width="50" height="25" />
</svg>
5.<clipPath> 裁剪
1.
2. 
矩形-rect
 `<svg` `width="120"` `height="120"` `viewPort="0 0 120 120"` `version="1.1"` `xmlns="<http://www.w3.org/2000/svg>">`
 
 `<defs>`
 
 `<clipPath` `id="myClip">`
 
 `<circle` `cx="30"` `cy="30"` `r="20"` `/>`
 
 `<circle` `cx="100"` `cy="70"` `r="30"` `/>`
 
 `</clipPath>`
 
 `</defs>`
 
 `<rect` `x="10"` `y="10"` `width="100"` `height="100"` `clip-path="url(#myClip)"` `fill="pink"` `/>`
 
 `</svg>`
 
 ---

