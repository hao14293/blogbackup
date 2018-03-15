---
title: Git push失败
categories: Git and Github
abbrlink: 36673
date: 2018-03-10 11:24:22
tags:
---
刚才在将本地文件夹上传到github上时出现下面报错
```sh
$ git push -u origin master
fatal: unable to access 'https://github.com/hao14293/blogbackup.git/': gnutls_handshake() failed: Error in the pull function.
```
解决方法
```sh
$ git config --global credential.helper store
```
然后再次
```sh
$ git push -u origin master
```
输入<code>Username</code> <code>Password</code>就完美解决了。

