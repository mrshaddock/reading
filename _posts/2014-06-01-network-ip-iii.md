---
layout: post
title: 计算机网络-网络层(三)
categories: 计算机网络
tag: 网络层
---

### 8. ICMP

**为了更有效地转发IP数据报和提高交付成功的机会，在网际层使用了网际控制报文协议ICMP。ICMP允许主机或路由器报告差错情况和提供有关异常情况的报告。**

ICMP报文有两种：

1. ICMP差错报告报文
2. ICMP询问报文

因为ICMP报文是放在IP数据报的数据中，而IP首部的检验和不检查数据部分，所以ICMP出错的情况，首部检验和是无法检查到的。

对于ICMP差错报告报文，一共有5种类型：

1. 终点不可达：当路由器或主机不能交付数据时，就会向源点发送终点不可达报文
2. 源点抑制：当路由器或主机因为拥塞而丢弃数据时，就会给源点发送源点抑制报文，源点就知道要放慢发送数据的速率
3. 时间超过：首部中有个生产时间，超过的情况，会向源点发送时间超过报文，并丢弃数据报（分片的情况会全部丢弃）
4. 参数问题：首部数据有问题时，就会发送参数问题报文
5. 改变路由（重定向）：路由器把改变路由报文发送给主机，让主机知道下次发送给另外的路由器（可通过更好的路由）

对于ICMP询问报文，一共有2种类型：

1. 回送请求和回答：由主机或路由器向特定的目的主机发出的询问。目的主机必须进行答复。这种询问常用于询问终点是否可达及了解有关状态
2. 时间戳请求和回答：用于同步时间。是从1900年1月1日开始的秒数

### 9. ICMP的应用举例

ICMP的一个重要应用就是**分组间检测——ping(Packet InterNet Groper)**，用来测试两个主机之间的连通性。**ping使用了ICMP回送请求与回送回答报文。ping是应用层直接使用网络层ICMP的一个例子。它没有通过运输层的TCP和UDP。**

另一个非常有用的命令是traceroute（UNIX的命令哦），**它用来跟踪一个分组从源点到终点的路径**。在windows下的命令是tracert。原理是酱紫滴：

> Traceroute从源主机向目的主机发送一连串的IP数据报，数据报中封装的是无法交付的UDP用户数据报（提供非法端口号即可）。第一个数据报P1的生存时间TTL是1。当P1到达第一个路由器R1时，R1先收下它，接着把TTL减1。由于TTL=0，R1就会把P1丢弃，然后发送一个ICMP的差错报告报文——时间超过报文。以此类推，当最后一个数据报刚刚到达目的主机时，数据报的TTL是1.这时候**因为数据达到了目的地址，但是终点不可达（端口非法），所以会发送一个终点不可达的ICMP差错报告报文[不是时间超过ICMP差错报告报文]。

这样，源主机就达到了自己的目的，因为它不仅知道了路径中经过的路由器和目的主机的IP，也知道了经过他们的往返时间。注意的是，对于每个TTL值，源主机会发送三次同样的IP数据报。

### 10. 路由选择

这一节主要是讲了路由选择协议，那么，路由选择算法必须是重中之重啊。但是有几点：

* 网络规模太大：考虑到网络实在是太大了，不说主机，就路由器全世界大概就有上千万个，所以，路由算法将是整个网络性能的关键。
* 隐私性：比如很多军工或者企业，不希望自己的网络布局或其他信息被外界了解，但同时也需要连入互联网

所以，粗略来看，路由算法必须要跟网络一样分片管理，比如分成多个子网，而对于路由来说，这种做法叫做**自治系统**，俗称AS。

当然了，和局域网有相同的一点是：无论一个AS内部采用何种路由选择都是可以的，只要能和外部有个公用的接口进行通信就OK了。根据这个原则，因特网把路由选择协议分为两大类：

1. 内部路由协议：在AS内部使用的路由选择协议，这与互联网中其他AS使用的路由选择协议无关。目前使用最多是RIP和OSPF协议
2. 外部路由协议：若源主机和目的主机不在一个AS内部（可能这两个AS内部使用不同的路由选择协议），那传递数据用到的就是外部路由协议，常用的是BGP的版本4（BGP-4）

下面先介绍一下内部路由协议：

1. RIP：这个协议非常简单，说白了就是维护一个向量，这个向量的值是到达其他路由器的距离，所以每个路由器的路由表都是不同的（因为距离不同），所以就需要定时进行更新，具体的不多介绍，只需要记住核心：定时刷新、距离，**算法思路为Bellman-Ford**
2. OSPF：这个使用的是**Dijkstra算法的最短路**，所以每个路由器的路由表都是整个网络的拓扑结构。而这里有一点特别之处：因为路由器要保存全网路由，那么势必表太大。于是设计者将一个AS内部划分为几个区域，每个区域作为OSPF的基本单位，而各个区域之间必定有通信员，AS之间也必定有通信员。这个就不多说了。

