---
title: Ubuntu 16.04 安装Eclipse
categories: Linux
abbrlink: 47249
date: 2018-03-10 01:13:10
tags:
---
### 配置 java 环境
* [官方下载jdk](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)(我安装的是jdk1.8.0_161)
* 创建安装文件夹（我的是 /opt/java)
<pre><code>sudo mkdir /opt/java</code></pre>
* 把下载下来的jdk-8u161-linux-x64.tar.gz 移到 /opt/java 并解压
<pre><code>sudo mv /下载/jdk-8u161-linux-x64.tar.gz /opt/java
sudo tar -zxvf jdk-8u161-linux-x64.tar.gz</code></pre>
* 配置环境变量
<pre>sudo gedit /etc/environment</pre><!--more-->
末尾加入以下配置（JAVA_HOME后的路径就是你jdk的文件位置）
<pre>PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:$JAVA_HOME/bin"
export CLASSPATH=.:$JAVA_HOME/lib:$JAVA_HOME/jre/lib
export JAVA_HOME=/opt/java/jdk1.8.0_161</pre>
修改完后保存关闭，输入下面命令使环境变量生效
<pre>source /etc/environment</pre>
* 但这个时候输入<code>java -version</code>发现是下面这样
<pre>$ java -version
 程序 'java' 已包含在下列软件包中：
   default-jre
   gcj-5-jre-headless
   openjdk-8-jre-headless
   gcj-4.8-jre-headless
   gcj-4.9-jre-headless
   openjdk-9-jre-headless
请尝试：sudo apt install <选定的软件包>
</pre>
这是因为Ubuntu默认安装的有java环境，需要我们再配置以下
<pre>  sudo update-alternatives --install /usr/bin/java    java /opt/java/jdk1.8.0_161/bin/java 300
    sudo update-alternatives --install /usr/bin/javac javac /opt/java/jdk1.8.0_161/bin/javac 300
    sudo update-alternatives --install /usr/bin/jar jar /opt/java/jdk1.8.0_161/bin/jar 300
    sudo update-alternatives --install /usr/bin/javah javah /opt/java/jdk1.8.0_161/bin/javah 300
    sudo update-alternatives --install /usr/bin/javap javap /opt/java/jdk1.8.0_161/bin/javap 300</pre>
    上面命令把每条后半部分地址改为你的jdk地址，/opt/java是我的地址。<br><br>
最后执行这条命令使之生效
<pre>sudo update-alternatives --config java</pre>
这个时候再执行<code>java -version</code> <code>javac -version</code>
<pre>$ java -version
java version "1.8.0_161"
Java(TM) SE Runtime Environment (build 1.8.0_161-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.161-b12, mixed mode)
$ javac -version
javac 1.8.0_161
</pre>
这样就大功告成。

### 安装 Eclipse
* [官方下载](http://www.eclipse.org/downloads/eclipse-packages/)
* 解压（我是单独创建了个eclipse文件夹）
<pre>cd 下载/
sudo mv eclipse-inst-linux64.tar.gz /eclipse
tar -zxvf eclipse-inst-linux64.tar.gz</pre>
* 进入文件夹双击<code>eclipse-inst</code>,然后就安装吧。（耐心）
* 创建快捷方式
<pre>[Desktop Entry]
  Encoding=UTF-8
  Name=Eclipse
  Comment=Eclipse
  Exec=/home/ubuntu/eclipse/jee-oxygen/eclipse/eclipse
  Icon=/home/ubuntu/eclipse/jee-oxygen/eclipse/icon.xpm
  Terminal=false
  StartupNotify=true
  Type=Application
  Categories=Application;Development;</pre>
 <code>Exec</code><code>Icon</code>改成你的地址。<br><br>
 对该文件进行赋权
 <pre>chmod u+x /usr/share/applications/Eclipse.desktop</pre>
 <br>
 <b>大功告成。</b>
 


    