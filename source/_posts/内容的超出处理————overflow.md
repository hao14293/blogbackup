---
title: 内容的超出处理————overflow
abbrlink: 20894
date: 2018-06-04 23:37:46
tags:
categories: web
---
<hr>
<!--more-->
### 1.基本语法
```sh
overflow: hidden;
```
overflow 的属性值

| 值 | 描述 |
|:---|:---|
| visible | 默认值。内容不会被修剪，会呈现在元素框之外。 |
| hidden | 内容会被修剪，并且其余内容是不可见的。 |
| scroll | 内容会被修剪，但是浏览器会显示滚动条以便查看其余内容。 |
| auto | 如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容。 |
| inherit |  从父元素继承overflow 属性 |

> x 和 y 两个方向的控制:<br>overflow 属性的设置是针对水平、垂直两个方向的，如果只想针对其中一个方向设置，可以使用 overflow-x 或 overflow-y ,取值与overflow 一样。

### 2. 实现文本超出隐藏
使用CSS实现元素的文本超出隐藏，通常有两种方式，一种是直接隐藏内容，另一种是超出显示为省略号。<br>
超出隐藏比较简单，只需要为一个有固定宽高的元素设置为 <code>overflow:hidden;</code>。 <br>
下面演示超出文本显示为省略号
```sh
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .text-overflow{
            width: 400px;
            height: 40px;
            border: 2px solid black;
            line-height: 40px;
            overflow: hidden;
            text-overflow: ellipsis;
            word-break: keep-all;
            white-space: nowrap;
        }
    </style>
</head>
<body>
<div class="text-overflow">
    这个电影让我想起了一年前我和家人闹矛盾，被断生活费的那段日子了。
    被房东赶出来，落难了身边大部分朋友都变路人，寄宿在一个很肮脏的屋子里过了4个多月。
    我还清楚地记得去年的圣诞节那晚我在打工的地方工伤切伤了左手拇指，一个人捂着满是血的
    左手在车站侯座上坐了很久很久……明天一定会变好的！
</div>
</body>
</html>
```
> 样式的最后4行代码是显示省略号的“核心代码”。<br>text-overflow:ellipsis; 表示当对象内文本溢出时显示省略号；它必须与hidden:overflow; 一起使用；<br> word-break:keep-all; 和 white-space: nowarp; 的作用一致，都是让对象内的文本不换行。

### 3. 多行文本超出显示为省略号
多行文本超出显示为省略号的需求，可以由后台直接截取相应数据量的文字，后面加上“...”，再作为实际数据呈现在前端。<br>
如果单纯由前端来实现，就有点复杂，仅使用 HTML 和 CSS很难实现，通常借助Javascript  来辅助实现。<br>
```sh
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .text-overflow{
            width: 400px;

            border: 2px solid black;
            line-height: 20px;

        }
    </style>
</head>
<body>
<div class="text-overflow" id="con">
    这个电影让我想起了一年前我和家人闹矛盾，被断生活费的那段日子了。 被房东赶出来，落难了身边大部分朋友都变路人，寄宿在一个很肮脏的屋子里过了4个多月。
    我还清楚地记得去年的圣诞节那晚我在打工的地方工伤切伤了左手拇指，一个人捂着满是血的 左手在车站侯座上坐了很久很久……明天一定会变好的！
</div>
<script>
    var con = document.getElementById('con');
    var textcon = con.innerHTML;
    con.innerHTML = textcon.substring(0,49) + '...';
</script>
</body>
</html>
```

### 4. 关于 overflow 问题区
>  overflow:scroll; 与 overflow:auto; 的区别 ？<br>
当文本超出时，overflow: scroll 与 overflow: auto 都会出现滚动条。<br>当文本没有超出元素区域时，overflow:auto 并不会出现滚动条，而 overflow: scroll 依旧会显示滚动条。 <br>
