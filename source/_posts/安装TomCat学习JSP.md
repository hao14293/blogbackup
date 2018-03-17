---
title: 安装TomCat学习JSP
tags: TomCat
categories: Geeker
abbrlink: 33888
date: 2018-03-17 11:35:30
---
&nbsp;&nbsp;&nbsp;&nbsp;这学期开了JSP应用教程，昨天上课时需要安装TomCat，但大家安装的都不是很顺利，安装后也会有各种问题，我就帮大家解决了各种问题，在这里记录一下，以后有同学需要的时候可以看一下。<br>
### 安装JDK，配置java环境
JSP，全称Java Server Page ，所以肯定需要Java环境。我安装的是[1.8.0_161](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)，不推荐安装最新版的9.0。<br>
Linux系统安装JDK可以参考我之前这篇文章，[点这里](http://shiyi.today/year/03/10/47249/)。<br>
Windows系统，我习惯安装在C:\Java,默认安装在C:\Program Files\Java,安装在哪里都行，然后一路默认下去，等待安装结束。<br>
然后就是配置Java环境。<br><!--more-->
 右击我的电脑属性，高级系统设置，环境变量，点系统变量的新建，
 ![1.png](https://i.loli.net/2018/03/17/5aac8e3b00f8a.png)
 变量名：<code>JAVA_HOME</code>
 变量值：是你安装的路径，我的是C:\Java\jdk1.8.0_161,默认的话是 C:\ProgramFiles\Java\jdk1.8.0_161,你可以点击 浏览目录选择。<br>
 再修改path 变量，win7可以直接在最前面加上， <code>%JAVA_HOME%\bin;</code>注意在英文输入法下输<code>;</code>,win10需要点新建 ，值也是<code>%JAVA_HOME%\bin;</code>，然后点击上移到第一个，确定。<br>
 再新建 classpath ,变量值为<code>.\;%JAVA_HOME%\lib\tiils.jar</code><br>
 然后就确定、确定、确定。<br>
 打开cmd,分别输入java、javac 看是不是分别输出一大长串东西，是的话就是安装好了，如果提示不是内部命令，那就检查一下是不是分号在中文输入法下输入的，win10的path可能会默认加上<code>" ;"</code>,有的话就删掉。<br>
 这样的话就能配置好Java环境了。<br>
 ### 安装TomCat
 [下载TomCat](https://tomcat.apache.org/download-80.cgi)
 下载好后一路点下去就能安装好了。<br>
 如果你下载的有两个或以上Java版本，在选择Java地址是一定选择你现在配置的那个版本地址，不然会出错。<br>
 打开TomCat，在浏览器输入 <code>127.0.0.1:8080</code> 出现TomCat的相关页面就安装好了。<br>
 如果操作没有问题，Java路径也没问题，那就卸载后重新装一下。<br>
 Linux环境下，创建TomCat文件夹，解压，命令行
 ```sh
 $ cd TomCat/apache-tomcat-8.5.29/bin/
 $ ./startup.sh 
```
就可以运行<br>
 
### 使用时的问题
TomCat默认安装在 <code>C:\Program Files\Apache Software Foundation</code>下,找到<code>webapps\Root</code>,这是TomCat的Web服务器的根目录，在里面新建JSP文件，就可以在浏览器中运行了。<br>
但win10下你肯定会发现无法将JSP文件保存在这个目录下，就算保存下来也是TXT格式，不是JSP格式，这是因为win10为了安装问题，默认不能通过修改文件后缀名来修改文件格式，解决这个问题可以将下图中的文件扩展名打上勾。<br>
![2.png](https://i.loli.net/2018/03/17/5aac8e3b1d456.png)
然后就解决了。<br>
如果运行时再提示有问题，那就是你输入格式的错误了，看是不是空格多打或少打了，或者大小写弄错了，自己琢磨一下就能解决了。<br>


有问题的直接评论问我，或者给我发邮件。<br>
大早上起床写教程，好累啊。


