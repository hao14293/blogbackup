---
title: 关于CSS里的line-height详解
tags: css
categories: web
abbrlink: 31791
date: 2018-04-15 22:44:47
---
### 一、前言
最近写CSS一直会用到line-height属性，但对其一直是一知半解。在看完《CSS权威指南》后，这周末就写写line-height的相关内容。

### 二、 基线、行间距和行高
先说清楚一些名词概念，这是理解下文的基础,首先看一张图:
![18.png](https://i.loli.net/2018/04/15/5ad3658e77758.png)

1. 基线: 一个文本行分成4格线，分别是顶线、中线、基线、底线，
```sh
 基线的位置与 字体font-family 有关，不同的字体基线的位置有偏差```
 <!--more-->
2. 内容区（content area）: 顶线与底线包围的区域,其高度与字体和字号相关,
```sh
                  粗略等于 font-size值;```
3. 行间距: 文本行基线之间的距离,由 <code style="color:red;">line-height-font-size</code>决定;
还是看上面那张图，就有比较直观的感受了。
下面是重点！
注意看， <code style="color:red;">line-height= (基线-底线的距离)+行距+(顶线-基线的距离)</code>，
等价于  <code style="color:red;">顶线-基线+基线-底线 + 行距 = 顶线-底线 + 行距</code>

而根据上文的定义， <code style="color:red;">顶线-底线</code>就是内容区的大小，它由 <code style="color:red;">font-size属性</code>决定;
所以可以得出:
 <code style="color:red;">line-height= font-size + 行距</code>

换句话说， <code style="color:red;">行间距=line-height - font-size</code>

行内框/行高: 它就是单个内联元素的高度！它是由 <code style="color:red;"> line-height属性</code>决定的;
```sh
   我们在内容区的上下两部分，各自加上半行间距(行间距的一半)，就是行内框的高度；
   所以，行内框的大小就是:  `（line-height - font-size)/2+font-size + （line-height - font-size)/2`
   即`（line-height - font-size) + font-size= line-height````
一定要注意:
```sh
仅考虑文本的情况，行内（文本）元素的高度是由line-height决定的，并不是由元素中的文本撑开的```
1. 行框: 它就是一行中，有多个内联元素时的 总体高度
```sh
  由该行中行内框组成, 行框高度要包含最高行内框的顶端和最低行内框的底端```
下面这张图，很直观的反应了行内框和行框的概念(但他内容区的):

![19.png](https://i.loli.net/2018/04/15/5ad3658e56a06.png)
所以总结一下就是，我们可以简单理解为，
  <code style="color:red;">font-size</code> :可以控制 内联元素里 文本大小(当然也和font-fmaily有关);
  <code style="color:red;">line-height</code>:可以控制 内联元素的 文本高度;

### 三、 line-height属性的值
* em/ex和百分数值: 都相对于<code style="color:red;"> (父)元素的font-size值 </code>计算；
* number: 继承的是一个缩放因子，子元素能够根据自己的字体大小计算行高;

### 四、图像的行内框
```sh
图像元素的行内框高度是由它自己的盒子（height+padding+border+margin）决定的```
### 五、参考文档
  1.[ css中的line-height;](https://segmentfault.com/a/1190000003038583)<br>
  2.[ CSS line-height与行内框;](http://www.zyy1217.com/2016/11/30/CSS%20line-height%E4%B8%8E%E8%A1%8C%E5%86%85%E6%A1%86/)

