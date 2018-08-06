---
title: setTimeout和setInterval
categories: web
abbrlink: 59283
date: 2018-08-04 00:54:10
tags:
---
> 简单来说，一个是过一段时间执行某个任务，然后就没然后了;一个是每隔一段时间执行一下某个任务。


<!--more-->

看似很简单的两个概念，其实用途很广。下面就举两个刚学的例子。
##### 延迟切换菜单栏 (setTimeout)
```sh
<script>
function $(id) {
        return typeof id==='string'?document.getElementById(id):id;
    }
window.onload=function () {
       // 标签的索引
       var index=0;
       var timer=null; //定时器

       var lis=$('notice-tit').getElementsByTagName('li'),
           divs=$('notice-con').getElementsByTagName('div');

       if(lis.length!=divs.length) return;

       for(var i=0;i<lis.length;i++){
           lis[i].id=i;
           lis[i].onmouseover=function () {
               var that=this;
               //如果存在尊卑执行的定时器，立刻清除，只有当前时间大于500ms才开始执行
               if(timer){
                   clearTimeout(timer);
                   timer=null;
               }
               //延迟500ms
               timer=setTimeout(function () {
                   for(var j=0;j<lis.length;j++){
                       lis[j].className='';
                       divs[j].style.display='none';
                   }
                   lis[that.id].className='select';
                   divs[that.id].style.display='block';
               },500);
           }
       }
   }
  </script> 
   ```
   
##### 图片自动循环切换 (setInterval)

```sh
<script type="text/javascript">
        window.onload=function(){
            var wrap=document.getElementById('wrap'),
                pic=document.getElementById('pic'),
                list=document.getElementById('list').getElementsByTagName('li'),
                index=0,
                timer=null;
            var imgs=pic.getElementsByTagName("li");
            // 定义并调用自动播放函数
            // 定义图片切换函数
            // 鼠标划过整个容器时停止自动播放
            // 鼠标离开整个容器时继续播放至下一张
            // 遍历所有数字导航实现划过切换至对应的图片
            if(imgs.length=list.length){
                for(var i=0;i<list.length;i++){
                    list[i].id=i;
                    list[i].onmouseover=function(){
                        qiehuan(this.id);
                    }
                }
            }
            wrap.onmouseover=function(){
                clearInterval(timer);
            }
            wrap.onmouseout=function(){
                timer=setInterval(play,2000);
            }
            
            if(timer){
                clearInterval(timer);
                timer=null;
            }

            timer=setInterval(play,2000);
            //自动切换函数
            function play(){
                index++;
                if(index>=list.length){
                    index=0;
                }
                qiehuan(index);
            }

     
            function qiehuan(curindex){
                for(var j=0;j<imgs.length;j++){
                    list[j].className="";
                    imgs[j].style.display="none";
                }
                list[curindex].className="on";
                imgs[curindex].style.display="block";
                index=curindex;
            }
        }

    </script>
```