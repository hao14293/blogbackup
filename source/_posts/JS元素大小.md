---
title: JS元素大小
categories: web
abbrlink: 28779
date: 2018-08-08 22:01:04
tags:
---
> 很多行为用到了，很重要。

## 偏移量(offset dimension)
元素的可见大小由其高度、宽度决定，包括所有内边距、滚动条和边框大小（不包括外边距）。通过下列4个属性可以取得元素的偏移量.

<!--more-->
* offsetHeight: 元素在垂直方向上占用的空间大小，以像素计。包括元素的高度、（可见的）水平滚动条的高度、上边框高度和下边框高度。
* offsetWidth: 元素在水平方向上占用的空间大小，，以像素计。包括元素的宽度、（可见的）垂直滚动条的宽度、左边框宽度和右边框宽度。
* offsetLeft: 元素的左外边框至包含元素的左内边框之间的像素距离。
* offsetTop: 元素的上外边框至包含元素的上内边框之间的像素距离。

其中，offsetLeft 和 offsetTop 属性与包含元素有关。包含元素的引用保存在 offsetParent属性中。offsetParent属性不一定与parentNode的值相等。<br>
通过几个实例测试来理解一下:
```javascript

<style>
        *{
            margin: 0;
            padding: 0;
        }
        body{
            width: 100%;
            height: 100%;
        }
        #myDiv{
            width: 500px;
            height: 800px;
            margin: 0 auto;
            background: #ebebeb;
        }
        }
</style>
<body>
	<div id="myDiv" ></div>
    <script>
    var element = document.getElementById('myDiv');

    console.log(element.offsetHeight);
    console.log(element.offsetHeight);
    console.log(element.offsetLeft);
    console.log(element.offsetTop);

    </script>
</body>
```
    

![1.png](https://i.loli.net/2018/08/08/5b6afeac718a7.png)
<code>element.offsetHeight</code> 和 <code>element.offsetHeight</code> 很好理解，就是元素的宽高，<code>element.offsetLeft</code>和<code>element.offsetTop</code>就是元素的外边框到父元素的内边框之间的距离。
> 所有这些偏移量属性都是只读的。

## 客户区大小(client dimension)
元素的客户区大小，指的是<b>元素内容<span style="color:red">及其内边距</span></b>所占据的空间大小。有关属性有两个: <code>clientWidth</code>和  <code>clientHeight</code>。这个就很好理解了。

## 滚动大小 (scroll dimension)
滚动大小，指的是<b>包含滚动内容的元素</b>的大小。有些元素（例如 html），即使没有执行任何代码也能自动地添加滚动条;但另外一些元素，则需要通过 CSS 的 overflow属性进行设置才能滚动。以下是四个与滚动大小相关的属性。
* scrollHeight: 在没有滚动条的情况下，元素内容的总高度。
* scrollWidth: 在没有滚动条的情况下，元素内容的总宽度。
* scrollLeft: 被隐藏在内容区域左侧的像素数。通过设置这个属性可以改变元素的滚动位置。
* scrollTop: 被隐藏在内容区域上方的像素数。通过设置这个属性可以改变元素的滚动位置。

scrollWidth和 scrollHeight 主要用于确定元素内容的实际大小。例如，通常认为 html 元素是在 Web 浏览器的视口中滚动的元素。因此，带有垂直滚动条的页面总高度就是 <code>document.documentElement.scrollHeight</code>

> 测试发现， scrollHeight、scrollWidth和 clientHeight、clientWidth 相等。

在确定文档的总高度时，必须取得scrollWidth/clientWidth 和 scrollHeight/clientHeight中的最大值，才能保证在跨浏览器的环境下得到精确的结果。
```javascript
var docHeight = Math.max(document.documentElement.scrollHeight,
                            document.documentElement.clientHeight);

var docWidth = Math.max(document.documentElement.scrollWidth,
                            document.documentElement.clientWidth);
 ```
 通过scrollLeft 和 scrollTop 属性既可以确定元素当前滚动的状态，也可以设置元素的滚动位置。在元素尚未滚动时，这两个属性的值都等于0.如果元素被垂直滚动了，那么 scrollTop的值会大于0,且表示元素上方不可见内容的像素高度。scrollLeft同理。这两个属性都是可以设置的，因此将元素的 scrollLeft和scrollTop设为0 ，就可以重置元素的滚动位置。
 
 > 这几个属性在很多地方都会用到。
 
 