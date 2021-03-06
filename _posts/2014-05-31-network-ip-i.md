---
layout: post
title: 计算机网络-网络层(一)
categories: 计算机网络
tag: 网络层
---

## 1. 前言

因为最近遇到了几个网络问题，但是对细节记不清楚了。于是趁端午放假简单复习一下网络层的知识。当然，学习一个知识最科学有效的是先从整体把握，所以先推荐两篇非常好的文章：

* [互联网入门（一）](http://www.ruanyifeng.com/blog/2012/05/internet_protocol_suite_part_i.html)
* [互联网入门（二）](http://www.ruanyifeng.com/blog/2012/06/internet_protocol_suite_part_ii.html)

看完之后，应该对网络有个整体的把握，下面从细节部分进行学习。


## 2. 背景知识

网络通信和电话网络有一定的相似性，所以怎样构建计算机网络也引发了一场讨论，重点在于：

* 可靠的交付应该由谁保证？

根据电话网络的经验，可靠交付应该由网络保证。但最后计算机网络没有采用这种方式，原因如下：

> 传统的电信网络目的是通话。所以电信使用了复杂的程控机和软件来保证可靠的数据传输。而且这种数据传输是面向连接的通信方式。所以根据这种经验，计算机也应该采用这种方式，先建立一个虚连接，以保证通信双方拥有一切网络资源，然后双方在这一条“专用”网络上进行通信。但因特网的先驱们却提出了一种崭新的网络设计思路，它们认为电话网络中之所以采用网络保证可靠传输是因为电话作为一个端设备太简单了，没有任何其他的功能。**而计算机作为一个强大的端系统，完全可以保证数据的检验相关工作，而网络只要负责传输数据即可。这样就位因特网的设计减少了巨大的工作量**。事实证明，这种设计思路是划时代的，正是使用了这种架构，因特网才发展到现在的规模。

所以，总结一下就是：

> 网络层只负责向上层提供最简单的、无连接的、尽最大努力交付的数据报服务（可能丢失、重复、无序、出错）。而对数据报的正确性，都放在端系统进行。

## 3. 虚拟互联网络

很简单的概念，但是却非常重要。因为通络通信有非常多的方式，比如有线网、无线网、卫星网络等等，但如果我们对互联网的通信有一个统一的标准，那么不管一个网络的内部是怎样实现的，只要和其它网络进行通信时采用这个标准，就可以进行无障碍通信。从用户角度来看，他们根本觉察不到两个网络的存在，他们会认为所有用户都处于一个网络之中。

一般来说，将网络互联起来要使用一些中间设备，根据中间设备所在层次，可以分为4种：

* 物理层——转发器（repeater）
* 数据链路层——网桥或桥接器（bridge）
* 网络层——路由器（router）
* 网络层以上——网关（gateway）

当中间设备是转发器或者网桥时，他们仅仅是把一个网络的主机数目扩大了，但本质仍是一个网络，所以称不上网络互连。而网关由于比较复杂，所以目前用的也比较少。因此，在讨论网络互联时都是指**用路由器进行网络互联和路由选择**。

## 4. 网际协议IP

网际协议IP是TCP/IP体系中最重要的协议之一，也是最重要的因特网协议之一。与IP协议配套使用的还有4个协议：

* 地址解析协议ARP
* 逆地址解析协议RARP
* 网际控制报文协议ICMP
* 网际组管理鞋机IGMP

其中，IP处在4个协议的中间位置。IP使用ARP/RARP来完成从IP地址到MAC地址的转换；ICMP和IGMP使用IP协议完成IP数据包的？？？？？

### 1. IP地址

IP地址是因特网上每个主机的唯一的、无重复的标识符。IP地址的编址方法经过了3个历史阶段：

1. 分类的IP地址：最基本的编址方法，早在1981年就通过了标准协议
2. 子网的划分：最基本的编址方法的改进，在1985年通过了标准RFC 950
3. 构成超网：无分类的编址方法。1993年提出后迅速推广使用

我们先从分类的IP说起，当然根据发展，现在分类的IP地址已经不再使用了，但是它是整个IP地址编址的根源。

* A类地址：0开头，网络号为1个字节
* B类地址：10开头，网络号为2个字节
* C类地址：110开头，网络号为3个字节
* D类地址：1110开头，多播地址
* E类地址：1111开头，保留以后使用

### 2. 一个问题

localhost/127.0.0.1/本机IP三者的区别是神马？

* localhost：一个域名，默认指向127.0.0.1。可到/etc/hosts查看，一般作为第一行。
* 127.0.0.1/8：A类地址中，网络号全为1（01111111）保留作为**本地软件环回测试（loopback test)本主机的进程之间的通信只用**。若主机发送一个目的地址为环回地址（例如127.0.0.1）的IP数据报，则本主机中的协议软件就处理数据报中的数据，而不会把数据报发送到任何网络。**目的地址为环回地址的IP数据报永远不会出现在任何网络上，因为网络号为127的地址根本不是一个网络地址。**
* 本机IP：对于目前的PC来说，本机一般有3块网卡：虚拟网卡(loopback)、ethernet（有线网卡）、wlan（无线网卡）。

了解了它们的定义，我们就可以回答这个问题了：

> localhost是一个域名，它被互联网默认指向为127.0.0.1；而127.0.0.1/8是一个环回测试地址，用来测试本机的TCP/IP协议栈，发往这段A类地址的数据包不会出网卡，而是在虚拟网卡（loopback）上，所以网络中不会出现；而本机IP是真实网卡的地址，具体来说，你连接有线，有线网卡的地址就是主机IP；你连接无线，无线网卡的地址就是主机IP；当然，你有多个网卡的情况你就有多个主机IP喽。

更具体的可以去知乎上讨论这个问题：[localhost/127.0.0.1/本机IP有什么区别？](http://www.zhihu.com/question/23940717)

### 3. IP地址的特点

1. 每个IP地址都是由网络号和主机号组成。好处是分配时候不需要管主机号；网络路由的时候只添加网络号，减少路由表大小，加快路由选择的过程
2. IP地址是一个主机和一个链路的接口，所以当想连接多个网络时，就需要多个网络地址。因为路由器也是一个主机，只是它功能不同，它因为要完成网络互连的任务，所以它至少应当有两个不同的IP地址，同理它也至少含有两个MAC地址
3. 通过转发器、网桥连接起来的局域网如果拥有相同的网络号，name它们仍然是一个网络。所以具有不同网络号的局域网必须通过路由器进行网络互连

### 4. IP地址和硬件地址

如果看了阮一峰的互联网入门的那2篇文章，这个问题就应该知道答案了。说到底就是用ARP和RARP来完成IP地址和MAC地址的转换。

那么，我们就来说说ARP和RARP的原理。但因为现在广泛使用的DHCP协议包含了RARP协议，所以单独使用RARP已经很少了。故这里只介绍ARP协议，RARP协议我们知道是把物理地址转换成IP地址就行了。

ARP协议的功能就是完成IP地址到MAC地址的转换。我们每台主机都有一个ARP高速缓存（可通过```arp -a```查看本机的ARP缓存），当A主机是新入网或者刚加电的情况下，ARP缓存是空的。

1. 如果它想给B主机通信，A主机的ARP进程会先给**本局域网**的所有主机广播发送ARP请求，请求内容大概是：“我的IP是A，硬件地址是A1；我想知道IP是B的主机的是硬件地址”。
2. 本局域网的所有主机都受到该ARP请求
3. 主机B看到这个请求，就会给A发送ARP响应“我的IP是B，我的硬件地址是B1”，同时为了以后跟A通信方便，也会将A的IP和对应的硬件地址写入到自己的ARP缓存。而其他主机都会忽略这个ARP请求
4. 主机A收到ARP响应后，将B的IP和MAC地址写入到自己的ARP缓存

如上粗体字所示，ARP是工作在**局域网**中的，如果主机A想知道其他网络的主机B的IP，ARP是无能为力的。那怎么解决这个问题呢？

> 其实，根本不需要存在这样的困扰。因为有IP地址的存在。假如A网络的一台主机想和B网络的一台主机通信，那么首先会根据IP判断是否为同一个网络，如果是，那么直接使用ARP即可；如果不是，**只需要知道路由器的ARP地址即可**，因为这个数据包会经手路由器转发，路由器会完成数据包的交接。因为路由器也是一个主机，所以当路由器把数据包路由到下一个网络时，也会判断IP是否属于这个网络，进而完成和上面相同的选择。

那么，总结一下，ARP的使用场景有下述4种：

* 发送方是主机，要把IP数据报发送到本网络的另一台主机：用ARP找到对应的硬件地址
* 发送方是主机，要把IP数据报发送到其他网络的主机：用ARP找到本网络的路由器硬件地址，剩下工作交给路由器完成
* 发送方是路由器，要把IP数据报发送到本网络的主机：用ARP找到对应的硬件地址
* 发送方是路由器，要把IP数据报发送到其他网络的主机：用ARP找到本网络的路由器硬件地址，剩下工作交给路由器完成

### 5. IP数据报格式

一个IP数据报分为两部分：

* 首部：前一部分是固定长度，20字节，所有IP数据报必备
* 数据：可选字段，长度可变

下面先说一下首部都啥玩意吧：

* 版本：4位，是IP4还是IP6
* 首部长度：4位，单位是4字节，如果是1111，那么首部长度就是15*4字节=60字节
* 区分服务：8位，一般用不到
* 总长度：16位，首部和数据的长度。所以一个IP数据报的最大长度是2 << 16 - 1 = 65535字节
* 标识：16位，IP软件产生一个数据报，计数器就会加1.但是因为网络层是无连接的、无序的，所以这个号码不是为了标识IP数据报的前后顺序。**而是当数据报的长度超过网络的MTU后分片后，这个值就会复制给所有的数据片中，标识这是一个数据报，只是长度超限分片处理了。**
* 标志：3位，目前只有2位有意义。**最低位MF（More Fragment），1表示后面还有分片；0表示是数据报的最后一个分片。中间一位DF(Do not Fragment），意思是不能分片。只有当DF=0时的时候才能分片**
* 片偏移：13位，指出每个分片在未分片的原数据报中的偏移位置。以8个字节为偏移最小单位，**所以，每个分片的长度一定是8字节的整数倍**
* 生存时间：8位，防止无效数据报在网络中兜圈子。所以每经过一个路由器，就减去数据报离开上个主机消耗的时间，小于0就抛弃这个数据报
* 协议：8位，标明IP数据报中数据使用何种协议，以便使目的主机的网络层知道应该将数据交给哪个处理过程。常见的有ICMP/TGMP/UDP/TCP/IPV6
* 首部检验和：16位，只检验首部，不检验数据。检验方法使用了很简单的算法：把首部划分为16位的片，然后把这些片用反码加起来存入初始化全0的检验和字段，然后接收方收到用反码再加一遍，如果没出错就是0.非0肯定出错了。（TCP可能会使用这个东东吧）
* 源地址：32位
* 目的地址：32位
* 扩展字段：1-40字节，选择功能

### 6. 路由器转发流程

每个路由器中都有一个路由表，路由表的每一项大概是：

> （目的网络地址，下一跳地址）

* 特定主机路由：一般而言，因特网都是基于目的主机所在网络进行路由选择，但某些特殊情况下也可以指定路由路线。比如安全部门，必须走的是自己经过加密的路由，那么我就指定数据报不管是不是走长路都要走我设定的路由路线，这就叫做**特定主机路由**
* 默认路由：当发现目的网络不在自己的路由表中，就会走默认路由

那么，我们就要思考：IP数据报中只有源IP和目的IP，又没有下一跳IP，那么，IP数据报是怎么选择路由路线呢？

> 当路由器收到一个待转发的IP数据报时，会从路由表得出下一跳IP地址，**它不是把IP写入IP数据报中，而是送交下层的网络接口软件。网络接口软件负责把下一跳IP地址转换成MAC地址（使用ARP协议），并放入数据链路层的MAC帧中。所以，实际上IP数据报在发送过程中，源IP和目的IP是不变的，因为他们是逻辑地址，是抽象出来的。而实际改变的是MAC地址，他们是物理地址，实际网络就是根据物理地址进行路由的。**

而且，我们要理解为什么不直接使用MAC地址，因为使用抽象的IP地址，就隐藏了底层的细节，便于分析解决问题。虽然在IP地址转换成MAC地址过程中浪费了一定的时间，但总体上还是利大于弊。

根据上述过程，可以总结出路由选择算法的步骤：

1. 从数据报首部得出目的IP地址D，得出目的网络地址为N
2. 若N就是该数据报所在的网路，就进行直接交付，不需要其他路由器的帮助（这里包括使用ARP，并在数据链路层封装成MAC帧等）；否则执行步骤（3）
3. 若路由表含有N的特定主机路由，则使用特定主机路由；否则执行（4）
4. 若路由表含有网络N的路由，则把数据报转发到下一跳指定的路由器；否则执行（5）
5. 若路由表中有默认路由，则把数据报转发到默认路由指定的下一跳网络；否则执行（6）
6. 报告分组转发出错
