---
title: 深入理解不起眼的 &lt;head&gt; 标签
categories: web
abbrlink: 54492
date: 2018-07-24 17:03:04
tags:
---
头标签 <code>&lt;head&gt;</code> ,这个不起眼不被重视的标签，其实很重要，值得深入理解一下。<br>
<!--more-->
<code>&lt;head&gt;</code> 部分可以放 <code>&lt;base&gt;</code><code>&lt;link&gt;</code><code>&lt;meta&gt;</code><code>&lt;script&gt;</code><code>&lt;style&gt;</code>以及<code>&lt;title&gt;</code>。 下面分别说一下这些标签作用:
* <code>&lt;title&gt;</code>: 网页的标题，显示在浏览器选项卡上。
* <code>&lt;meta&gt;</code>: 元信息，所谓元信息，指的是对信息进行描述的信息。比如，网页有什么属性，用什么语言，作者是谁，主要内容是什么，这些都是元信息。<br> meta是用来在HTML文档中模拟HTTP的响应头报文。 有下面几条属性:

| 属性 | 值 | 描述 |
|:----:|:---:|:---:|
| content | some_text | 定义与 http-equiv 或 name 属性相关的元信息 |
| http-equiv | content-type/expires/refresh/set-cookie | 把 content 属性关联到 HTTP 头部。|
| name | author/description/keywords/generator/revised/others | 把 content 属性关联到一个名称。|
| scheme | some_text | 定义用于翻译 content 属性值的格式。|

<pre>
看几个例子:
&lt;meta name="keywords" content="HTML,ASP,SQL" &gt;
&lt;meta name="author" content="lili" &gt;
</pre>

一般大家对 meta 的了解就这么多，其实meta 还有很多属性也很有用，下面详细说一下:
<pre>
基本语法：
&lt; meta 属性="该属性下的子属性" content="具体子属性对应的属性值" &gt;

name属性:
 generator: 代码的生成工具
 keywords: 关键词，不同词语用英文逗号分隔开
 description: 描述信息
 author: 作者
 copyright: 版权
 renderer: 渲染内核
 robots: 机器人，主要包括 all,none,index,noindex,follow,nofollow几种属性值。
 * all: 文件将被检索，且页面上的链接可以被查询
 * none : 文件将不被检索，且页面上的链接不可以被查询
 * index: 文件将被检索
 * noindex: 文件将不被检索，但页面上的链接可以被查询(不让robot/spider登录)
 * follow: 页面上的链接可以被查询
 * nofollow: 文件将不被检索，页面上的链接可以被查询(不让robot/spider 顺着此页链接向下深入地探查)

http-equiv属性:
 content-type: 编码类型
 content-language: 语言
 refresh: 用于定时让网页刷新或跳转到指定页面
 expires: 设定网页的到期时间，一旦过期则必须到服务器上重新调用。必须使用GMT时间格式
 pragma: 用于设定禁止浏览器从本地的缓存中调阅页面内容，设定后一旦离开网页就无法从Cache中再调出
 set-cookie: 设置cookie过期时间
 pics-label: 用于进行网页等级评定
 windows-target: 告知网页在当前窗口中以独立页面显示，可以防止网页被别人当做一个frame调用，即防止被钓鱼
 page-enter:  用于设定进入网页时的特殊效果
 page-exit: 离开网页时的特殊效果
 
当前使用频繁的主要有 字符编码、关键字、描述信息、自动刷新、独立页面显示

 设置字符编码:
  &lt;meta charset="UTF-8" &gt;
  
 这些设置主要是告诉浏览器信息，也方便搜索引擎抓取自己的页面
 </pre>
 * link: link标签定义文档与外部资源的关系。 最常见的是链接样式表。
<pre>
link 中 最常用的属性是 rel
&lt;link rel="value" &gt;
主要是链接外部样式表
&lt;link rel="stylesheet" href="./css/index.css" &gt;
link另外一个主要用处是 定义 icon 图标
&lt;link rel="icon" href="images/favicon.png" sizes="16*16" type="image/png" &gt;
</pre>

<code>&lt;head&gt;</code> 中的重点大概就是这些，一些小知识点还是挺重要的。

 
  
