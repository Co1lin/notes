# 6 Data Link Layer

_Click on a tile to change the color scheme_:

<div class="tx-switch">
  <button data-md-color-scheme="default"><code>default</code></button>
  <button data-md-color-scheme="slate"><code>slate</code></button>
</div>

<script>
  var buttons = document.querySelectorAll("button[data-md-color-scheme]")
  buttons.forEach(function(button) {
    button.addEventListener("click", function() {
      var attr = this.getAttribute("data-md-color-scheme")
      document.body.setAttribute("data-md-color-scheme", attr)
      var name = document.querySelector("#__code_0 code span:nth-child(7)")
      name.textContent = attr
    })
  })
</script>

## 向网络层提供的服务

(1) 无确认的无连接服务：事先不建立逻辑连接；出错不检测不恢复。e.g. 以太网

(2) 有确认的无连接服务：发送的每一帧接到后单独确认；e.g. 802.11 (WiFi)

(3) 有确认的**有连接**服务：

- 建立连接

- 每一帧编号；确保恰好收到一次；

- 适用于：长距离不可靠；e.g. 卫星；（可以想象丢失了确认可能导致一个帧被收发多次，因而将浪费带宽。）

- 三个阶段：

  - 建立连接：初始化变量、计数器
  - 传输数据帧
  - 释放连接：释放变量、缓冲区等资源

## 成帧

检错、纠错；拆分比特流以成帧

### 字节计数法

- 用头部中的一个字段来标识该帧中的字符数（后面跟着的帧的字符数）
- 计数值有可能因为一个传输错误而被弄混

### 字节填充的标志字节法

- 每个帧用标志字节（flag byte）作为开始和结束
- 解决数据中出现FLAG：ESC转义字节
- 数据中出现ESC：仍然用ESC转义；<u>在接收方，第一个转义字节ESC被删除，而留下紧跟在它后面的一个数据字节</u>
- 缺点：只能使用 8 比特的字节；帧只能以8字节为单元
### 比特填充的标志比特法
  - 帧的划分可以在比特级完成
  - 每个帧的开始和结束是标志字节：01111110（Ox7E）
  - 每当发送方在数据中遇到连续五个1，它便自动在输出的比特流中填入一个比特0。
  - 当接收方看到 5 个连续比特 1 ，后面跟一个比特0，它就自动剔除（即删除）比特 0 。
  - 缺点：如果数据中标志字节太多，则帧的长度大幅增加。
### 物理层编码违禁法

- 核心思想:选择的定界符不会在数据部分出现
- 4B/5B编码方案：
  - 4比特数据映射成5比特编码，剩余的一半码字(16个码字)未使用，可以用做帧定界符
  - 例如: 00110组合不包含在4B/5B编码中，可做帧定界符

## 检错

### 奇偶校验

增加1位校验位，可以检查奇数个位错误

- 偶校验：保证1的个数为偶数个
- 奇校验：保证1的个数为奇数个

### Checksum 校验和

![Screen Shot 2021-04-17 at 12.19.34 AM](6-Data%20Link%20Layer.assets/Screen%20Shot%202021-04-17%20at%2012.19.34%20AM.png)

### CRC 循环冗余校验

![Screen Shot 2021-04-17 at 12.22.00 AM](6-Data%20Link%20Layer.assets/Screen%20Shot%202021-04-17%20at%2012.22.00%20AM.png)

## 纠错

Codeword：n位码字，m个消息位和r个校验位，$n = m + r$。

Hamming distance：两个码字中不相同的位的个数

原理：空间稀疏：对 r 校验位，可能的报文中只有很少一部分（$1/2^r$）是合法的。

Hamming distance为d的编码方案：最短的两个合法的编码之间的距离为d

- 为了检查出d个错，可以使用海明距离为 **d+1** 的编码
- 为了纠正d个错，可以使用海明距离为 **2d+1** 的编码
  - 合法码字之间的距离足够远，即使发生了 d 位变化，结果也还是离它原来的码字最近。这意味着在不太可能有更多错误的假设下，可以唯一确定原来的码字。


设计纠错码的要求：

- m个信息位，r个校验位，n=m+r位码字；最多错d位。

- 有效信息：$2^m$个；
- 每一个n位纠错码，包括正确的，实际中可能会因为出错变成 $\Sigma_{k = 0}^d\C_n^k$ 种形式。
- 保证n位纠错码能够表示上述所有不同的码字：$2^m \cdot \Sigma_{k = 0}^d\C_n^k \leq 2^n$。
- 由上式可解出纠错所需的校验位的个数的**下界**。

### Hamming Code

#### 编码

![Screen Shot 2021-04-17 at 12.33.44 AM](6-Data%20Link%20Layer.assets/Screen%20Shot%202021-04-17%20at%2012.33.44%20AM.png)

缺省为偶校验

#### 纠错

