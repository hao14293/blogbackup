---
title: Ubuntu 16.04 开机输入密码后一直退回到输入密码界面
categories: Linux
abbrlink: 3185
date: 2018-05-21 22:39:07
tags:
---
刚才准备写篇博客，开机却发现 输入密码后黑屏一下就退回到输入密码界面。<br>
<b>这里先说解决方法:</b>
```sh
ctrl+alt+F1 切换到 tty
sudo rm -r .Xauthority
sudo reboot
```
<!--more-->
开机后就解决问题了，不过原来配置的桌面没了，需要重新配置一下，其他的东西都还在，不用紧张。