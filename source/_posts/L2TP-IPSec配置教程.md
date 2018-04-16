---
title: L2TP/IPSec配置教程
tags: vpn
categories: Geeker
abbrlink: 49666
date: 2018-03-19 19:45:59
top: 99
---
![google-gif.gif](https://i.loli.net/2018/03/19/5aafb0ce0960b.gif)
<!--more-->
上次介绍过[在服务器上配置shadowsocks](http://shiyi.today/year/02/07/57013/)的方法，但这种方法必须要有shadowsocks客户端才能使用，在android和windows下还好，都有客户端可以下载，但在Linux下就比较麻烦了。而我用的正是Linux。所以今天就又配置了L2TP/IPSec，这种连接更加安全，速度也更快，连接更简单。<br>
<br>
服务器依然用的上次介绍的[Vultr](https://www.vultr.com/?ref=7325742)，配置的话请看[上篇教程](http://shiyi.today/year/02/07/57013/)。<br>
<br>
下面进入正题。<br>
这次我们采用的是Github上[setup-ipsec-vpn](https://github.com/hwdsl2/setup-ipsec-vpn)这个脚本安装方法，不需要怎么配置，傻瓜式安装。<br>
先<code>update</code>一下<br>
<b>Ubuntu Debian</b>是<code>sudo apt-get update</code><br>
<b>CentOS</b>是<code>yum  update</code><br><br>
然后<br><br>
<b>Ubuntu LTS, Debian 系统</b>，使用下列这行代码：
```sh
wget https://git.io/vpnsetup -O vpnsetup.sh && sudo sh vpnsetup.sh
```
<b>CentOS 系统</b>,使用这行代码:
```sh
wget https://git.io/vpnsetup-centos -O vpnsetup.sh && sudo sh vpnsetup.sh
```
然后就等待安装好。<br>
安装好后，会自动分配 <code>Username，Password，Pre-Shared key</code>,这些随机数会让人崩溃。别担心，可以修改的。<br>
<b>Ubuntu、Debian</b>复制、运行以下代码:
```sh
wget https://git.io/vpnsetup -O vpnsetup.sh
nano -w vpnsetup.sh
[Replace with your own values: YOUR_IPSEC_PSK, YOUR_USERNAME and YOUR_PASSWORD]
sudo sh vpnsetup.sh
```
<b>CentOS</b>是下面代码:
```sh
wget https://git.io/vpnsetup-centos -O vpnsetup.sh
nano -w vpnsetup.sh
[Replace with your own values: YOUR_IPSEC_PSK, YOUR_USERNAME and YOUR_PASSWORD]
sudo sh vpnsetup.sh
```
把 <code>username passward Pre-shared key</code>分别改成你想设置的。<br>
这样服务器端就配置好了。<br>
* * *
##### 下面配置客户端
先说比较麻烦的Linux吧。<br>
我的系统是Ubuntu 16.04 Lts，所以这里只介绍Ubuntu系统的。<br>
这里只把代码贴出来，<b>依次复制运行</b>就行。<br>
```sh 
sudo apt install intltool libtool network-manager-dev libnm-util-dev libnm-glib-dev libnm-glib-vpn-dev libnm-gtk-dev libnm-dev libnma-dev ppp-dev libdbus-glib-1-dev libsecret-1-dev libgtk-3-dev libglib2.0-dev xl2tpd strongswan
```
```sh
git clone https://github.com/nm-l2tp/network-manager-l2tp.git
```
```sh
cd network-manager-l2tp
```
```sh
autoreconf -fi    
intltoolize
```
```sh
./configure --disable-static --prefix=/usr --sysconfdir=/etc --libdir=/usr/lib/x86_64-linux-gnu --libexecdir=/usr/lib/NetworkManager --localstatedir=/var --with-pppd-plugin-dir=/usr/lib/pppd/2.4.7   
```
```sh
make    
sudo make install
```
```sh
sudo apparmor_parser -R /etc/apparmor.d/usr.lib.ipsec.charon    
sudo apparmor_parser -R /etc/apparmor.d/usr.lib.ipsec.stroke
```
```sh
sudo apt remove xl2tpd    
```
```sh
sudo apt install libpcap0.8-dev  
wget https://github.com/xelerance/xl2tpd/archive/v1.3.6/xl2tpd-1.3.6.tar.gz    
tar xvzf xl2tpd-1.3.6.tar.gz    
cd xl2tpd-1.3.6    
make    
sudo make install 
```
这样就好了。点wifi那个标志，编辑连接，增加，VPN里的L2TP，网关是你服务器地址，用户名、密码是你自己设的，再点IPsec设置，ID是你服务器地址，Pre-sharef key是你自己设置的。然后确定。这时候再看你的VPN连接里面就有了，点击连接就OK了。<br><br>


##### Windows 10 and 8.x
1. 右键单击系统托盘中的无线/网络图标。<br>
2. 选择 打开网络与共享中心。<br>
3. 单击 设置新的连接或网络。<br>
4. 选择 连接到工作区，然后单击 下一步。<br>
5. 单击 使用我的Internet连接 (VPN)。<br>
6. 在 Internet地址 字段中输入你的 VPN 服务器 IP。<br>
7. 在 目标名称 字段中输入任意内容。单击 创建。<br>
8. 返回 网络与共享中心。单击左侧的 更改适配器设置。<br>
9. 右键单击新创建的 VPN 连接，并选择 属性。<br>
10. 单击 安全 选项卡，从 VPN 类型 下拉菜单中选择 "使用 IPsec 的第 2 层隧道协议 (L2TP/IPSec)"。<br>
11. 单击 允许使用这些协议。确保选中 "质询握手身份验证协议 (CHAP)" 复选框。<br>
12. 单击 高级设置 按钮。<br>
13. 单击 使用预共享密钥作身份验证 并在 密钥 字段中输入你的 VPN IPsec PSK。<br>
14. 单击 确定 关闭 高级设置。<br>
15. 单击 确定 保存 VPN 连接的详细信息。<br>
注： 在首次连接之前需要修改一次注册表。请参见下面的说明。<br>

##### Windows 7, Vista and XP
1. 单击开始菜单，选择控制面板。<br>
2. 进入 网络和Internet 部分。<br>
3. 单击 网络与共享中心。<br>
4. 单击 设置新的连接或网络。<br>
5. 选择 连接到工作区，然后单击 下一步。<br>
6. 单击 使用我的Internet连接 (VPN)。<br>
7. 在 Internet地址 字段中输入你的 VPN 服务器 IP。<br>
8. 在 目标名称 字段中输入任意内容。<br>
9. 选中 现在不连接；仅进行设置以便稍后连接 复选框。<br>
10. 单击 下一步。<br>
11. 在 用户名 字段中输入你的 VPN 用户名。<br>
12. 在 密码 字段中输入你的 VPN 密码。<br>
13. 选中 记住此密码 复选框。<br>
14. 单击 创建，然后单击 关闭 按钮。<br>
15. 返回 网络与共享中心。单击左侧的 更改适配器设置。<br>
16. 右键单击新创建的 VPN 连接，并选择 属性。<br>
17. 单击 选项 选项卡，取消选中 包括Windows登录域 复选框。<br>
18. 单击 安全 选项卡，从 VPN 类型 下拉菜单中选择 "使用 IPsec 的第 2 层隧道协议 (L2TP/IPSec)"。<br>
19. 单击 允许使用这些协议。确保选中 "质询握手身份验证协议 (CHAP)" 复选框。<br>
20. 单击 高级设置 按钮。<br>
21. 单击 使用预共享密钥作身份验证 并在 密钥 字段中输入你的 VPN IPsec PSK。<br>
22. 单击 确定 关闭 高级设置。<br>
23. 单击 确定 保存 VPN 连接的详细信息。<br>
注： 在首次连接之前需要[修改一次注册表](https://documentation.meraki.com/MX-Z/Client_VPN/Troubleshooting_Client_VPN#Windows_Error_809)，以解决 VPN 服务器 和/或 客户端与 NAT （比如家用路由器）的兼容问题。<br>

要连接到 VPN： 单击系统托盘中的无线/网络图标，选择新的 VPN 连接，然后单击 连接。如果出现提示，在登录窗口中输入 你的 VPN 用户名 和 密码 ，并单击 确定。最后你可以到 这里 检测你的 IP 地址，应该显示为你的 VPN 服务器 IP。<br>
##### OS X
1. 打开系统偏好设置并转到网络部分。<br>
2. 在窗口左下角单击 + 按钮。<br>
3. 从 接口 下拉菜单选择 VPN。<br>
4. 从 VPN类型 下拉菜单选择 IPSec 上的 L2TP。<br>
5. 在 服务名称 字段中输入任意内容。<br>
6. 单击 创建。<br>
7. 在 服务器地址 字段中输入你的 VPN 服务器 IP。<br>
8. 在 帐户名称 字段中输入你的 VPN 用户名。<br>
9. 单击 鉴定设置 按钮。<br>
10. 在 用户鉴定 部分，选择 密码 单选按钮，然后输入你的 VPN 密码。<br>
11. 在 机器鉴定 部分，选择 共享的密钥 单选按钮，然后输入你的 VPN IPsec PSK。<br>
12. 单击 好。<br>
13. 选中 在菜单栏中显示 VPN 状态 复选框。<br>
14. 单击 高级 按钮，并选中 通过VPN连接发送所有通信 复选框。<br>
15. 单击 TCP/IP 选项卡，并在 配置IPv6 部分中选择 仅本地链接。<br>
16. 单击 好 关闭高级设置，然后单击 应用 保存VPN连接信息。<br>
要连接到 VPN： 使用菜单栏中的图标，或者打开系统偏好设置的网络部分，选择 VPN 并单击 连接。<br>

##### Android
1. 启动 设置 应用程序。<br>
2. 在 无线和网络 部分单击 更多...。<br>
3. 单击 VPN。<br>
4. 单击 添加VPN配置文件 或窗口右上角的 +。<br>
5. 在 名称 字段中输入任意内容。<br>
6. 在 类型 下拉菜单选择 L2TP/IPSec PSK。<br>
7. 在 服务器地址 字段中输入你的 VPN 服务器 IP。<br>
8. 在 IPSec 预共享密钥 字段中输入你的 VPN IPsec PSK。<br>
9. 单击 保存。<br>
10. 单击新的VPN连接。<br>
11. 在 用户名 字段中输入你的 VPN 用户名。<br>
12. 在 密码 字段中输入你的 VPN 密码。<br>
13. 选中 保存帐户信息 复选框。<br>
14. 单击 连接。<br>

##### iOS
1. 进入设置 -> 通用 -> VPN。<br>
2. 单击 添加VPN配置...。<br>
3. 单击 类型 。选择 L2TP 并返回。<br>
4. 在 描述 字段中输入任意内容。<br>
5. 在 服务器 字段中输入你的 VPN 服务器 IP。<br>
6. 在 帐户 字段中输入你的 VPN 用户名。<br>
7. 在 密码 字段中输入你的 VPN 密码。<br>
8. 在 密钥 字段中输入你的 VPN IPsec PSK。<br>
9. 启用 发送所有流量 选项。<br>
10. 单击右上角的 存储。<br>
11. 启用 VPN 连接。<br>

<center>好了，在Google的世界里尽情畅游吧！！！</center>
![mmexport1521463352259.gif](https://i.loli.net/2018/03/19/5aafb11469d31.gif)
