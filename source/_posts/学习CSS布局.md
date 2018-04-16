---
title: 学习CSS布局
categories: web
abbrlink: 7925
date: 2018-04-12 16:05:18
tags: css
---
### 没有布局

如果你只想把所有内容都塞进一栏里，那么不用设置任何布局也是OK的。然而，如果用户把浏览器窗口调整的很大，这时阅读网页会非常难受：读完每一行之后，你的视觉焦点要从右到左移动一大段距离。试着调整下浏览器窗口大小你就明白我的意思了！

在解决这个问题之前，我们需要了解一个很重要的属性： <code>display</code>.
*** 
### display属性
<code>display</code> 是CSS中最重要的用于控制布局的属性。每个元素都有一个默认的 display 值，这与元素的类型有关。对于大多数元素它们的默认值通常是 <code>block</code> 或 <code>inline</code> 。一个 block 元素通常被叫做块级元素。一个 inline 元素通常被叫做行内元素。<br>
***
##### block
<div style="width:100%;padding:0 5px;border:2px green">
<p><code>div</code> 是一个标准的块级元素。一个块级元素会新开始一行并且尽可能撑满容器。其他常用的块级元素包括<code> p</code> 、<code> form</code> 和HTML5中的新元素： <code>header</code> 、<code> footer </code>、<code> section</code> 等等。</p></div>
***
##### inline
<code>span</code>是一个标准的行内元素。一个行内元素可以在段落中<code><span>像这样</span></code>包裹一些文字而不会打乱段落的布局。<code>a</code>元素是最常用的行内元素，它可以被用作链接。
***
##### none
另一个常用的display值是<code>none</code>。一些特殊元素的默认 <code>display</code> 值是它，例如 <code>script</code> 。<code> display:none </code>通常被 JavaScript 用来在不删除元素的情况下隐藏或显示元素。

