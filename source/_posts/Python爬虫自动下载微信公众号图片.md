---
title: Python爬虫自动下载微信公众号图片
tags: 爬虫
categories: Python
abbrlink: 14174
date: 2018-05-15 21:53:29
---
![不爱我就拉倒](https://i.loli.net/2018/05/15/5afaea0fcf013.jpeg)
<!--more-->
>  爬虫真的是特别好玩的东西，Python是写爬虫最方便的语言。


不知大家有没有这种经历，躺床上刷微信公众号发现一篇特别好的文章，很打动人心，文章里面的配图也特别精美就想下载下来，当然手机可以一张一张保存，但图片比较多的时候就很不方便了，而且写博客、公众号的时候大部分是在电脑上。但是用电脑右击保存图片却是.webp格式，而这种格式不能直接插入文章中，虽然可以把.webp保存为其它格式，但这样效率太低，尤其是你想大量获取图片时。<br>
所以，我就想写一个爬虫来自动下载图片，挑选出精致中意的图片以备以后使用。<br><br>
今晚简单写了一个初级爬虫，虽然已经可以满足我的需求，但不够精致，哈哈哈，也就是只能让那个我一个人在我电脑上用。所以，后面两天再修改一下，让每个人都可以简单使用。<br><br>
下面放代码：
```sh
#--coding:utf-8  --

import os
import urllib2
import re
import requests
from datetime import datetime


# 保存路径
def save_img():
    url = str(raw_input("please input url:"))
    #try:
    if(os.name == 'Windows'):
        pass
    elif os.name == 'posix':
        if(os.path.exists(r"/home/ubuntu/img")):
            path = '/home/ubuntu/img/'
        else:
            os.makedirs(r"/home/ubuntu/img")
            path = '/home/ubuntu/img/'
        
        
                
    html = requests.get(url).text
    picurl = re.findall('(?<=data-src=").+?(?=")', html, re.S)
        
    i = 0
    for each in picurl:
        print each
            
        if 'jpeg' in each:
            piclast = '.jpeg'
        elif 'jpg' in each:
            piclast = '.jpg'
        elif 'gif' in each:
            piclast = '.gif'
        else:
            piclast = '.jpeg'
               
        pic = requests.get(each,timeout=10)
        string = path + datetime.now().strftime('%Y-%M-%d %H:%M:%S') +str(i)+ piclast
            
        fp = open(string,'wb')
        fp.write(pic.content)
        fp.close()
        i += 1;  
            
if __name__ == '__main__':
    while(True):
        save_img()
    ```
    
代码还是有点小问题的，当然这只是初级版本，这两天会修改完善的。

<h4>第一次修改后代码:</h4>
```sh
#--coding:utf-8  --

# python2.7
import os
import urllib2
import re
import requests
from datetime import datetime


# 保存路径
def save_img():
    url = str(raw_input("please input url:"))
    #try:
    
    # 将图片保存在当前文件夹的img文件夹中
    
    filepath = os.getcwd()
    #parent_path = os.path.dirname(filepath)
    if(os.name == 'Windows'):
        path = filepath + '\\img\\'
    elif os.name == 'posix':
        path = filepath + '/img/'
    
    print "图片保存在" + path + "中"
    filepath = path
    if(os.path.isdir(filepath)):
        pass
    else:
        os.makedirs(filepath)
    path = filepath
    
    # 解析网页获取图片url   
        
    html = requests.get(url).text
    picurl = re.findall('(?<=data-src=").+?(?=")', html, re.S)
      
    # 保存图片  
    i = 1
    print "当前网页一共有 " + str(len(picurl)) + " 张图片"
    for each in picurl:
        print "正在下载第 " + str(i) + " 张图片" 
        # 判断图片格式    
        if 'jpeg' in each:
            piclast = '.jpeg'
        elif 'jpg' in each:
            piclast = '.jpg'
        elif 'gif' in each:
            piclast = '.gif'
        else:
            piclast = '.jpeg'
        # 保存图片        
        pic = requests.get(each,timeout=10)
        string = path + datetime.now().strftime('%Y-%M-%d %H:%M:%S') +str(i)+ piclast # 图片名为当前时间
            
        with open(string,'wb') as fp:
            fp.write(pic.content)
            fp.close()
        i += 1;  
            
if __name__ == '__main__':
    #while(True):
        save_img()
        print "下载完毕"
        ```