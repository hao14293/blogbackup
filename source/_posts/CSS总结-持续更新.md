---
title: CSS总结(持续更新)
tags: css
categories: web
abbrlink: 31424
date: 2018-04-17 01:50:21
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




