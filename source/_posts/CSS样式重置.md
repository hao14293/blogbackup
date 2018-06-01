---
title: CSS样式重置
categories: web
abbrlink: 58849
date: 2018-05-29 22:48:36
tags:
---
下列是日常开发中常用的样式重置表
<!--more-->
```sh
@charset"UTF-8";

html{
    color: #000;
    background: #FFF;
    font-family: 'Microsoft YaHei',sans-serif,"Arial Narrow";
}

body,div,dl,dt,dd,ul,ol,li,h1,h2,h3,h4,h5,h6,pre,code,form,fieldset,legend,input,button,textarea,p,blockquote,th,td,strong{
    padding: 0;
    margin: 0;
    font-family: 'Microsoft YaHei',sans-serif,"Arial Narrow";
}

table{
    border-collapse: collapse;
    border-spacing: 0;
}

img{border: 0;}
a{
    text-decoration: none;
    color: #333;
    outline: none;
}
a:hover{
    text-decoration: underline;
}

var,em,strong{font-style: normal;}

em,strong,th,var{font-style: inherit;font-weight: inherit;}

li{list-style: none;}

caption,th{text-align: left;}

h1,h2,h3,h4,h5,h6{font-size: 100%;font-weight: normal;}

input,button,textarea,select,optgroup,option{
    font-family: inherit;
    font-size: inherit;
    font-style: inherit;
    font-weight: inherit;
}

input,button,textarea,select{*font-size:100%;}
```
具体使用看网站需求，这些一般需求可以概括到。