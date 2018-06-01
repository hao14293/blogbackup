---
title: CSS总结
tags: css
categories: web
abbrlink: 31424
date: 2018-04-19 18:45:21
---
CSS属性繁多，有些属性用的时候总想不起来，所以就写个记录贴，这样便于记忆和复习。
***
### 阴影（shadow)
<code>shadow</code>有<code>text-shadow(文本阴影)</code>和<code>box-shadow(框阴影)</code>两种.<br>
<b>语法</b>
```sh
text-shadow: h-shadow v-shadow blur color;
box-shadow: h-shadow v-shadow blur spread color inset;
```
<!--more-->
通过设置<code>shadow属性</code>可以实现很多效果，比如我将导航栏设置为鼠标移动到上面时多出来6px不同颜色的高度。<br>
代码如下：
```sh
a:link {
  display: inline-block;
  height: 90%;
  width: 18%;
  position: relative;
  background-color: #DAD9CC;
}
a: hover {
  box-shadow: 0px -8px 0px # 858471;
}
```
***
### background: transparent
经常会看到这样的代码
```sh
* {
  background: transparent;
}
```
意思就是背景透明。实际上background默认的颜色就是透明的属性，所以写和不写都是一样的。但如果一个元素覆盖在另一个元素之上，而你想显示下面的元素，这时你就需要把上面的这个元素设置为<code>background: transparent;</code>.
***
### z-index
首先声明：<p style="color:red"> z-index 仅能在定位元素上奏效，即只能在position属性值为relative、absolute或fixed的元素上有效。</p>
基本原理是：<b>z-index 属性设置元素的堆叠顺序。拥有更高堆叠顺序的元素总是会处于堆叠顺序较低的元素的前面。</b><br>
<b>元素可拥有负的 z-index 属性值。</b>
***
### width: auto 和 width: 100% 区别
```sh
width: auto
* 子元素（包括content+padding+border+margin）撑满整个父元素的content区域。
* 子元素有margin、border、padding时，会减去子元素content区域相对应的width值
* 父元素的content = 子元素（content + padding + border + margin )


width: 100%
* 强制将子元素的content区域 撑满 父元素的content区域
* 子元素有margin、border、padding时，不改变子元素content区域的width，而是溢出父盒子，保持原有值
* 父元素的content = 子元素的content```

