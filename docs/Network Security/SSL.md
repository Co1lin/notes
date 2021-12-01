# Secure Sockets Layer & HTTPS

## Introduction of SSL

![Screen Shot 2021-11-27 at 9.37.27 PM](SSL.assets/Screen%20Shot%202021-11-27%20at%209.37.27%20PM.png)

SSL 在传输层的 TCP 之上，各个应用层之下，可以为各应用层提供安全服务。

SSL and TLS ：

- Developed by Netscape Communication initially in 1994
- 1999, TLS v1.0 released, which can be regarded as SSL v3.1

Do not supports UDP. (QUIC for UDP)

## Structure of SSL

### SSL Record Protocol

SSL Handshake protocol 定义了 share secret key （共享秘钥） ； SSL Record Protocol 使用之，用传统秘钥算法对 SSL payload 加密。

### SSL Handshake Protocol

在传递 data from application 之前使用

- authentication between client and server （先认证 server 身份；client 认证可选）
- 协商：密码算法； Master Secret 主会话秘钥

4-phase handshake:

1. 2*hello: Establish security capabilities: version, random number, chip suite, encryption algorithm
2. Server Authentication and Key Exchange:
    1. server sends its certificate
    2. server key exchange
    3. request certificate
3. Client Authentication and Key Exchange: 
    1. if requested, sends cert
    2. client key exchange
    3. cert verify
4. 2*Finish: change cipher spec

Master secret 不直接用于加密、认证，而是产生一系列秘钥： MAC 、加密、 IV

### SSL 告警协议

两个字节：

1. 是否告警
2. 消息出错的严重程度（若致命，则立即终止连接；会话中的其它连接继续，但在此会话中不再建立新连接）

### SSL 修改密码规约协议

握手结束时发送，通知接收方，以后的记录将使用刚才协商的密码算法和密钥进行加密/认证

## Security Analysis of SSL

建立在 RSA 的安全性上

防范“重放攻击”：每一次安全连接有一个 128 bit 的随机的连接序号。由于 attackers 无法提前知道此序号，因此不能对服务器的请求做出正确应答。

## Security Threat faced by WEB

主动攻击：伪装成其它用户、篡改通信内容、篡改 Web 站点内容

被动攻击：窃听

Man-In-The-Middle Attack (MITM): 中间人与 client and server 分别建立联系

MITM Attack Example: ARP Spoofing: 伪造 ARP frame ，告诉他人错误的 IP-MAC 映射关系，切断被欺骗者的通讯

DNS Spoofing: ARP Spoofing 之后可以做 （伪造假的 DNS replies）

SSL Strip

