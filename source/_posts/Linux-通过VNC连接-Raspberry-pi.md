---
title: Linux 通过VNC连接 Raspberry pi
categories: Raspberry pi
abbrlink: 52173
date: 2018-05-26 22:31:05
tags:
---
玩 Raspberry pi 没有显示器 ？那就试试用你的电脑屏幕来显示啊。
<!--more-->
#### 1. 通过SSH 登录 Raspberry pi
```sh
~$ ssh pi@192.168.1.103
pi@192.168.1.103's password:
```
#### 2. 在 Raspberry pi端安装 VNC server
```sh
pi@raspberrypi:~ $ sudo apt-get install tightvncserver  
```
#### 3. 开启 Raspberry pi 端 VNC server(首次需要设置密码)
```sh
pi@raspberrypi:~ $ sudo vncserver :2   

You will require a password to access your desktops.

Password: 
Verify:   
Would you like to enter a view-only password (y/n)? n
A VNC server is already running as :2
```
#### 4. 在Linux 端安装 VNC viewer 客户端
 我的 Ubuntu 16.04 默认有这个软件
 ```sh
 sudo apt-get install xtightvncviewer
 ```
 
 #### 5. 在Linux 端开启 VNC viewer 并连接Raspberry pi
 ```sh
 vncviewer
 ```
 我的是弹出图形界面，
 输入 Raspberry pi  IP地址和服务端口号，比如我的
 ```sh
 192.168.1.103:2
 ```
 OK了.