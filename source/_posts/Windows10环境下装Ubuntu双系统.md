---
title: Windows10环境下装Ubuntu双系统
tags: Ubuntu
categories: Geeker
abbrlink: 56114
date: 2018-03-08 01:15:44
---
很多入门的小伙伴都想体验一下Linux，但又不舍得Windows,所以就想到了装双系统。我刚开始也是这样，大一开始折腾，虽然网上教程很多，但我第一次仔仔细细地按照教程做，最后还是没能成功，还把Windows上资料给弄没了。这两年也装过不少次系统了，所以今天想写一下教程，按照这个教程请放心安装，一定可以成功。

#### 环境/工具
* windows10
* U盘（大于4G）
* [Ultraiso](https://cn.ultraiso.net/xiazai.html)
* ubuntu 16.04 LTS [(百度云下载)](https://pan.baidu.com/s/1PPSasUqv29sf0z1euPMQVQ)
* 耐心

#### 方法/步骤<!--more-->
##### 一. 制作Ubuntu 16.04启动盘
1.插入用来制作启动盘的U盘（会被格式化，请备份好重要文件），打开UltraISO刻录软件（免费无限期试用）。<br>
![1.png](https://i.loli.net/2018/03/07/5aa005924c794.png)

2.选择“文件(F)”->“打开”，找到“Ubuntu-16.04-desktop-amd64.iso”镜像文件，然后点击“打开”。<br>
![2.png](https://i.loli.net/2018/03/07/5aa00592968e0.png)
3.选择“启动(B)”->“写入硬盘映像”，打开启动盘制作界面。<br>
![3.png](https://i.loli.net/2018/03/07/5aa00592225d2.png)
4.然后点击下方的“写入”，会弹出警告提示，确定后，就会开始制作启动盘。写入完成后关闭UltraISO软件即可。<br>
![4.png](https://i.loli.net/2018/03/07/5aa0059224ede.png)

这样就制作好了启动盘。

#### 二. 为Ubuntu系统分盘
在Windows 10中打开“磁盘管理器”，找一个空闲的磁盘分区，压缩出来一部分空间给Ubuntu使用，压缩出来的硬盘应处于未分配状态。或者通过删除某个不使用的本地磁盘使其处于未分配状态。<br>
<b>敲黑板:</b>这个可能有人不太理解，其实很简单，如果你只有C盘，右击选择压缩磁盘，大小就是你要分出来的ubuntu系统的大小，想要好好用linux的同学最好分出大于40G。分出来后这部分是黑色的。<br>
![5.png](https://i.loli.net/2018/03/07/5aa007c2d8368.png)

#### 三. [BIOS设置]
不同电脑进入BIOS方法不同，一般 F2键和 ESC键的比较多，可以试一下，不行的话可以自己百度。<br>
1.关机，重新打开电脑，进入BIOS，关闭Windows系统的快速启动（Fast Boot）选项，即设置为Disable状态。<br>

![6.png](https://i.loli.net/2018/03/07/5aa0087dcfff5.png)
2.在BIOS中设置U盘为第一启动项，保存并重启电脑。<br>
<b>不知道怎么设置的看这里</b>:上图中的Boot Option #1就是第一启动项，按ENTER键和上下键选择你的U盘启动项后ENTER，然后保存后重启。

#### 四. [安装Ubuntu 16.04 LTS]
1. 这时候开机后会看到<b>Try Ubuntu</b>或者<b>Install Ubuntu</b>,选择<b>Try Ubuntu</b>，也就是第一个，Enter。<br>
2. 稍等一会儿进入UbuntuDesktop。<br>
 ![7.png](https://i.loli.net/2018/03/07/5aa00af1ece68.png)
3. 双击左上角的”Install Ubuntu 16.04LTS“，打开安装界面。（安装过程比较简单，根据提示输入一些信息即可）<br>
![8.png](https://i.loli.net/2018/03/07/5aa00b458a074.png)
  在左侧语言栏选择安装语言，然后点击“继续”。<br>
![9.png](https://i.loli.net/2018/03/07/5aa00b45a41b2.png)
<b>如果网速比较快</b>，可以勾选“安装Ubuntu时下载更新”。（如果选择的语言是中文，这里在更新的时候会自动安装中文输入法，当然也可以安装完成后安装搜狗拼音输入法Linux版）<br>
![10.png](https://i.loli.net/2018/03/07/5aa00b4637f25.png)
<b>注意注意，重点来了,一定要选择最后一个<code>其他选项</code></b>,不要有疑问，为了你的Windows安全。<br>
接下来要为Ubuntu系统分区，就像Windows下的C、D、E盘。网上有不同的分法，我的使用经验是分三个就行，分别是/boot、/swap和 /。  /boot我是分1G，swap分5G，剩下的全部分给/。<br>
![18.png](https://i.loli.net/2018/03/08/5aa00cdc68ab8.png)
我没截到图，图片是网上找的，就是先点分出来标注空闲的那个分区，然后点左下角<code>+</code>号，/boot /swap / 都是在挂载点里面选择。选择逻辑分区。把三个都分好。<br>
<b>特别注意</b>,分好后看最下面的<b>安装启动引导器的设备</b>,我第一次错就是在这里，这个要选择/boot 对应的sda，不然会失败。<br>
![11.png](https://i.loli.net/2018/03/07/5aa00b45c7793.png)
继续<br>
![12.png](https://i.loli.net/2018/03/07/5aa00b46509d2.png)
shanghai 继续<br>
![13.png](https://i.loli.net/2018/03/07/5aa00b4653926.png)
继续<br>
![14.png](https://i.loli.net/2018/03/07/5aa00b4636485.png)
名字、密码配好      
继续<br>
![15.png](https://i.loli.net/2018/03/07/5aa00b4695197.png)
耐心等待<br>
![16.png](https://i.loli.net/2018/03/07/5aa00b468e36c.png)<br>
![17.png](https://i.loli.net/2018/03/07/5aa00b4614c81.png)
可以拔掉U盘了，立即重启。<br>
如果此时黑屏的话不要紧张，电源键重启，开机后就会看到选择进入系统界面了，第一个就是Ubuntu系统，第三个是Windows，第二个也是Ubuntu，先不用管。<br><br>
然后，好好折腾你的Ubuntu吧！！！