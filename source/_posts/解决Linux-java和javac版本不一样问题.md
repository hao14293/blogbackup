---
title: 解决Linux java和javac版本不一样问题
categories: Linux
abbrlink: 27171
date: 2018-05-11 10:33:25
tags:
---
之前写过 [Linux安装java](http://shiyi.today/year/03/10/47249/) 教程，可是今天发现<code>java -version</code> 和 <code>javac -version</code> 版本号不一样。虽然在安装<code>java</code>时设置了系统变量，可系统依旧默认使用自带的 <code>OpenJDK</code>，下面给出解决方法。<br>
>>> <b>注：</b>使用<code>which java </code>和<code>which javac </code>可以查看各自路径。

简单来说，就是把这2个文件<code>ln -s</code> 到我们新的<code>jdk</code>下的<code>java</code>和<code>javac</code>上，命令如下:

    rm -rf /usr/bin/java
    rm -rf /usr/bin/javac
    ln -s $JAVA_HOME/bin/java /usr/bin/java
    ln -s $JAVA_HOME/bin/javac /usr/bin/javac
    
然后再查看<code>java -version</code> <code>javac -version</code>，问题解决了。