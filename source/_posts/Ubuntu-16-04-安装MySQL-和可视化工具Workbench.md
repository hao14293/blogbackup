---
title: Ubuntu 16.04 安装MySQL 和可视化工具Workbench
categories: Linux
abbrlink: 53258
date: 2018-06-01 22:24:22
tags:
---
<hr>
<!--more-->
<hr>

### 1.可以直接在官方下载.deb包安装:
[https://dev.mysql.com/downloads/](https://dev.mysql.com/downloads/)

<br>不过更推荐用命令行安装
### 2.Terminal 命令行安装
```sh
sudo apt-get install mysql-server  //安装过程中需要设置密码
sudo apt-get install mysql-client   //可以省略，第一步中已经安装了mysql-client
sudo apt-get install libmysqlclient-dev
```

然后使用如下命令检查是否安装成功：
```sh
sudo netstat -tap | grep mysql
```
### 3.安装 MySQL workbench
下载地址:  [https://dev.mysql.com/downloads/workbench/](https://dev.mysql.com/downloads/workbench/)
```sh
sudo dpkg -i mysql*.deb       //下载好后安装
sudo apt-get install -f       //安装依赖包
sudo dpkg -i mysql*.deb       //再次运行安装包即可完成安装
```
<hr>