![Screen Shot 2021-04-17 at 12.35.27 AM](6-Data%20Link%20Layer.assets/Screen%20Shot%202021-04-17%20at%2012.35.27%20AM.png)

- 组1、2共同定位列

- 组3、4共同定位行

整个过程：

- 每个码字到来前，接收方计数器清零；
- 接收方检查每个校验位k（k = 1, 2, 4 ...）的奇偶值是否正确（每组运算）；
- 若校验位奇偶值不对，计数器加 k；
- 最后，若计数器值为0，则码字有效；若计数器值为j，则第j位出错；

## 基本协议

### Utopia

- Simplex 单工
- 不考虑接收能力

### Stop-and-wait P2

- Half Duplex 半双工
- 接收能力有限，有可能overwhelming
  - Sender：发一帧暂停；等待ACK（ACK帧：dummy frame）
  - Receiver：接收后ACK

### Stop-and-wait P3

- 考虑错误
- Timer：超时未收到ACK重传
- SEQ：（最小序号1bit）
  - 发送/接收方一开始初始化帧序号（期待帧序号）0
  - 接收方由此判断这是个新帧还是应该被丢弃的重复帧
- 缺点：只能有一个没有被确认的帧在发送中；信道利用率很低

ARQ：Automatic Repeat request：发送方在前移到下一个数据 之前必须等待一个肯定确认

PAR：Positive Acknowledgement with Retransmission：带有重传的肯定确认

### Sliding Window Protocol

#### Basis

- Window

- Duplex

- 捎带/载答 piggybacking

- 循环重复使用有限的帧序号

- 流量控制:接收窗口驱动发送窗口的转动

  - 发送窗口：$W_T$，表示在收到对方确认的信息之前，可以连续发出的最多数据帧数
    - 已发送但未确认的帧；$[low, high]$；
    - 发送一个帧，high++
    - 接收到一个帧的ACK：low=被确认的帧的序号+1
  - 接收窗口：$W_R$ ，为可以连续接收的最多数据帧数（不给上层提交）
    - high：允许接收的最大序号；low：希望收的最小序号；窗口外的丢弃

- 累计确认：对按序到达的最后一个分组发送确认

- 链路利用率$\leq {w \over 1+2BD}$；$BD = { bandwidth \cdot delay \over bit~ size~ of~ frame }$

#### 1-bit SWP P4

- $W_T = W_R = 1$
- 效率低

#### GBN P5

- $W_R = 1, W_T \leq 2^n - 1$（序号空间-1）
- 接收：收到一个出错帧或乱序帧时，丢弃所有的后继帧，并且不为这些帧发送确认（或反向确认NACK）
- 发送：超时后，重传所有未被确认的帧

#### SR P6

- $W_R > 1, W_T \leq 2^{n - 1}$
- 每个PDU一个Timer

## 数据链路协议实例

### PPP (Point-to-Point Protocol)

#### 成帧

![Screen Shot 2021-04-17 at 10.24.12 AM](6-Data%20Link%20Layer.assets/Screen%20Shot%202021-04-17%20at%2010.24.12%20AM.png)

- Address字段：这个字段总是被设置为二进制值 11111111，表示所有站点都应该接受该帧
- Control：默认值是00000011，此值表示一个无编号帧。（Internet几乎都是采用一种“无编号模式”来提供无连接无确认的服务。）
- Address 和 Control 总是取默认配置的常数，因此 LCP 提供了某种必要的机制，允许通信双方就是否省略这两个字段进行协商，去掉的话可以为每帧节省 2 个字节的 空间。

字节填充：

- Sender：0x7E开始；Payload中出现0x7E，则用转义字节 0x7D 去填充，然后将紧跟的那个字节与 0x20 进行 XOR 操作，如此转义使得第 6 位（第6低的位）比特反转。
- Receiver：扫描搜索 0x7D，发现后立即删除；然后用 0x20 对紧跟在后面的那个字节进行 XOR 操作
- 两个帧之间只需要一个标志字节

### PPPoE (Point-to-Point Protocol over Ethernet)

#### 目的

- 提供Ethernet上的PPP链接
- 实现身份验证、加密、压缩
- 实现访问控制、计费、业务类型分类，运营商广泛支持

#### 状态转移

![Screen Shot 2021-04-17 at 10.36.49 AM](6-Data%20Link%20Layer.assets/Screen%20Shot%202021-04-17%20at%2010.36.49%20AM.png)

- 一开始DEAD，不存在物理层连接；
- 物理层连接建立，转移到ESTABLISH；交换LCP报文进行PPP选项协商；
- 协商成功，进入AUTHENTICATE；成功，则进入NETWORK，然后发NCP配置网络层参数（e.g. For IP Protocol, assign IP Address）；
- 进入OPEN，数据传输；完成传输，进入TERMINATE；当物理层连接被舍弃后回到 DEAD 状态。

