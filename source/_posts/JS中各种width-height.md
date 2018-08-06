---
title: JS中各种width/height
categories: web
abbrlink: 25026
date: 2018-08-03 16:35:51
tags:
---
> 对于初学者来说，搞明白这些概念真的痛苦。

#### 窗口位置
IE、Safari、Opera 和 Chrome 都提供了 <code>screenLeft</code> 和 <code>screenTop</code>属性，分别用于表示窗口相对于屏幕左边和上边的位置。 Firefox 则在 <code>screenX</code> 和 <code>screenY</code> 属性中提供了相同的窗口位置信息， Safari 和 Chrome 也同时支持这两种属性。Opera 虽然也支持 <code>screenX</code> 和 <code>screenY</code> ，但与 <code>screenLeft</code> 和 <code>screenTop</code>属性并不对应，因此建议大家不要在Opera 中使用它们。 使用下列代码可以跨浏览器取得窗口左边和上边的位置。

<!--more-->
```sh
var leftPos = (typeof window.screenLeft == "number") ? window.screenLeft : window.screenX;
var topPos = (typeof window.screenTop == "number") ? window.screenTop : window.screenY;
```

<hr>
嗯，书上就这么写的，什么意思呢？ 下面实际测试一下。

```sh
# Chrome 浏览器

var leftPos = (typeof window.screenLeft == "number") ? window.screenLeft : window.screenX;
var topPos = (typeof window.screenTop == "number") ? window.screenTop : window.screenY;
console.log(leftPos);
console.log(topPos);
```
结果如图
![1.png](https://i.loli.net/2018/08/03/5b64177ca1f1a.png)
这个时候我浏览器是全屏的，我是Ubuntu 系统，和Mac系统一样，屏幕顶部会有状态条，所以 <code>0 24</code>就是指当前浏览器窗口最左边和最上边距离电脑屏幕左边框的上边框的距离。<br>那，浏览器不全屏呢？
![2.png](https://i.loli.net/2018/08/03/5b6418b23ffe0.png)
这时候浏览器没有全屏， 结果就变了，所以 <code>screenLeft</code> 和 <code>screenTop</code>属性就是当前浏览器窗口最左边和最上边距离电脑屏幕左边框的上边框的距离。
<hr>
在使用这些值的时候，还必须注意一些小问题。在 IE、Opera中，<code>screenLeft</code> 和 <code>screenTop</code>中保存的是从屏幕左边和上边到由 window 对象表示的页面可见区域的距离。 但是在 Chrome、Firefox和 Safari中， <code>screenY</code>或<code>screenTop</code>中保存的是整个浏览器窗口相对于屏幕的坐标值，即在窗口的y轴坐标为0时返回0.<br>

更让人捉摸不透是，Firefox、Safari和Chrome 始终返回页面中每个框架的 <code>top.screenX</code> 和 <code>top.screenY</code> 值。即使在页面由于被设置了外边距而发生偏移的情况下，相对于 <code>window</code> 对象使用 <code>screenX</code> 和 <code>screenY</code> 每次也都会返回相同的值。而IE 和 Opera 则会给出框架相对于屏幕边界的精确坐标值。
<hr>
#### 窗口大小
接下来进入重点啦。<br>
IE9+、Firefox、Safari、Opera 和 Chrome 均提供了4个属性： <code>innerwidth</code>、<code>innerHeight</code>、<code>outerWidth</code>、<code>outerHeight</code>。在IE9+、Safari、Firefox中，<code>outerWidth</code>、<code>outerHeight</code>返回浏览器窗口本身的尺寸（无论是从最外层的 window 对象还是从某个框架访问）。在 Opera 中，这两个属性的值表示页面视图容器的大小。 而<code>innerwidth</code>、<code>innerHeight</code>则表示该容器中页面视图区的大小（减去边框宽度）。在Chrome 中，<code>outerWidth</code>、<code>outerHeight</code>与 <code>innerwidth</code>、<code>innerHeight</code>返回相同的值，即视口大小而非浏览器窗口大小。<br>
在IE、Firefox、Safari、Opera 和 Chrome 中， <code>document.documentElement.clientWidth</code> 和 <code>document.documentElement.clientHeight</code> 中保存了页面视口的信息。在IE6中，这些属性必须在标准模式下才有效;如果是混杂模式，就必须通过 <code>document.body.clientWidth</code>和<code>document.body.clientHeight</code>取得相同信息。而对于混杂模式下的Chrome，则无论通过 <code>document.documentElement</code> 还是  <code>document.body</code>中的 <code>clientWidth</code>和<code>clientHeight</code>属性，都可以取得视口的大小。<br>
虽然无法确定浏览器窗口本身的大小，但却可以取得页面视口的大小
```sh
var pageWidth = window.innerWidth,
    pageHeight = window.innerHeight;
    
if (typeof pageWidth != "number"){
    if (document.compatMode == "CSS!Compat"){
    	pageWidth = document.documentElement.clientWidth;
        pageHeight = docuement.documentElement.clientHeight;
    } else {
    	pageWidth = document.body.clientWidth;
        pageHeight = document.body.clientHeight;
    }
}
```
<hr>
好啦，看完一脸蒙逼。。。实际测试以下吧。
```sh
# Chrome 先看各种 width
console.log(window.innerWidth);
console.log(window.outerWidth);
console.log(document.documentElement.clientWidth);
console.log(document.body.clientWidth);
```
结果
![3.png](https://i.loli.net/2018/08/03/5b6421ae4ebd2.png)
再看看不是全屏状态
![4.png](https://i.loli.net/2018/08/03/5b642281539f7.png)
<code>document.documentElement.clientWidth</code>与<code>document.body.clientWidth</code>始终相等，都比<code>window</code>少一个滚动条宽度。但是 <code>window.innerWidth</code>与  <code>window.outerWidth</code>为什么不相同了呢？
<br>
我们再看一种情况，我把控制台放在右侧(刚才两种情况都在底部)，调整控制台宽度，再看一下
![5.png](https://i.loli.net/2018/08/03/5b64241fd2807.png)
这下应该能看明白了吧，<p style="color:red;font-size=16px;"><code>innerWidth</code>是<b>指可视窗口宽度</b>，<code>outerWidth</code><b>指浏览器窗口宽度</b>。<code>document.documentElement.clientWidth</code>与<code>document.body.clientWidth</code><b>指可视窗口宽度减去滚动条宽度。</b><br></p>
那刚才控制台在底部时，<code>window.innerWidth</code>与  <code>window.outerWidth</code>为什么不相同了呢？这是因为浏览器没有全屏时左右会有边框，这一部分不是可视窗口。
<hr>
那 <code>Height</code>呢？测试一下。
```sh
	# chrome
	console.log(window.innerHeight);
    console.log(window.outerHeight);
    console.log(document.documentElement.clientHeight);
    console.log(document.body.clientHeight);
```
需要注意的是，此时没有放任何元素
![6.png](https://i.loli.net/2018/08/03/5b6427b36da85.png)
<code>961</code> 是可视区域高度， <code>1056</code>是浏览器窗口高度。 <br>我们再看种情况，此时&lt;body&gt;中 放一个 <code>div</code>,设置以下样式
```sh
.scrolldiv{
            width: 500px;
            height: 400px;
            border: 0;
            margin: 1000px auto 100px auto;
            background-color: #FF0000;
        }
        ```
        我们再看一下各种高度
        
![7.png](https://i.loli.net/2018/08/03/5b6428b0e5e49.png)
诶，不一样了，可以得出以下结论<br>
<p style="color:red;font-size:16px"><code>window.innerHeight</code>与  <code>window.outerHeight</code> 分别表示 <b>可视区域和浏览器窗口高度</b>，<code>document.documentElement.clientHeight</code>表示<b>整个网页的高度(可视区域加上可视区域中目前看不到的高度)</b>，<code>document.body.clientHeight</code>表示<b>可视区域高度</b></p>。


<hr>
JS新手，有错误请指出。