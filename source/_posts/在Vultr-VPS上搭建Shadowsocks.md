---
title: 在Vultr VPS上搭建Shadowsocks
tags:
  - vpn
  - vps
  - ss
categories: Geeker
abbrlink: 57013
date: 2018-02-07 00:16:39
---
作为一个离开Google就没法生活的人，之前也尝试了很多方法。这次自己搭建vpn,也折腾了不少，值得好好记录一下。废话少说，直接进入正题。

## 1.VPS的选择
　　可选择的VPS有很多，国内有阿里云、腾讯云，不过随着国内管控越来越严，在国内厂商的VPS上搭建VPN不是一个好的选择(你懂得)。知乎上有个相关话题,需要的可以参考一下。我选择Vultr是因为它便宜，在这里说我也不怕嘲笑，我还是个穷学生，经济问题还是首先要考虑的。可选择的套餐很多，5 $/mon还可以接受。
![1.png](https://i.loli.net/2018/03/07/5a9f517f5e486.png)
<!--more-->

进入官网后注册登录后还要先充值10，当时我想先充0.01得了，结果人家不傻，最低要10$，也算了，为了我的Google,花点钱也可以学点东西。选择好位置、价格后，其他的默认就行了，然后就可以部署了。

<b>提示：系统最好选择centos 6，方便部署</b>

## 2.在VPS上部署Shadowsocks
首先先登录到VPS上，我系统是Ubuntu 16.04 LTS,直接
<code>ssh root@your vps ipaddress</code>

ipaddress可以在这里看
![2.jpg](https://i.loli.net/2018/03/07/5a9f517f28365.jpg)
密码是系统默认生成的，如果需要修改密码，可以参考这个官方教程。

### 2.1下面开始进入重点，部署Shadowsocks服务端.
这里使用 teddysun 的一键安装脚本。

依次输入下面三条命令

<pre><code>wget –no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh
chmod +x shadowsocks.sh
./shadowsocks.sh 2>&1 | tee shadowsocks.log</code></pre>

最后一步输完，你应该会看到下图中内容──是要你为 Shadowsocks 服务设置一个个人密码。

![3.jpg](https://i.loli.net/2018/03/07/5a9f517f2ecf9.jpg)

设置 ss 密码
输好回车后会让你选择一个端口，输入 1-65535 间的数字都行。

![4.jpg](https://i.loli.net/2018/03/07/5a9f517f49a08.jpg)

设置 ss 端口

遵照上图指示，按任意键开始部署 Shadowsocks。这时你什么都不用做，只需要静静地等它运行完就好。结束后就会看到你所部署的 Shadowsocks 的配置信息。

![5.jpg](https://i.loli.net/2018/03/07/5a9f517f4f519.jpg)

ss 配置信息
记住黄框中的内容，也就是服务器 IP、服务器端口、你设的密码和加密方式。

### 2.2 TCP Fast Open
实际上只要具备上述四个信息，你就可以在自己的任意设备上进行登录使用了。但是为了更好的连接速度，你还需要多做几步。

首先是打开 TCP Fast Open，输入以下命令，意为用 nano 这个编辑器打开一个文件。

<code>nano /etc/rc.local</code> 

你的「终端」会刷新一下，出现下图。

![6.jpg](https://i.loli.net/2018/03/07/5a9f517f50dae.jpg)

别慌张，它就是个文本编辑器。用方向键把光标移到最末端，粘贴下面这一行内容，然后按 Ctrl + X 退出。

<code>echo 3 > /proc/sys/net/ipv4/tcp_fastopen</code>

输入“Y”并回车确认退出。

![7.jpg](https://i.loli.net/2018/03/07/5a9f517f524d2.jpg)

然后依法炮制，输入：
<code>nano /etc/sysctl.conf</code>

在文末加上下面的内容，保存退出。

<code>net.ipv4.tcp_fastopen = 3</code>

打开一个 Shadowsocks 配置文件。

<code>nano /etc/shadowsocks.json</code>

把其中 “fast_open” 一项的 false 替换成 true。

<code>“fast_open”:true</code>

如果你希望添加多用户的话，可以将 “password” 字段如下图修改。其中，”22345”:”password1”意为该用户使用 22345 端口、以“password1”为密码连接登录 Shadowsocks。

![8.jpg](https://i.loli.net/2018/03/07/5a9f517f53bc0.jpg)

保存退出。最后，输入以下命令重启 Shadowsocks。

<code>/etc/init.d/shadowsocks restart</code>

### 2.3 安装 Shadowsocks 客户端
客户端安装就简单很多了，Android、Win、Mac这里就不再多述了。Linux客户端配置还是很麻烦的，改天再单独写个教程。

## 3.开启锐速
完成上述步骤后，使用过程中可能会发现连接速度有时不太稳定。这就是「锐速」发挥功能的时候了。

### 3.1 什么是锐速
锐速 ServerSpeeder 是一个 TCP 加速软件，对 Shadowsocks 客户端和服务器端间的传输速度有显著提升。
而且，不同于 FinalSpeed 或 Kcptun 等需要客户端的工具，「锐速」的一大优势是只需要在服务器端单边部署就行了。换句话说，你不需要再安装另外一个应用。另外，「锐速」虽然已经停止注册和安装了，不过网上还是有不少「破解版」可用。

### 3.2 部署锐速
首先需要用 SSH 登录 VPS。

<code>ssh root@your vps ipaddress</code>

依然使用一键安装脚本，输入以下命令。

<pre><code>wget -N –no-check-certificate https://raw.githubusercontent.com/91yun/serverspeeder/master/serverspeeder-all.sh && bash serverspeeder-all.sh</code></pre>


安装需要一段时间，等待一会。如果出现下图内容，请按提示输入数字。另外，若运行中碰到提示说需要更换内核的话，请参考此文更换之。

![9.jpg](https://i.loli.net/2018/03/07/5a9f517f60327.jpg)

安装完成后，输入以下命令打开配置文件。

<code>nano /serverspeeder/etc/config</code>

将 advinacc 的 0 改为 1，保存并退出。

![10.jpg](https://i.loli.net/2018/03/07/5a9f517f5cdf7.jpg)

退出「终端」程序。
至此，整个搭建过程就大功告成了！接下来，尽情地享受起飞的速度吧😄


需要帮助的话直接联系我。