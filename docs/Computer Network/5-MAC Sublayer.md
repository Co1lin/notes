# 5 MAC Sublayer

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

需要解决的问题：多方竞争信道使用权时如何确定谁可以使用信道。





## 数据链路层交换原理

数据链路层设备**扩充**网络

冲突域的个数 https://blog.51cto.com/u_2225558/2312623

网桥：

方式：存储转发

作用：分割冲突域；连接不同类型的局域网

### 透明网桥

无需配置

#### 逆向学习

![Screen Shot 2021-04-14 at 10.35.51 AM](5-MAC%20Sublayer.assets/Screen%20Shot%202021-04-14%20at%2010.35.51%20AM.png)

MAC地址表的构建：逆向学习源地址：

主机**向外发送数据时**，其MAC地址就会被学习

老化时间（默认300s）（应对设备位置变更）

总结：增（帧的源地址不在表中）；删（老化时间到期）；改（帧的源地址在表中，更新时间戳）

#### 泛洪 Flooding

将从某个接口收到的数据流向除该接口之外的所有接口发送出去。

两种目的地址的帧，需要泛洪：

- 广播帧:目的地址为FF-FF-FF-FF-FF-FF的数据帧
- 未知单播帧:目的地址不在MAC地址转发表中的单播数据帧

### 链路层交换机

分类：多端口透明网桥；POE（Power Over Ethernet）交换机

交换模式1：存储转发：转发前接收整个帧，CRC校验；延迟高

交换模式2：直通交换：一旦接收到帧的目的地址，就开始转发；延迟低

交换模式3：**无碎片转发**：接收到帧的前64字节，即开始转发；<u>延迟中，但是过滤了冲突导致的碎片帧</u>

对称交换/非对称交换：出入带宽是否相同

### 生成树协议

增强稳定性：冗余拓扑；物理环路引发了以下问题：

- 广播风暴：交换机在网桥上无休止地flooding，无限循环
- 重复帧：X发送到环路的单播帧，造成目的设备Y收到重复的帧
- MAC地址表不稳定：当一个帧的多个副本到达不同端口时，交换机会不断修改同一MAC地址对应的端口

解决冗余拓扑中的传输问题：构建无环生成树

- 收发BPDU：
  - 根桥ID
  - 根路径开销
  - 指定桥ID
  - 指定端口ID
- 选举：
  - 根桥
    - 同一广播域中的全部交换机参与选举
    - 桥ID最小的
    - 根桥所有端口都处在转发状态
  - 为每个非根桥选出一个根端口（开销最小；kaixiao哦  IEEE根据）
  - 为每个网段指定端口
    - 一个具有最小根路径开销的端口，作为该网段的指定端口
    - 指定端口处于转发状态，负责该网段的数据转发
    - 连接该网段的其他端口，若既不是指定端口，也不是根端口，则阻塞（显然，根桥的所有端口都是指定端口）

当由交换机(网桥)或链路故障导致网络拓扑改变时，重新构造生成树

快速生成树协议(Rapid Spanning Tree Protocol, RSTP)

如何寻找最优路径？

#### 源路由网桥



### 虚拟局域网

广播域：

交换机划分VLAN，分隔广播域

区分不同VLAN的数据帧的方式：

- 数据帧中携带VLAN标记
- VLAN标记由交换机添加/剥除，对终端透明

![Screen Shot 2021-04-14 at 11.11.56 AM](5-MAC%20Sublayer.assets/Screen%20Shot%202021-04-14%20at%2011.11.56%20AM.png)

## 无线局域网

### 802.11 物理层

#### MAC

CSMA/CA(Carrier Sense Multiple Access with Collision Avoid)

![Screen Shot 2021-04-14 at 11.36.37 AM](5-MAC%20Sublayer.assets/Screen%20Shot%202021-04-14%20at%2011.36.37%20AM.png)

