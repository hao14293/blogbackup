---
title: Ubuntu 16.04 程序快捷方式创建方法
categories: Linux
abbrlink: 43744
date: 2018-03-09 11:15:03
tags:
---
方法很简单，下面以创建Eclipse快捷方式为例。其他程序只需把名字改为对应名字就行。
<br><br>
<pre><code>sudo gedit /usr/share/applications/Eclipse.desktop</code></pre>
<br><br>
复制以下代码：<br>
<pre><code>
[Desktop Entry]

Encoding=UTF-8

Name=Eclipse

Comment=Eclipse

Exec=/home/ubuntu/eclipse/jee-oxygen/eclipse/eclipse

Icon=/home/ubuntu/eclipse/jee-oxygen/eclipse/icon.xpm

Terminal=false

StartupNotify=true

Type=Application

Categories=Application;Development;
</code></pre>
把<code>Exec</code>和<code>Icon</code>改为你的地址。<br><br>
    最后
<pre>sudo chmod u+x Pycharm.desktop</pre>