它和<code> visibility</code> 属性不一样。把<code> display</code> 设置成<code> none </code>元素不会占据它本来应该显示的空间，但是设置成<code> visibility: hidden</code>; 还会占据空间。
***
<!--more-->
##### 其他display值
还有很多的更有意思的 display 值，例如<code> list-item</code> 和<code> table</code> 。[这里有一份详细的列表](https://developer.mozilla.org/en-US/docs/Web/CSS/display)。之后我们会讨论到 <code>inline-block</code> 和<code> flex</code> 。
##### 额外加分点
就像我之前讨论过的，每个元素都有一个默认的 display 类型。不过你可以随时随地的重写它！虽然“人为制造”一个行内元素可能看起来很难以理解，不过你可以把有特定语义的元素改成行内元素。常见的例子是：把 <code>li</code> 元素修改成 <code>inline</code>，制作成水平菜单。

### margin:auto;
```sh
#main {
  width: 600px;
  margin: 0 auto;
}
```
<div id="main" style="width:600px; margin: 0 auto;border: 1px solid red;">
设置块级元素的 width 可以防止它从左到右撑满整个容器。然后你就可以设置左右外边距为 auto 来使其水平居中。元素会占据你所指定的宽度，然后剩余的宽度会一分为二成为左右外边距。<br>
<br>
唯一的问题是，当浏览器窗口比元素的宽度还要窄时，浏览器会显示一个水平滚动条来容纳页面。让我们再来改进下这个方案...</div>
***
### max-width
```sh
#main {
  max-width: 600px;
  margin: 0 auto;
}
```
<div style="max-width: 600px;margin: 0 auto;">在这种情况下使用 max-width 替代 width 可以使浏览器更好地处理小窗口的情况。这点在移动设备上显得尤为重要，调整下浏览器窗口大小检查下吧！

顺便提下， 所有的主流浏览器包括IE7+在内都支持 max-width ，所以放心大胆的用吧。</div>
***
### 盒模型
在我们讨论宽度的时候，我们应该讲下与它相关的另外一个重点知识：<i>盒模型</i>。当你设置了元素的宽度，实际展现的元素却超出你的设置：这是因为元素的边框和内边距会撑开元素。看下面的例子，两个相同的元素显示的实际宽度却不一样。<br>
```sh
.simple{
  width: 500px;
  margin: 20px auto;
}
.fancy{
  width: 500px;
  margin: 20px auto;
  padding: 50px;
  border-width: 10px;
}
```

![1.png](https://i.loli.net/2018/04/15/5ad22a7322bce.png)
以前有一个代代相传的解决方案是通过数学计算。CSS开发者需要用比他们实际想要的宽度小一点的宽度，需要减去内边距和边框的宽度。值得庆幸地是你不需要再这么做了...
***
### box-sizing
人们慢慢的意识到传统盒子模型不直接，所以它们新增了一个叫做<code>box-sizing</code>的CSS属性。当你设置了一个元素为<code>box-sizing: border-box;</code>时，此元素的内边距和边框不再增加他的宽度。这里由一个与前一页相同的例子，惟一的区别是两个元素都设置了<code>box-sizing: border-box;</code>:
```sh
.simple {
  width: 500xp;
  margin: 20px auto;
  -webkit-box-sizing: border-box;
     -moz-box-sizing: border-box;
          box-sizing: border-box;
  }
  .fancy {
    width: 500px;
    margin: 20px auto;
    padding: 50px;
    border: solid blue 10px;
    -webkit-box-sizing: border-box;
       -moz-box-sizing: border-box;
            box-sizing: border-box;
  }
  ```
  ![2.png](https://i.loli.net/2018/04/15/5ad22c6df238d.png)
  既然没有比这更好的方法，一些CSS开发者想要页面上所有的元素都有如此表现。所以开发者们把以下CSS代码放在它们页面上：
  ```sh
  * {
    -webkit-box-sizing: border-box;
       -moz-box-sizing: border-box;
            box-sizing: border-box;
    }
 ```
    这样可以确保所有的元素都会用这种更直观的方式排版。<br>
    不过<code>box-sizing</code>是个很新的属性，目前你还应该向我上面例子中那样使用<code>-webkit</code>和<code>-moz-</code>前缀。这可以启动特定浏览器实验中的特性。同时记住它是支持IE8+的。
***
### position
为了制作更多复杂的布局，我们需要讨论下<code>position</code>属性。它有一大堆的值，名字还都特抽象，别提有多难记了。让我们先一个个的过一遍，不过你最好还是把这页放到书签里。
##### static
```sh
.static {
    position: static;
}
```
![3.png](https://i.loli.net/2018/04/15/5ad22fcb4f6ea.png)
##### relative
```sh
.relative1 {
  position: relative;
}
.relative2 {
  position: relative;
  top: -20px;
  left: 20px;
  background-color: white;
  width: 500px;
}
```
![4.png](https://i.loli.net/2018/04/15/5ad22fcb4afd7.png)
##### fixed
一个固定定位（position属性的值为fixed)元素会相对于视窗来定位，这意味着即便页面滚动，它还是会停留在相同的位置。和 relative 一样， top 、 right 、 bottom 和 left 属性都可用。

比如让一个div始终显示在页面右下角，CSS如下：
```sh
.fixed {
  position: fixed;
  bottom: 0;
  right: 0;
  width: 200px;
  background-color: white;
}
```
一个固定定位元素不会保留它原本在页面应有的空隙（脱离文档流）。<br>
令人惊讶地是移动浏览器对 fixed 的支持很差。[这里有相应的解决方案](http://bradfrost.com/blog/mobile/fixed-position/).
##### absolute
<code>absolute</code>是最棘手的position值。 <code>absolute </code>与<code> fixed </code>的表现类似，但是它不是相对于视窗而是相对于最近的“positioned”祖先元素。如果绝对定位（position属性的值为absolute）的元素没有“positioned”祖先元素，那么它是相对于文档的 body 元素，并且它会随着页面滚动而移动。记住一个“positioned”元素是指 position 值不是<code> static </code>的元素。
***

### position例子
通过具体的例子可以帮助我们更好地理解"position"。下面是一个真正的页面布局。
```sh
.container {
  position: relative;
}
nav {
  position: absolute;
  left: 0px;
  width: 200px;
}
section {
  /* position is static by default */
  amrgin-left: 200px;
}
footer {
  position: fixed;
  bottom: 0;
  left: 0;
  height: 70px;
  background-color: white;
  width: 100%;
}
body {
  margin-bottom: 120px;
}
```
![10.png](https://i.loli.net/2018/04/15/5ad2fac68be6a.png)
这个例子在容器比nav元素高的时候可以正常工作。如果容器比nav元素低，那么nav会溢出到容器的外面。之后我们会讨论下其他布局技术，它们都各有优劣。
***
### float
另一个布局中常用的CSS属性是<code>float</code>。float可用于实现文字环绕图片，如下
```sh
img {
  float: right;
  margin: 0 0 1em 1em;
}
```
![11.png](https://i.loli.net/2018/04/15/5ad35aa1d387b.png)
***
### clear 
<code>clear</code>属性被用于控制浮动。比较下面两个例子：
```sh
<div class="box">...</div>
<section>...</section>
```
```sh
.box {
  float: left;
  width: 200px;
  height: 100px;
  margin: 1em;
}
```
![11.png](https://i.loli.net/2018/04/15/5ad35be7e0c87.png)
```sh
.box {
  float: left;
  width: 200px;
  height: 100px;
  margin: 1em;
}
.after-box {
  clear: left;
}
```
![12.png](https://i.loli.net/2018/04/15/5ad35be7d3462.png)
***
### 清除浮动(clearfix hack)
在使用浮动的时候经常会遇到一个古怪的事情：
```sh
img {
  float: right;
}
```
![13.png](https://i.loli.net/2018/04/15/5ad35d2a1d38d.png)
见证奇迹的时刻到了！有一种比较丑陋的方法可以解决这个问题，它叫做清除浮动（clearfix hack）.

让我们加入一些新的CSS样式：
```sh
.clearfix {
  overflow: auto;
}
```
现在再看看发生了什么：
![14.png](https://i.loli.net/2018/04/15/5ad35d2a192b2.png)
这个可以在现代浏览器上工作。如果你想要支持IE6，你就需要再加入如下样式：
```sh
.clearfix {
  overflow: auto;
  zoom: 1;
}
```
有些独特的浏览器需要“额外的关照”。[清除浮动这潭 水很深很深](https://stackoverflow.com/questions/211383/what-methods-of-clearfix-can-i-use)，但是这个简单的解决方案已经可以在今天所有的主要浏览器上工作。
***
### 浮动布局例子
完全使用<code> float </code>来实现页面的布局是很常见的。这里有一个我之前用<code> position </code>实现的布局例子，这次我使用<code> float </code>实现了它。
```sh
nav {
  float: left;
  width: 200px;
}
section {
  margin-left: 200px;
}
```
![16.png](https://i.loli.net/2018/04/15/5ad35eeeab82d.png)
***
### 媒体查询
“响应式设计（Responsive Design” 是一种让网站针对不同的浏览器和设备“呈现”不同显示效果的策略，这样可以让网站在任何情况下显示的很棒！

媒体查询是做此事所需的最强大的工具。让我们使用百分比宽度来布局，然后在浏览器变窄到无法容纳侧边栏中的菜单时，把布局显示成一列：
```sh
@media screen and (min-width: 600px){
  nav {
    float: left;
    width: 25%;
  }
  section {
    margin-left: 25%;
  }
}
@media screen and (max-width: 599px){
  nav li{
    display: inline;
  }
}
```
使用 [meta viewport](https://dev.opera.com/articles/an-introduction-to-meta-viewport-and-viewport/) 之后可以让你的布局在移动浏览器上显示的更好。
***
### inline-block
你可以创建很多网格来铺满浏览器。在过去很长的一段时间内使用 float 是一种选择，但是使用 inline-block 会更简单。让我们看下使用这两种方法的例子：

困难的方式（使用浮动）
```sh
.box {
  float: left;
  width: 200px;
  height: 100px;
  margin: 1em;
}
.after-box {
  clear: left;
}
```
容易的方式（使用 inline-block）<br>
你可以用 display 属性的值 inline-block 来实现相同效果。
```sh
.box2 {
  display: inline-block;
  width: 200px;
  height: 100px;
  margin: 1em;
}
```
### inline-block布局
你可以使用 inline-block 来布局。有一些事情需要你牢记：

* vertical-align 属性会影响到 inline-block 元素，你可能会把它的值设置为 top 。
* 你需要设置每一列的宽度
* 如果HTML源代码中元素之间有空格，那么列与列之间会产生空隙

```sh
nav {
  display: inline-block;
  vertical-align: top;
  width: 25%;
}
.column {
  display: inline-block;
  vertical-align: top;
  width: 75%;
```
*** 







