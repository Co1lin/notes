# Application Layer

## DNS

### DNS 服务器分层

![Screen Shot 2021-06-04 at 3.31.31 PM](app.assets/Screen%20Shot%202021-06-04%20at%203.31.31%20PM.png)

### DNS 查询模式

域名解析过程分为递归查询和迭代查询两种。

递归查询：“靠别人”；

迭代查询：“靠自己”。

![Screen Shot 2021-06-04 at 3.30.21 PM](app.assets/Screen%20Shot%202021-06-04%20at%203.30.21%20PM.png)

本地服务器显然是递归式查询，帮助查询进程找到。

而上层的，比如根域名服务器，一般式迭代式，i.e. 不会帮请求者去查询，而是告诉它应该找谁查询。

## FTP

有两个进程，一个控制，一个传输数据。

![Screen Shot 2021-06-04 at 3.40.40 PM](app.assets/Screen%20Shot%202021-06-04%20at%203.40.40%20PM.png)

传输模式有两种：主动方式、被动方式

## Email

Simple Mail Transfer Protocol, over TCP

Post Office Protocol--Version3, over TCP

Internet Mail Access Protocol

![Screen Shot 2021-06-04 at 3.55.48 PM](app.assets/Screen%20Shot%202021-06-04%20at%203.55.48%20PM.png)

### SMTP

建立连接、发送邮件、释放连接

![Screen Shot 2021-06-04 at 4.03.57 PM](app.assets/Screen%20Shot%202021-06-04%20at%204.03.57%20PM.png)

 为了解决 SMTP 无法发送 ASCII 字符以外数据的问题，引入 Multipurpose Internet Mail Extensions （MIME）。

MIME 已经逐步应用于浏览器。

### POP3

在用户从 POP3 服务器下载邮件之后，有两种处理方式：保留或删除。

### IMAP

比 POP3 复杂。客户打开 IMAP 的邮箱时，可以看到首部。需要打开某个邮件时，邮件才上传到用户的计算机上。

IMAP 可以让用户在不同地方使用不同的计算机随时上网阅读处理邮件，还可以只读邮件的某一个部分（正文、附件）。

## World Wide Web & HTTP

Uniform Resource Locator 统一资源定位符：

- 一般形式： `<Protocol>://host:ip/path`

WWW 使用 Hyper Text Markup Language 超文本标记语言。

### Hypertext Transfer Protocol

#### Cookie

HTTP 是无状态的，于是网站需要使用 Cookie 识别用户。

Cookie 是存储在用户主机中的文本文件，记录一段时间内某用户（用户对应识别码）的访问记录。

#### 持久/非持久连接

区别在于是否每次请求资源都要先 TCP 三次握手。

![Screen Shot 2021-06-04 at 4.28.36 PM](app.assets/Screen%20Shot%202021-06-04%20at%204.28.36%20PM.png)

#### 流水线

不是请求一个资源，收到后再请求下一个；而是一次性把多个请求发出去。

HTTP 1.0：规定浏览器与服务器只保持短暂的连接，浏览器的每次请求都需要与服务器建立一个TCP连接，服务器完成请求处理后立即断开TCP连接，服务器不跟踪每个客户也不记录过去的请求。

HTTP 1.1：持续连接，也需要增加新的请求头来帮助实现，例如， Connection 请求头的值为 Keep-Alive 时，客户端通知服务器返回本次请求结果后保持连接； Connection 请求头的值为 close 时，客户端通知服务器返回本次请求结果后关闭连接。 HTTP 1.1 还提供了与身份认证、状态管理和 Cache 缓存等机制相关的请求头和响应头。

 #### 报文格式

![Screen Shot 2021-06-04 at 4.40.00 PM](app.assets/Screen%20Shot%202021-06-04%20at%204.40.00%20PM.png)

e.g.

```
GET /index.html HTTP/1.1
Host: www.gov.cn
Connection: Close
Cookie: 123456
```

