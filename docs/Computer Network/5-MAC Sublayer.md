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
![Screen Shot 2021-04-17 at 10.55.02 AM](5-MAC%20Sublayer.assets/Screen%20Shot%202021-04-17%20at%2010.55.02%20AM.png)

## 信道分配

需要解决的问题：多方竞争信道使用权时如何确定谁可以使用信道。

### 静态分配

- 方法：TDM、FDM
- 适用范围：用户少；数目固定；通信量大；流量稳定
- 不适用：突发性业务；
- 缺点：资源分配不合理，有浪费，效率低；延迟时间增大N倍（N为划分的子信道个数）

### 动态分配

#### ALOHA

##### 纯ALOHA

- 想发就发，随时发
- 冲突的判断方式：
  - 每个站在给中央计算机发送帧之后，该计算机把该帧重新广播给所有站。因此，那个发送站可以侦听来自集线器的广播，以此确定它的帧是否发送成功。
  - 类比：SSH终端输入，看到的是远程反馈回来的接收的结果。
- 信道效率评估：
  - 假设新生成的帧加上需要重发的老帧的数量符合Poisson Distribution，每帧时**平均帧数为G**。
  - 吞吐量 S 就是负载 G 乘以成功传输的概率：$S = GP_0$ （$P_0$是这一帧没有遭受冲突的概率）。
  - 给定的帧时内生成了k帧的概率：$Pr[k] = {G^k e^{-G} \over k!}$。不冲突即为k=0，概率为$e^{-G}$。
  - 考虑某一帧（图中阴影），其可能与前面的或者后面的帧时中发出的帧冲突。前后两帧加起来生成0帧的概率为$e^{-2G}$。![Screen Shot 2021-04-17 at 12.07.58 PM](5-MAC%20Sublayer.assets/Screen%20Shot%202021-04-17%20at%2012.07.58%20PM.png)
  - 吞吐量：$S=Ge^{-2G}$。最大值：$S = {1 \over 2e} \approx 0.184$, where $G = 0.5$。

##### 分槽 ALOHA

![Screen Shot 2021-04-17 at 12.11.13 PM](5-MAC%20Sublayer.assets/Screen%20Shot%202021-04-17%20at%2012.11.13%20PM.png)

- 改善：敲回车之后，必须等到下一个时槽的开始时刻才发送。
- 在测试帧所在的同一个时间槽中没有其他流量的概率是$e^{-G}$，$S = G e^{-G}$。

#### Carrier Sense Protocol 载波侦听

##### 1-persistent Carrier Sense Multiple Access

- 发之前先侦听信号
  - 其它站在发，等待直到信道空闲
  - 空闲，发一帧（1-persistent：发现信道空闲时发送的概率为1）
  - 冲突：等待一段随机的时间，然后再从头开始上述过程

##### Non-persistent Carrier Sense Multiple Access

- 如果信道忙，并不是持续侦听然后一到空闲就立即发送
  - 等待一段随机时间，重复以上过程
- 缺点：延时++

##### p-persistent Carrier Sense Multiple Access

- 如果信道空闲，发送概率为p

比较：

![Screen Shot 2021-04-17 at 12.24.40 PM](5-MAC%20Sublayer.assets/Screen%20Shot%202021-04-17%20at%2012.24.40%20PM.png)

##### CSMA with Collision Detection (CSMA/CD)

- 被Ethernet采用

- 如果一个站检测到冲突，它立即中止自己的传送，等待一段随机时间，然后再重新尝试传送。

如何确定竞争周期的长度？

- 假设两个相距最远的站传播信号所需要的时间为$\tau$。
- $t_0$时一个站开始发
- $t_0 + \tau - \epsilon$，即信号到达最远那个站之前的一刹那， 那个站也开始传输
- 最远的那个站立即检测到冲突，停止
- 冲突引起的微小噪声尖峰要到$t_0 + 2\tau - \epsilon$才能回到原来那个站

综上，一个站传输了$2\tau$之后还没有监听到冲突，它才可以确保自已经seize了信道（即知道其他站知道自己在传因此不会干扰自己）。

![Screen Shot 2021-04-17 at 12.40.07 PM](5-MAC%20Sublayer.assets/Screen%20Shot%202021-04-17%20at%2012.40.07%20PM.png)

缺点：电缆很长而帧的长度又很短时，冲突不仅降低了带宽，而且使得发送一个帧的时间变得动荡不定。

#### 无冲突协议

##### 位图协议

![Screen Shot 2021-04-17 at 12.47.59 PM](5-MAC%20Sublayer.assets/Screen%20Shot%202021-04-17%20at%2012.47.59%20PM.png)

- 竞争期：举手示意，资源预留
- 传输期：按序发送

- 利用率：
  - 低负荷/非均衡时：d/(d+N) （站点越多，N越大，利用率越低）
  - 在高负荷条件下：d/(d+1)，接近100%
- 缺点：无法考虑优先级



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