来看个例子
```sh
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style type="text/css">
        * {
            margin: 0;padding: 0;
        }
        body {
            background: #dcdcdc;
        }
        .box {
            width: 400px;
            border: 3px solid red;
            padding: 0 50px;
        }
        .box1 {
            width: auto;
            height: 100px;
            background: pink;
            padding: 0 50px;
            margin: 0 50px;
            border-width: 0 50px;
            border-style: solid;
            border-color: green;
        }
        .box2 {
            width: 100%;
            height: 100px;
            background: gold;
            padding: 0 50px;
            margin: 0 50px;
            border-width: 0 50px;
            border-style: solid;
            border-color: green;
        }

    </style>
</head>
<body>
<div class="box">
    <div class="box1"></div>
    <div class="box2"></div>
</div>
</body>
</html>```
![19.png](https://i.loli.net/2018/04/16/5ad4b2ee34ac4.png)
***
### text-transform
<b>text-transform 属性控制文本的大小写。</b>

| 值 | 描述 |
|:---|:----|
| none | 默认。定义带有小写字母和大写字母的标准的文本. |
| capitalize | 文本中的每个单词以大写字母开头。 |
| uppercase | 定义仅有大写字母。 |
| lowercase | 定义无大写字母，仅有小写字母。 |
| inherit |  规定应该从父元素继承 text-transform 属性的值。 |

*** 
### background-size
<b>语法:</b>
```sh
background-size: length|percentage|cover|contain;
```

| 值 | 描述 |
|:---|:---|
| length | 设置背景图像的高度和宽度。第一个值设置宽度，第二个值设置高度。如果只设置一个值，则第二个值会被设置为 "auto"。 |
| percentage | 以父元素的百分比来设置背景图像的宽度和高度。第一个值设置宽度，第二个值设置高度。如果只设置一个值，则第二个值会被设置为 "auto"。 |
| cover | 把背景图像扩展至足够大，以使背景图像完全覆盖背景区域。背景图像的某些部分也许无法显示在背景定位区域中。 | 
| contain | 把图像图像扩展至最大尺寸，以使其宽度和高度完全适应内容区域。 | 

*** 
### opacity
<b>opacity 属性设置元素的不透明级别。</b>
```sh
语法
opacity: value|inherit;
```

| 值 | 描述 | 
|:----|:----|
| value | 规定不透明度。从 0.0 （完全透明）到 1.0（完全不透明）。 | 
| inherit | 应该从父元素继承 opacity 属性的值。 | 

*** 

### &lt;video>属性

| 属性 | 值 | 描述 | 
|:-----|:----|:-----|
| autoplay | autoplay | 如果出现该属性，则视频在就绪后马上播放。 | 
| controls | controls | 如果出现该属性，则向用户显示控件，比如播放按钮。 | 
| height  | pixels | 设置视频播放器的高度。 | 
| loop | loop | 如果出现该属性，则当媒介文件完成播放后再次开始播放。 | 
| muted | muted  |规定视频的音频输出应该被静音。 | 
| poster | URL | 规定视频下载时显示的图像，或者在用户点击播放按钮前显示的图像. | 
| preload | preload | 如果出现该属性，则视频在页面加载时进行加载，并预备播放。如果使用 "autoplay"，则忽略该属性。 | 
| src | url | 要播放的视频的 URL。 | 
| width | pixels | 设置视频播放器的宽度。 | 

*** 
### cursor
<b>cursor 属性规定要显示的光标的类型(形状)。</b>

| 值 | 描述 |
|:---|:-----|
| url | 需使用的自定义光标的 URL。注释:请在此列表的末端始终定义一种普通的光标，以防没有由 URL 定义的可用光标 |
| default | 默认光标（通常是一个箭头） |
| auto | 默认。浏览器设置的光标。 |
| crosshair | 光标呈现为十字线。 |
| pointer | 光标呈现为指示链接的指针（一只手） |
| move | 此光标指示某对象可被移动。 |
| e-resize | 此光标指示矩形框的边缘可被向右（东）移动。 |
| ne-resize | 此光标指示矩形框的边缘可被向上及向右移动（北/东）。 | 
| nw-resize | 此光标指示矩形框的边缘可被向上及向左移动（北/西）。 |
| n-resize | 此光标指示矩形框的边缘可被向上（北）移动。 |
| se-resize | 此光标指示矩形框的边缘可被向下及向右移动（南/东）。 |
| sw-resize | 此光标指示矩形框的边缘可被向下及向左移动（南/西）。 |
| s-resize | 此光标指示矩形框的边缘可被向下移动（南）。 |
| w-resize | 此光标指示矩形框的边缘可被向左移动（西）。 | 
| text | 此光标指示文本。 |
| wait | 此光标指示程序正忙（通常是一只表或沙漏）。 |
| help | 此光标指示可用的帮助（通常是一个问号或一个气球）。 | 
*** 
### transform
<b>transform 属性向元素应用 2D 或 3D 转换。该属性允许我们对元素进行旋转、缩放、移动或倾斜。</b>

| 值 | 描述 |
|:---|:-----|
| none | 定义不进行转换。 |
| matrix(n,n,n,n,n,n) | 定义 2D 转换，使用六个值的矩阵。 |
| matrix3d(n,n,n,n,n,n,n,n,n,n,n,n,n,n,n,n) | 定义 3D 转换，使用 16 个值的 4x4 矩阵。 |
| translate(x,y) | 定义 2D 转换。 | 
| translate3d(x,y,z) | 定义 3D 转换。 |
| translateX(x) | 定义转换，只是用 X 轴的值。 |
| translateY(y) | 定义转换，只是用 Y 轴的值。 |
| translateZ(z) | 定义 3D 转换，只是用 Z 轴的值。 |
| scale(x,y) | 定义 2D 缩放转换。 |
| scale3d(x,y,z) | 定义 3D 缩放转换。 |
| scaleX(x) | 通过设置 X 轴的值来定义缩放转换。 |
| scaleY(y) | 通过设置 Y 轴的值来定义缩放转换。 |
| scaleZ(z) | 通过设置 Z 轴的值来定义 3D 缩放转换。 |
| rotate(angle) | 定义 2D 旋转，在参数中规定角度。 |
| rotate3d(x,y,z,angle) |  定义 3D 旋转。 |
| rotateX(angle) | 定义沿着 X 轴的 3D 旋转。 |
| rotateY(angle) | 定义沿着 Y 轴的 3D 旋转。 |
| rotateZ(angle) | 定义沿着 Z 轴的 3D 旋转。 |
| skew(x-angle,y-angle) | 定义沿着 X 和 Y 轴的 2D 倾斜转换。 | 
| skewX(angle) | 定义沿着 X 轴的 2D 倾斜转换。 |
| skewY(angle) | 定义沿着 Y 轴的 2D 倾斜转换。 |
| perspective(n) | 为 3D 转换元素定义透视视图。 |

*** 
### animation
animation 属性是一个简写属性，用于设置六个动画属性：

* animation-name
* animation-duration
* animation-timing-function
* animation-delay
* animation-iteration-count
* animation-direction


<b>注释：请始终规定 animation-duration 属性，否则时长为 0，就不会播放动画了。</b>
```sh
语法
animation: name duration timing-function delay iteration-count direction;
```
| 值 | 描述 |
|:---|:-----| 
| animation-name | 规定需要绑定到选择器的 keyframe 名称。(keyframename/none) |
| animation-duration | 规定完成动画所花费的时间，以秒或毫秒计。 |
| animation-timing-function | 规定动画的速度曲线。 |
| animation-delay | 规定在动画开始之前的延迟。 |
| animation-iteration-count | 	规定动画应该播放的次数。(n/infinite) |
| animation-direction | 规定是否应该轮流反向播放动画。(normal/alternate) |

<code>animation-timing-function</code>

| 值 | 描述 |
|:---|:-----|
| linear | 动画从头到尾的速度是相同的。 |
| ease | 默认。动画以低速开始，然后加快，在结束前变慢。 |
| ease-in | 动画以低速开始。 |
| ease-out | 动画以低速结束。 |
| ease-in-out | 动画以低速开始和结束。 |
| cubic-bezier(n,n,n,n) | 在 cubic-bezier 函数中自己的值。可能的值是从 0 到 1 的数值。 | 

<code>cubic-bezier</code>即为贝塞尔曲线中的绘制方法。图上有四点，<code>P0-3</code>，其中<code>P0、P3</code>是默认的点，对应了<code>[0,0], [1,1]</code>。而剩下的P1、P2两点则是我们通过<code>cubic-bezier()</code>自定义的。<code>cubic-bezier(x1, y1, x2, y2)</code> 为自定义，<code>x1,x2,y1,y2</code>的值范围在[0, 1]。

预留的几个特效：
```sh
ease: cubic-bezier(0.25, 0.1, 0.25, 1.0)

linear: cubic-bezier(0.0, 0.0, 1.0, 1.0)

ease-in: cubic-bezier(0.42, 0, 1.0, 1.0)

ease-out: cubic-bezier(0, 0, 0.58, 1.0)

ease-in-out: cubic-bezier(0.42, 0, 0.58, 1.0)
```

*** 
### CSS选择器
在 CSS 中，选择器是一种模式，用于选择需要添加样式的元素。

| 选择器 | 例子 | 例子描述 |
|:-------|:-----|:---------|
| .class | .intro | 选择 class="intro" 的所有元素。 |
| #id | #firstname | 选择 id="firstname" 的所有元素。 | 
| * | * | 选择所有元素 |
| element | p | 选择所有 &lt;p> 元素。 |
| element,element | div,p | 选择所有&lt;div> 元素和所有 &lt;p> 元素。 |
| element element | div p | 选择 &lt;div> 元素内部的所有 p&lt;> 元素。 |
| element>element | div>p | 选择父元素为 &lt;div> 元素的所有 &lt;p> 元素。 |
| element+element | div+p | 选择紧接在 &lt;div> 元素之后的所有&lt;p> 元素。 |
| [attribute] | [target] | 选择带有 target 属性所有元素。 |
| [attribute=value] | [target=_blank] | 选择 target="_blank" 的所有元素。 |
| [attribute]~=value] | [titlr~=flower] | 选择 title 属性包含单词 "flower" 的所有元素。  |
| [attribute]=value] | [lang]=en] | 选择 lang 属性值以 "en" 开头的所有元素。 |
| :link | a:link | 选择所有未被访问的链接。 |
| :visited | a:visited | 选择所有已被访问的链接。 |
| :active | a:active | 选择活动链接。 |
| :hover | a:hover | 选择鼠标指针位于其上的链接。 |
| :focus | input:focus | 选择获得焦点的 input 元素。 |
| :first-letter | p:first-letter | 选择每个 &lt;p> 元素的首字母。 |
| :first-line | p:first-line | 选择每个 &lt;p> 元素的首行。 |
| :first-child | p:first-child | 选择属于父元素的第一个子元素的每个 &lt;p> 元素。 |
| :before | p:before | 在每个 &lt;p> 元素的内容之前插入内容。 |
| :after | p:after | 在每个 &lt;p> 元素的内容之后插入内容。 |
| :lang(language) | p:lang(it) | 选择带有以 "it" 开头的 lang 属性值的每个 &lt;p> 元素。 |
| element1~element2 | p~ul | 选择前面有 &lt;p> 元素的每个 &lt;ul> 元素。 |
| [attribute^=value] | a[src^="https"] | 选择其 src 属性值以 "https" 开头的每个 &lt;a> 元素。 |
| [attribute$=value] | a[src$=".pdf"] | 选择其 src 属性以 ".pdf" 结尾的所有 &lt;a> 元素。 |
| [attribute*=value] | a[src*="abc"] | 选择其 src 属性中包含 "abc" 子串的每个 &lt;a> 元素。	 |
| :first-of-type | p:first-of-type | 选择属于其父元素的首个 &lt;p> 元素的每个 &lt;p> 元素。 |
| :last-of-type | p:last-of-type | 选择属于其父元素的最后 &lt;p> 元素的每个 &lt;p> 元素。 |
| :only-of-type | p:only-of-type | 选择属于其父元素唯一的 &lt;p> 元素的每个 &lt;p> 元素。 |
| :only-child | p:only-child | 选择属于其父元素的唯一子元素的每个 &lt;p> 元素。 |
| :nth-child(n) | p:nth-child(2) | 选择属于其父元素的第二个子元素的每个 &lt;p> 元素。 |
| :nth-last-child(n) | p:nth-last-child(2) | 同上，从最后一个子元素开始计数。 |
| :nth-of-type(n) | p:nth-of-type(2) | 选择属于其父元素第二个 &lt;p> 元素的每个&lt;p> 元素。 |
| :nth-last-of-type(n) | p:nth-last-of-type(2) | 同上，但是从最后一个子元素开始计数。 |
| :last-child | p:last-child | 选择属于其父元素最后一个子元素每个 &lt;p> 元素。 |
| :root | :root | 选择文档的根元素。 |
| :empty | p:empty | 选择没有子元素的每个 &lt;p> 元素（包括文本节点）。 |
| :target | #news:target | 选择当前活动的 #news 元素。 |
| :enabled | input:enabled | 选择每个启用的 &lt;input> 元素。 |
| :disabled | input:disabled | 选择每个禁用的 &lt;input> 元素 |
| :checked | input:checked | 选择每个被选中的 &lt;input> 元素。 |
| :not(selector) | :not(p) | 选择非 &lt;p> 元素的每个元素。 |
| ::selection | ::selection | 选择被用户选取的元素部分。 |