然后再介绍一下外部路由协议BGP-4（下面简称BGP）：

BGP力求寻找一条能够到达目的网络且比较好的路由（不能兜圈子），而并非要寻找一条最佳路由。BGP采用了**路径向量路由选择协议**

每个AS都需要配置一个BGP发言人，两个AS要交换路由信息，就要建立TCP连接（端口号为179），然后在此连接上交换BGP报文以建立BGP会话，使用TCP是因为路由信息要可靠。

### 11. 路由器的构造

这个话说还真的不清楚呀。**路由器是一个具有多个输入接口和多个输出接口的专用计算机，其任务就是转发分组。**从路由器的输入端口收到的分组，按照目的网络的不同，发送到不同的输出端口（有没有很像邮局？）转发到下一跳路由器。所有路由器的工作以此类推，直到终点为止。**路由器的转发分组正是网络层的主要工作**。

话说路由器的构造分为两个部分：

1. 路由选择：核心是路由选择处理机。任务就是维护路由表
2. 分组转发：输入端口、交换结构、输出端口

重点在于**分组转发**，对于输入、输出端口，里面都有3层结构：物理层、数据链路层、网络层。物理层处理比特流，数据链路层接收数据帧，网络层需要根据IP数据报的首部判断里面是路由器之间的更新路由表的RIP/OSPF数据报还是数据分组，若是路由表相关的，就送到路由选择处理机；若是数据分组，就通过交换结构得出的转发表转发到合适的输出端口。

对于交换结构，一定要保证分组转发的速度，目前有3种方法：

1. CPU存储器，采用中断模式得到输出端口
2. 总线：可以理解为串行
3. 从横交换：相当于并行

Tips：
> 前面IP数据报首部有一个溢出字段，就和这里的输入输出端口的网络层有关，里面都一个队列，若数据发送太快，队列越长，空间越小，等空间使用殆尽的时候，就会发生溢出。

### 12. IP多播

这一点没有仔细看，但是大概轮廓很容易理解。举个例子说明吧：

> 一台服务器，100台客户端。服务器要给客户端发送一个通知，如果使用点对点通信，需要发送100个IP数据报；如果使用IP多播，只需要发送一个IP数据报。在遇到路由器的时候，路由器负责给每条有多播需求的下一跳复制IP数据报。所以，**IP数据报需要使用多播路由器。而多播地址是D类IP地址。而且多播使用的是IGMP协议，不适用ICMP，所以若使用ping+多播IP，是无法响应的。**

### 13. VPN和NAT

这两个玩意算是常识吧。简单说一下背景：

目前互联网还是使用的IP4，而IP4有效地址已经不多了。所以，我们可以给每个机构分配一个IP地址，至于这个机构内部，可以使用内部有效、外部无效的IP地址。就比如每个家庭都有爸爸妈妈，但是出门时候都用独一无二的名字。目前有3类公认的专用地址：

* 10.0.0.0-10.255.255.255
* 172.16.0.0-172.16.255.255
* 192.168.0.0-192.168.255.255

要记住的是，这些地址的数据只会出现在局域网中，永远不会出现在因特网中。路由器收到专用地址的数据报会直接丢弃。

那么，VPN是神马呢？VPN的出现可以简单总结为：当一个公司在世界各地有多个office，这些office显然不可能处于一个子网中，那么就势必要通过因特网来进行通信。但是，公司内部为了安全起见，肯定不希望自己的信息（数据报）被他人劫持，所以需要建立一条公司内部的“专用通道”进行通信。

当然，解决方法可以让电信拉一条专线，但是这个成本过于高。另外一种，就是VPN，建立一种“虚拟”的专用线路，既保证安全，又保证了彼此的通信。实现起来也简单，需要VPN路由器和VPN软件来保证数据的安全性（当然，需要设置硬件/软件让它知道其它VPN节点的存在及详细信息）。这样，通过软硬件的设置，就可以让任何网络的主机进行透明的“专用网络”的通信。

而NAT更简单了，因为一个网络内分配的IP地址是专用地址，无法和因特网的主机通信。那么，通过NAT软件，就可以将内部主机伪装成具有真实IP的主机。但是，**要和因特网的主机通信，必须是内部发起的通信。因为外部的主机是无法知道内部IP的，它只认识路由器的公网IP哦。**
