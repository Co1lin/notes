# Application Layer

Client: 发起通信的进程

Server: 等待联系的进程

Socket: 应用程序和网络之间的API (Application Programming Interface)

进程寻址：IP与port

Internet运输协议不提供的服务：定时、带宽保证。仅有：可靠数据传输、吞吐量。

## 应用层协议

是应用的一部分。如HTTP是Web应用的一部分。

## 运输层协议

### TCP

TCP建立连接之后是全双工的。

面向连接；可靠数据传输

有Congestion Control

#### SSL

Secure Sockets Layer

![SSL](../../../../Downloads/SSL.jpeg)

### UDP

无连接；不可靠数据传输

（不保证到达；可能乱序到达；无拥塞控制机制）

## Web and HTTP

Web的应用层协议是HTTP。HTTP使用TCP作为运输协议。

Stateless Protocol: 无状态协议；HTTP服务器不保存客户的任何信息。

### Connection

![image-20210225094447543](application%20layer.assets/image-20210225094447543.png)

## Email

Simple Mail Transfer Protocol, over TCP

![Screen Shot 2021-02-25 at 12.38.34 PM](application%20layer.assets/Screen%20Shot%202021-02-25%20at%2012.38.34%20PM.png)

Post Office Protocol--Version3

Internet Mail Access Protocol

## DNS

Provided by a software called Berkeley Internet Name Domain, 

on UNIX machine, 

over UDP with port 53.

![Screen Shot 2021-02-25 at 12.54.31 PM](application%20layer.assets/Screen%20Shot%202021-02-25%20at%2012.54.31%20PM.png)

TLD: Top-Level Domain

Authoritative DNS servers

Recursive Query: locahost to Local DNS Servers

Iterative Query: Other queries

Resource Record of DNS (a quadruple): (Name, Value, Type, TTL)

Time to live (different meaning from ttl in ping report)

## P2P

BitTorrent

Rarest First

DHT: Distributed Hash Table

## Streaming Media

### Dynamic Adaptive Streaming over HTTP

DASH



