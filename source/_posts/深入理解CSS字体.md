---
title: 深入理解CSS字体
categories: web
abbrlink: 37219
date: 2018-08-01 16:37:24
tags:
---
> 主要讲引用网络字体

<!--more-->
#### 常规字体

在css中字体通过 font 属性设置,如
```sh
.font{
	font-family: 'Microsoft TaHei';
    font-size: 24px;
    font-weight: 400;
    font-style: italic;
    }
    ```
 字体通常用英文名。下面是常见的字体中文名与其英文名<br>
 <b>windows常见内置中文字体</b>
 
 | 字体中文名 | 字体英文名 |
 |:-----:|:----------:|
 | 宋体 | SimSun |
 | 黑体 | SimHei |
 | 微软雅黑 | Microsoft Yahei |
 | 微软正黑体 | Microsoft JhengHei |
 | 楷体 | KaiTi |
 | 新宋体 | NSimSun |
 | 仿宋 | FangSong |
 
 #### 网络字体
 网络字体通过 <code>@font-face</code> 引用。
 IE9+以及各个主流浏览器均支持。
 
 <br>
 <b>语法</b>
 ```sh
 @font-face {
    font-family: <字体名>;
    src: <字体路径> [<格式>][,<字体路径> [<格式>]]*;
    [font-weight: <粗细>];
    [font-style: <样式>];
}
```

```sh
<style type="text/css">
@font-face {
  font-family: 'Bungee Inline';
  font-style: normal;
  font-weight: 400;
  src: local('Bungee Inline'), local('BungeeInline-Regular'), url(https://fonts.gstatic.com/s/bungeeinline/v2/Tb-1914q4rFpjT-F66PLCfn8qdNnd5eCmWXua5W-n7c.woff) format('woff');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02C6, U+02DA, U+02DC, U+2000-206F, U+2074, U+20AC, U+2212, U+2215, U+E0FF, U+EFFD, U+F000;
}

h2{
    font-family: 'Bungee Inline';
}
</style>
```
代码块中，<code>font-family</code>和<code>src</code>是必需的。<code>src</code>中的<code>local()</code>表是从本地系统查找字体，如果找不到，再从<code>url()</code>指定的查找。<br>
<code>format()</code>指的是字体的格式，常用字体格式如下：

| format格式 | Font格式 | 后缀名 |
| :-----:|:------:|:----:|
| truetype | TrueType | .tff |
| opentype | OpenType | .tff, .oft |
| truetype-aat | TrueType width Apple Advanced Typography extensions | .tff |
| embedded-opentype | Embedded Open Type | .eot |
| svg | SVG Font | .svg, .svgz |

<b>浏览器支持程度</b>

| 浏览器 | 支持类型 |
| :---:|:---:|
| IE6,7,8 | 仅支持 .eot |
| Firefox 3.5 | 支持 .ttf, .otf格式 |
| Firefox 3.6 | 支持 .ttf, .otf, 及woff格式 |
| Chrome,Safari, Opera | 支持 .ttf, .otf, .svg 格式 |


关于兼容各个浏览器的兼容写法，可以参考一下一个国外大神Paul Irish写的兼容代码：
```sh
@font-face {
    font-family: '字体名';
    src: url('字体名.eot'); /* IE9 兼容模式 */
    src: url('字体名.eot?#iefix') format('embedded-opentype'), /* IE6-IE8 */
             url('字体名.woff') format('woff'), /* 现代浏览器 */
             url('字体名.ttf')  format('truetype'), /* Safari, Android, iOS */
             url('字体名.svg#grablau') format('svg'); /* Legacy iOS */
   }
   ```
