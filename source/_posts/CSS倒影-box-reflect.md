---
title: CSS倒影 box-reflect
categories: web
abbrlink: 8479
date: 2018-08-04 19:29:38
tags:
---
> CSS 图片倒影，还未包含进CSS3官方标准中。

#### 基本语法
```sh
语法：
box-reflect：none | <direction> <offset>? <mask-box-image>?

<direction> = above | below | left | right

<offset> = <length> | <percentage>

<mask-box-image> = none | <url> | <linear-gradient> | <radial-gradient> | <repeating-linear-gradient> | <repeating-radial-gradient>

默认值：none

适用于：所有元素

继承性：无

动画性：否

计算值：指定值

取值：
none：
无倒影
<direction>　Demo: 简单图片倒影 See with Webkit
above：
指定倒影在对象的上边
below：
指定倒影在对象的下边
left：
指定倒影在对象的左边
right：
指定倒影在对象的右边
<offset>　Demo: 图片与倒影间隔 See with Webkit
<length>：
用长度值来定义倒影与对象之间的间隔。可以为负值
<percentage>：
用百分比来定义倒影与对象之间的间隔。可以为负值
<mask-box-image>　Demo: 更真实的图片倒影 文字倒影与渐变 See with Webkit
none：
无遮罩图像
<url>：
使用绝对或相对地址指定遮罩图像。
<linear-gradient>：
使用线性渐变创建遮罩图像。
<radial-gradient>：
使用径向(放射性)渐变创建遮罩图像。
<repeating-linear-gradient>：
使用重复的线性渐变创建背遮罩像。
<repeating-radial-gradient>：
使用重复的径向(放射性)渐变创建遮罩图像。
说明：
设置或检索对象倒影。
假设定义了 <mask-box-image>，<offset>必须指定，否则可以省略
对应的脚本特性为boxReflect。
```

<!--more-->
##### 来看一下实际用法：
```sh
img{
	-webkit-box-reflect: blow;
}
```

![1.jpg](https://i.loli.net/2018/08/04/5b6592b892271.jpg)
上面这个例子中倒影出现了图片的下方，但实际上我们也可以将倒影安置在左侧、右侧或上侧。只需要相应地设置<code> above/right/left/blow</code>。

##### 倒影偏移量
Offset属性值用来定义图片和倒影影像之间的间距。参考下面的代码：
```sh
img{
	-webkit-box-reflect: blow 10px;
}
```
![2.jpg](https://i.loli.net/2018/08/04/5b6593a5d8f24.jpg)
##### 给倒影增加消隐效果
在现实生活中，倒影的出现通常是上半部比较清晰，下面半部逐渐消隐。为了在CSS中实现这种效果，我们需要运用CSS3渐变色(Gradients)功能，就像下面这样：
```sh
img{
	-webkit-box-reflect: below 0px -webkit-gradient(
    	linear,
        left top,
        left bottom,
        from(transparent),
        to( rgba(250,250,250,.1)
    );
}
```
这段代码就能达到这样的效果：

![3.jpg](https://i.loli.net/2018/08/04/5b6593a5cd52b.jpg)
我们还可以使用color-stop来控制色彩过渡，让倒影更加漂亮：

```sh
img{
	-webkit-box-reflect: below 0px -webkit-gradient(
    	linear,
        left top,
        left bottom,
        from(transparent),
        color-stop(70%, transparent),
        to(rgba(250,250,250,.1)
    );
}
```
![4.jpg](https://i.loli.net/2018/08/04/5b6593a5e62d4.jpg)
#### 火狐浏览器中倒影的实现
<p style="font-size:16px;color:red;">目前只有Webkit浏览器(谷歌浏览器和Safari浏览器)实现box-reflect属性。</p>
为了在火狐浏览器中也实现倒影功能，我们需要寻找另外的途径：使用-moz-element()方法。这个方法能够复制指定网页元素的内容。让我们来看看下面的例子：

我们把图片包裹着一个ID是someid的<div>里。

并且，为了存放倒影影像，我们将使用:before伪元素，就像下面：
```sh
#someid {
position: relative;
/* 给倒影留下空间 */
margin-bottom: 120px;
}
#someid:before {
content:""; /* needed or nothing will be shown */
background: -moz-linear-gradient(top, white, white 30%, rgba(255,255,255,0.9) 65%, rgba(255,255,255,0.7)) 0px 0px,
-moz-element(#someid) 0px -127px no-repeat;
-moz-transform: scaleY(-1); /* flip the image vertically */
position:relative;
height:140px;
width: 360px; /* 需要 > image width + margin + shadow */
top: 247px;
left:0px;
}
```
这里的<code>-moz-transform</code>是一个负值，作用就是让复制过来的图形上下颠倒，达到倒影的效果。为了让<code>:before</code>伪元素跟原始图形相配合，我们需要移动它的位置。这里的背景偏移量 (-127px)是<code>:before</code>伪元素高，<code>(140px) – (图片的高 (247px) + div的border (20px))</code>。需要注意的是，火狐浏览器版的倒影实现只能用在页面的背景是真实背景。背景色要和<code>:before</code>伪元素使用的渐变色的颜色一致。 因为所有的属性都是来实现倒影的，而且这些属性都有火狐浏览器独有的前缀，和<code>-Webkit</code>的倒影不冲突，所以在代码在可以把两个版本倒影方法都写上，保证两种浏览器里都有效果。



