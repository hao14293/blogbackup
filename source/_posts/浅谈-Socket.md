---
title: 浅谈　Socket
tags: socket
categories: web
abbrlink: 858
date: 2018-02-13 00:27:48
---
### 1. Socket 基础
　　Socket原指“孔”或“插座”，它最初作为BSD UNIX的进程通信机制，通常被称作“套接字”。当然，如今Socket已经是Windows和Mac等其他操作系统所共同遵守的网络编程标准，用于描述IP地址和端口，是一个通信连的句柄，可以用来实现不同虚拟机或不同计算机之间的通信。Internet上的主机一般运行了多个服务软件，同时提供几种服务，每种服务都打开一个Socket，并绑定到一个端口上，不用的端口对应不同的服务。

　　在操作系统结构上，Socket为应用程序屏蔽了TCP/IP网络传输层及以下的网络细节，如图所示。Socket为操作系统的用户空间提供网络抽象，开发者编写的网络程序都会直接或间接地用到Socket抽象。通过Socket抽象可以控制传输层协议TCP和UDP,甚至包括部分网络层协议，例如IP和ICMP。
<!--more-->
![1.jpg](https://i.loli.net/2018/03/07/5a9f4c57a9a05.jpg)
Socket使用IP地址＋端口＋协议的三元组唯一标识一个通信链路。服务器端的一个通信链路可以对应于多个客户端，比如一个Web服务器端的80端口可以同时服务于大量的客户端。

### 2. 实战演练: Socket TCP 原语
　　用Socket进行网络开发需了解服务器和客户端的Socket原语，每个原语在不同的高级语言中都有相应的实现方式。TCP的Socket原语如图所示，所有基于TCP的Socket通信都遵循如图所示的流程。下面解释每个原语的含义。
![2.jpg](https://i.loli.net/2018/03/07/5a9f4c57cc091.jpg)
<code>socket():</code> 建立Socket对象。Socket是以类似文件系统的文件系统【打开、读写、关闭】的模式设计的，socket()原语相当于“打开”。socket()原语的参数通常包括使用的传输层协议类型、网络层类型等。

<code>bind():</code> 绑定。在参数中需要传入要绑定的IP地址和端口。IP地址必须是主机上的一个可用的地址（除了0.0.0.0指定绑定所有的本机IP）。端口必须是一个该Socket协议未被占用的端口，比如当一个主机上的两个程序试图同时绑定到80端口时，只有一个程序能够成功。<b>服务器端程序在listen()之前必须进行bind()操作</b>,而客户端程序如果在connect()原语之前没有调用bind()，则系统会自动为该Socket分配一个未被占用的地址和端口。

<b>技巧: 当主机上存在多个IP时，绑定地址0.0.0.0可以监听所有这些可用的IP。</b>

<code>listen():</code>监听。只在服务器端有用，告诉操作系统开始监听之前绑定的IP地址和端口，可以下参数中指定允许排队的最大连接数量。

<code>connect():</code>在客户端连接服务器。参数中需要指定服务器的地址和端口。调用connect()可能有两种结果，即与服务器端完成TCP３次握手并建立连接或者连接服务器失败。

<code>accept():</code>接受连接。只在服务器端有用，从监听到的连接中取出一个，并将其包装成一个新的Socket对象。这个新的Socket对象可被用于和相应的客户端进行通信。完成accept()标志着Socket已经完成TCP链路建立阶段的3次握手。如果当前没有客户端连接请求，则accept()调用会阻塞等待。

<code>send():</code>发送数据。服务器端和客户端均可调用send()向对方发送数据，在sendI)的参数中传入要发送的数据，通过send()的返回值判断数据是否发送成功。

<code>recv():</code>接收数据。服务器端和客户端均可调用recv()从对方接收数据。如果Socket中没有消息可以读取，则在默认情况下recv()调用会被阻塞直到消息到达；开发者也可以将Socket()设置为非阻塞模式，使recv()以失败形式返回。

<code>close():</code>关闭连接。通信中的任何一方可以调用close()发起关闭连接请求，另一方收到后也可调用close()关闭连接。

【示例】下面通过Python代码演示Socket编程方法，TCP服务器端的代码如下：

<pre><code>#!/usr/bin/env python
#-*- coding: utf-8 -*-

import socket
import datetime

HOST='0.0.0.0'
PORT=3434

#AF_INET说明使用IPV4地址，　SOCK_STREAM指明TCP
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind((HOST,PORT))
s.listen(1)

while True:
    conn,addr = s.accept()
    print 'Client %s connected!' % str(addr)
    dt = datetime.datetime.now()
    message = "Current time is " + str(dt)
    conn.send(Sent: ", message
    conn.close()</code></pre>
包socket封装了所有python的原生Socket操作，代码中通过socket(),bind(),listen()的一系列调用实现了对指定端口的监听，通过accept()接受客户端的连接，当有客户端连接成功后将当前系统时间发送给客户端，并马上关闭连接。因为代码主体处于while循环中，所以程序将不断监听并一直运行。


　　与该服务器端的代码相对应的客户端代码如下：

<pre><code>#!/usr/bin/env python
#-*- coding: utf-8 -*-

import socket

HOST = '127.0.0.1'
PORT = 3434

 #AF_INET说明使用IPV4地址，　SOCK_STREAM指明TCP协议
 s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

  s.connect((HOST, PORT))
  print 'Connect %s:%d OK!' % (HOST, PORT)
  data = s.recv(1024) #接受数据，最大长度为1024
  print 'Received: ', data
  s.close()</code></pre>
客户端通过connect()调用、连接服务器，连接成功后接收从服务器发来的数据，然后关闭连接、退出程序。

<b>注意：</b>客户端的Socket端口号由系统自动分配。

### 3.Socket UDP原语
UDP相对于TCP在传输层提供更少的控制，没有建立连接、断开连接等概念，所以基于UDP的Socket通信过程也比TCP稍微简单。在UDP中可以直接指定IP:Port进行数据收发。UDP Socket可以复用TCP中的socket()和bind()原语，除此之外，UDP属于自己的Socket原语如下。

<code>recvfrom():</code>从绑定的地址接收数据。

<code>sendto():</code>向指定的地址发送数据，在调用的参数中应该传入通信对端的地址和端口。


【示例】UDP的python服务器端的代码示例如下：

<pre><code>  #!/usr/bin/env python
#-*- coding: utf-8 -*-

import socket

HOST = '0.0.0.0'
PORT = 3434

#AF_INET说明使用IPV4地址，　SOCK_DGRAM指明UDP
s = socket.socket(socket.AF_INET,socket.SOCK_DGRAM)
s.bind()

while True():
    data, addr = s.recvgrom(1024)
    print 'Received: %s' % (data, str(addr))

s.close()</code><pre>
代码通过socket()和bind()调用绑定了本地所有地址的3434端口，通过socket()中的SOCKET_DGRAM指定Socket使用UDP, 在一个循环中不断地接收数据并打印。相应的UDP客户端python代码如下：

<pre><code>#!/usr/bin/env python
#-*- coding: utf-8 -*-

import socket

HOST = '127.0.0.1'
PORT = 3434

#AF_INET说明使用IPV4地址，socket.SOCK_DGRAM指明UDP
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

data = "Hello UDP!"
s.sendto(data, (HOST, PORT))
print "Sent: %s to %s:%d" % (data, HOST, PORT)

s.close()</code><pre>

