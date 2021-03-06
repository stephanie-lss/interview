# 计算机网络

## 计算机网络的帧结构
参考[TCP/IP 知识点整理](http://strawhatfy.github.io/2015/07/30/TCP-IP-Protocol/)
### MAC层

#### 802.3 以太网的帧结构
![](/assest/img/8023frame.png)

头部14 字节 尾部4字节 共 18字节 MTU 1500 最小都是46byte 不够的话会被填充到46 byte

* premble 前导码，用于同步
* sfd 帧开始符 表明下个字节开始时真实的数据
* 目的mac
* 源mac
* 长度 
* type 表明帧中的上层数据类型比如ip arp rarp
* fcs 帧校验序列 fcs 常用的算法时用crc 循环冗余校验填充 发送端先把数组按照一定划分大小划分为组，然后进行CRC32 得到冗余码

#### 802.11 无线局域网的帧结构
![](/assest/img/ieee80211mpu.png)
其中 四个 addr 分别是 `目的mac` `源mac` `接收端地址` 负责处理该帧的无线工作站 即BSSID `发送端地址` 将帧传送至无线媒介的无线接口，中继时用到的mac也就是下一跳mac
FCS(Frame Check Sequences) 帧校验序列
详细内容参考[IEEE 802.11 序列帧](http://rungame.me/blog/2016/06/23/wlan/)

### IP 报文的帧结构
![](/assest/img/ip_package.png)

* 版本号 4bit 4 代表 IPv4 
* IP头部的长度 4bit 计量单位是4字节，由于只有四位所以最大的首部的长度为 15 * 4 = 60
* 服务类型 8bit 有四位为 TOS(type of services)字段用于优先级控制，包括 最小时延 最大吞吐量 最高可靠性 最小费用 四位最多只能有一位置一
* IP数据包长度 16bit 计量单位 byte ，计量的范围包括头部和数据部分
* 数据包序号 16bit 每发送一个就会增加一，直到65535 然后又从零开始，该字段用于分片时候的恢复
* 分片标识字段 3bit 标志 第0bit 保留 DF：是否可以分片 MF：是否是最后一个分片 13 bit 偏移字段，标识当前分片在所属数据包中的位置，以8byte为计量单位
* TTL 8bit
* 协议 8bit 标识上层用到的协议
* 首部检验和 16bit 验证首部是否正确
* 32bit 源地址和32bit 目的地址
* 以32bit为计量单位的可选字段，如果不满32 必须以零填充到32bit，应为 IP头部的长度是以4byte 32bit 为计量单位的

* 由于头部 较长，应该巧计，除去源ip和目的ip就会有下面一些内容 **2协议（ip协议版本和上层协议）2序号 （包序号和分片序号） 2长度（首部长度和总长度） 3标识（TTL TOS 是否分片） 1校验（首部校验和）*** 

### 传输层

#### TCP 的帧结构
![](/assest/img/tcp_segment.png)

**2 端口（源端口和目的端口） 2长度（首部长度和窗口长度） 2 序号（包序号和确认序号） 1校验和（包括头部和数据） 1 紧急指针 最后还有  6 标识**

URG，紧急指针有效。
ACK，确认序号有效。
PSH，接收方应该尽快将这个报文段交给应用层。
RST，重建连接。
SYN，同步序号用来发起一个连接。
FIN，发送端完成发送，用来结束一个连接。

#### UDP 的帧结构
udp 的帧头就比较简单，两个 端口，一个长度，一个校验和

### 应用层 

#### HTTP 的帧结构

##### HTTP/1.1

`request-line`
GET /index.html HTTP/1.1

`headers`
User-Agent：产生请求的浏览器类型。
Accept：客户端可识别的内容类型列表。
Host：请求的主机名，允许多个域名同处一个IP地址，即虚拟主机。

`blank-line`
空行 内容 为 `CR` + `CF` 0x0d+0x0a

`request-body`
请求包体

##### HTTP/2
参考 [HTTP/2笔记之帧](http://www.blogjava.net/yongboy/archive/2015/03/20/423655.html)

### ICMP
帧结构 ： 类型 代码 校验和
#### ping
请求： 类型值 8  回送请求
应答： 类型值 0  回送应答
#### traceroute
设置TTL 并封装无法到达的udp 报文
类型值 3 目的不可达


## 计算机网络中常用的端口号
* 20 ftp 数据
* 21 ftp 控制
* 22 ssh
* 23 telnet
* 25 smtp
* 53 dns
* 67 dhcp 服务器
* 68 dhcp 客户端
* 69 udp tftp
* 80 tcp http
* 109 tcp pop2
* 110 tcp pop3
* 123 udp ntp
* 443 tcp https
