---
title: CSS选择器
categories: web
abbrlink: 37026
date: 2018-06-09 10:04:11
tags:
---
CSS2.0 版本中，共有10种选择器，ID选择器、类名选择器、标签名选择器、群组选择器、后代选择器、子代选择器、通配符选择器、毗邻选择器、伪类选择器、属性选择器。
<!--more-->
### 1. CSS2 选择器

| 选择器语法 | 选择器含义 |
|:-------|:---------:|
| #wrap | ID选择器 |
| .wrap |  类名选择器 |
| div | 标签名选择器 |
| #wrap p | 后代选择器 |
| #wrap>p | 子代选择器 |
| #wrap, .con p | 群组选择器  |
| * | 通配符选择器 |
| .con+p | 毗邻选择器 |
| :hover,:active | 伪类选择器 |
|  input[type='text'] | 属性选择器 |

### 2. CSS3 选择器————通用兄弟选择器
```sh
E~F{/*样式代码*/}
p~ul{}
```
匹配任何在E元素之后的同级F元素。<br>
通用兄弟选择器将选择某元素后面的所有兄弟元素，与毗邻选择器类似，需要在同一父元素中。
### 3. CSS3 选择器————伪类选择器
##### 3.1 结构性伪类

| 选择器语法 | 选择器含义 |
|:-------|:---------:|
| E:root | 匹配文档的根元素，对于HTML文档，就是HTML元素 |
| E:nth-child(n) | 匹配其父元素的第n个子元素，第一个编号为1 |
| E:nth-last-child(n) | 匹配其父元素的倒数第n个子元素，第一个编号为1 |
| E:last-child | 匹配父元素的最后一个子元素，等同于:nth-last-child(1) |
| E:nth-of-type(n) | 与:nth-child()作用类似，但是仅匹配使用同种标签的元素 |
| E:nth-last-of-type(n) | 与:nth-last-child()作用类似，但是仅匹配使用同种标签的元素 |
| E:first-of-type | 匹配父元素下使用同种标签的第一个子元素，等同于:nth-of-type(1) |
| E:last-of-type | 匹配父元素下使用同种标签的最后一个子元素，等同于:nth-last-of-type(1) |
| E:only-child | 匹配父元素下仅有的一个子元素，等同于:first-child:last-child或:nth-child(1):nth-last-child(1) |
| E:only-of-type | 匹配父元素下使用同种标签的唯一一个子元素，等同于:first-of-type:last-of-type或:nth-of-type(1):nth-last-type(1) |
| E:empty | 匹配一个不包含任何子元素的元素，注意，文本节点也被看作子元素 |
| E:not(s) | 匹配不符合当前选择器的任何元素 |

##### 3.2与用户界面相关的伪类

| 选择器语法 | 选择器含义 |
|:-------:|:---------:|
| E:enabled | 匹配表单中激活的元素 |
| E:disabled | 匹配表单中禁用的元素 |
| E:checked | 匹配表单中被选中的rasio或checkbox 元素 |
| E:selection | 匹配用户当前选中的元素 |