*** 
### @keyframes 规则
通过 @keyframes 规则，您能够创建动画。

创建动画的原理是，将一套 CSS 样式逐渐变化为另一套样式。

在动画过程中，您能够多次改变这套 CSS 样式。

以百分比来规定改变发生的时间，或者通过关键词 "from" 和 "to"，等价于 0% 和 100%。

0% 是动画的开始时间，100% 动画的结束时间。

为了获得最佳的浏览器支持，您应该始终定义 0% 和 100% 选择器。

<b>注释</b>：请使用动画属性来控制动画的外观，同时将动画与选择器绑定。

| 值 | 描述 |
|:---|:-----|
| animationname | 必需。定义动画的名称。 |
| keyframes-selector | 必需。动画时长的百分比。合法的值：  0-100%;from（与 0% 相同;to（与 100% 相同） | 
| css-styles | 必需。一个或多个合法的 CSS 样式属性。 |

看个例子就明白了
```sh
<!DOCTYPE html>
<html>
<head>
<style> 
div
{
width:100px;
height:100px;
background:red;
position:relative;
animation:mymove 5s infinite;
-moz-animation:mymove 5s infinite; /* Firefox */
-webkit-animation:mymove 5s infinite; /* Safari and Chrome */
-o-animation:mymove 5s infinite; /* Opera */
}

@keyframes mymove
{
0%   {top:0px; left:0px; background:red;}
25%  {top:0px; left:100px; background:blue;}
50%  {top:100px; left:100px; background:yellow;}
75%  {top:100px; left:0px; background:green;}
100% {top:0px; left:0px; background:red;}
}

@-moz-keyframes mymove /* Firefox */
{
0%   {top:0px; left:0px; background:red;}
25%  {top:0px; left:100px; background:blue;}
50%  {top:100px; left:100px; background:yellow;}
75%  {top:100px; left:0px; background:green;}
100% {top:0px; left:0px; background:red;}
}

@-webkit-keyframes mymove /* Safari and Chrome */
{
0%   {top:0px; left:0px; background:red;}
25%  {top:0px; left:100px; background:blue;}
50%  {top:100px; left:100px; background:yellow;}
75%  {top:100px; left:0px; background:green;}
100% {top:0px; left:0px; background:red;}
}

@-o-keyframes mymove /* Opera */
{
0%   {top:0px; left:0px; background:red;}
25%  {top:0px; left:100px; background:blue;}
50%  {top:100px; left:100px; background:yellow;}
75%  {top:100px; left:0px; background:green;}
100% {top:0px; left:0px; background:red;}
}
</style>
</head>
<body>

<p><b>注释：</b>本例在 Internet Explorer 中无效。</p>

<div></div>

</body>
</html>
```
目前浏览器都不支持 @keyframes 规则。

Firefox 支持替代的 @-moz-keyframes 规则。

Opera 支持替代的 @-o-keyframes 规则。

Safari 和 Chrome 支持替代的 @-webkit-keyframes 规则。
*** 





