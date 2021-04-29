# 4 Network Layer

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

Forwarding (data plane) and routing (control plane)

Control plane: how to determine the forwarding table? Traditional approach or SDN (Software defined networking) approach.

## Inside a router

![Screen Shot 2021-02-28 at 12.53.35 PM](4-network%20layer.assets/Screen%20Shot%202021-02-28%20at%2012.53.35%20PM.png)

Destination-based forwarding or generalized forwarding

The forwarding table is copied from the routing processor to the line cards over a separate bus. Forwarding decisions can be made locally to avoid centralized processing bottleneck.

### Input Port Processing & Destination-Based Forwarding

Lookup:

longest prefix matching rule

on-chip DRAM and faster SRAM; Ternary Content Addressable Memories (TCAMs)

#### Netmask

Ref: https://www.hacksplaining.com/glossary/netmasks

A netmask is a shorthand for **describing a range of IP addresses**, e.g. `192.168.0.1/32`.

The left hand side of a netmask (e.g. `192.168.0.1`) specifies a the host IP address. The right hand side specifies (e.g. `/32`) how many digits of the host address are significant, when considered as a binary number. **Non-significant bits in the binary form are treated as a wild-card.**

Apparently, the left part of a subnet mask consists of **consecutive 1**, and the right part consists of **consecutive 0**.

#### Routing Table

Ref: https://en.wikipedia.org/wiki/Routing_table

| Network destination |     Netmask     |    Gateway    |   Interface   | Metric |
| :-----------------: | :-------------: | :-----------: | :-----------: | :----: |
|       0.0.0.0       |     0.0.0.0     |  192.168.0.1  | 192.168.0.100 |   10   |
|      127.0.0.0      |    255.0.0.0    |   127.0.0.1   |   127.0.0.1   |   1    |
|     192.168.0.0     |  255.255.255.0  | 192.168.0.100 | 192.168.0.100 |   10   |
|    192.168.0.100    | 255.255.255.255 |   127.0.0.1   |   127.0.0.1   |   10   |
|     192.168.0.1     | 255.255.255.255 | 192.168.0.100 | 192.168.0.100 |   10   |

- The columns **Network destination** and **Netmask** <u>together describe the **Network identifier**</u> as mentioned earlier. For example, destination **192.168.0.0** and netmask **255.255.255.0** can be written as **192.168.0.0/24**.
- The **Gateway** column contains the same information as the **Next hop**, i.e. it points to the gateway through which the network can be reached.
- The **Interface** indicates what locally available interface is responsible for reaching the gateway. In this example, gateway **192.168.0.1** (the internet router) can be reached through the local <u>network card</u> with address **192.168.0.100**.
- Finally, the **Metric** indicates the associated cost of using the indicated route.

[Forwarding Table](https://en.wikipedia.org/wiki/Routing_table#Forwarding_table)

#### The Link-State (LS) Routing Algorithm

Each node broadcasts link-state packets to *all* other nodes in the network.

All nodes have an identical and complete view of the network.

Dijkstra’s algorithm

#### The Distance-Vector (DV) Routing Algorithm

Iterative, asynchronous, and distribute:

It is *distributed* in that each node receives some information from one or more of its *directly attached* neighbors, performs a calculation, and then <u>distributes the results of its calculation back to its neighbors</u>. 

It is *iterative* in that this process continues on <u>until no more information is exchanged</u> between neighbors.

The algorithm is *asynchronous* in that it does not require all of the nodes to operate in lockstep with each other.

**Bellman-Ford equation**: $dist(x, end) = \min_v(w(x, v), dist(v, end))$, while $v$ is one of x's neighbor.

The solution to the Bellman-Ford equation provides the **entries in node *x*’s forwarding table**.

With the DV algorithm, each node *x* maintains:

- the cost *c*(*x, v*) from *x* to directly attached neighbor, v
- Node *x*’s **distance vector**, that is, $D_x=[D_x(y): y \in N]$, containing *x*’s estimate of its cost to all destinations *y*, in *N*
- Distance vectors of each of its neighbors, that is, $D_v=[D_v(y): y \in N]$ for each neighbor *v* of *x*

![Screen Shot 2021-04-08 at 11.24.26 PM](4-network%20layer.assets/Screen%20Shot%202021-04-08%20at%2011.24.26%20PM.png)

#### Routing Information Protocol

Based on UDP. Relationship and position in the network packets:

```
IP Header (e.g. 20Bytes)
  UDP Header (8 Bytes)
    RIP Packet
```

Structure of RIPv2 (Ref: [RFC 2453](https://datatracker.ietf.org/doc/rfc2453/?include_text=1)):

```
0                   1                   2                   3 
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  command (1)  |  version (1)  |       must be zero (2)        |	// header (4 Bytes)
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
| Address Family Identifier (2) |        Route Tag (2)          |	// entry follows
+-------------------------------+-------------------------------+
|                         IP Address (4)                        |
+---------------------------------------------------------------+
|                         Subnet Mask (4)                       |
+---------------------------------------------------------------+
|                         Next Hop (4)                          |
+---------------------------------------------------------------+
|                         Metric (4)                            |	// total: 24 Bytes
+---------------------------------------------------------------+
// more entries (total <= 25) may follow
```

### Switching

1) Switching via mem.

2) Switching via a bus.

3) Switching via an interconnection network.

### Output Port Processing

selecting, de-queueing, link- and physical-layer functions

### Queueing

#### Input queueing

$R_{switch} = N R_{line}$, negligible queuing

Problem: head-of-the-line (HOL) blocking (may cause unbounded queue size)

#### Output queueing

Though $R_{switch} = N R_{line}$, unbounded queue size!

#### Buffer size

Rule of thumb: $RTT \times Capacity$

(Recent: $RTT \times Capacity / \sqrt N$, where N denotes the number of TCP flows)

#### Packet Scheduling

FIFO/FCFS

Priority queueing

Round robin queuing -> weighted fair queuing (WFQ): class i will receive a fraction $w_i \over \Sigma w_j$ of the bandwidth.

## The Internet Protocol (IP)

### IPv4

Ref: [IPv4 - Wikipedia](https://en.wikipedia.org/wiki/IPv4)

![Screen Shot 2021-04-10 at 11.11.38 AM](4-network%20layer.assets/Screen%20Shot%202021-04-10%20at%2011.11.38%20AM.png)

Internet Header Length (IHL): [20, 60] Bytes

#### IPv4 Datagram Fragmentation

Maximum Transmission Unit

![Screen Shot 2021-04-21 at 11.11.58 AM](4-network%20layer.assets/Screen%20Shot%202021-04-21%20at%2011.11.58%20AM.png)

#### Classification

|                                  |     **A类IPv4地址**     |      **B类IPv4地址**      |      **C类IPv4地址**      |                 **D类IPv4地址**                 |             **E类IPv4地址**              |
| :------------------------------: | :---------------------: | :-----------------------: | :-----------------------: | :---------------------------------------------: | :--------------------------------------: |
|          **网络标志位**          |            0            |            10             |            110            |                      1110                       |                  11110                   |
|          **IP地址范围**          | 1.0.0.0~127.255.255.255 | 128.0.0.0~191.255.255.255 | 192.0.0.0~223.255.255.255 |            224.0.0.0~239.255.255.255            |        240.0.0.0~247.255.255.255         |
|        **可用IP地址范围**        | 1.0.0.1~127.255.255.254 | 128.0.0.1~191.255.255.254 | 192.0.0.1~223.255.255.254 |                                                 |                                          |
|    **是否可以分配给主机使用**    |           是            |            是             |            是             |                       否                        |                    否                    |
|        **网络数量（个）**        |       126 (27-2)        |        16384 (214)        |       2097152 (221)       |                       ---                       |                   ---                    |
| **每个网络中可容纳主机数（个）** |    16777214 (224-2)     |       65534 (216-2)       |        254 (28-2)         |                       ---                       |                   ---                    |
|           **适用范围**           |   大量主机的大型网络    |   中等规模主机数的网络    |        小型局域网         | 留给Internet体系结构委员会(IAB)使用【组播地址】 | 保留，仅作为搜索、Internet的实验和开发用 |

#### DHCP



#### Network Address Translator







## 网络层的服务方式

### 无连接服务

所有的数据包被独立送入网络，每个包独立路由，不提前建立任何设置。

路由算法

### 有连接服务

Virtual Circuit

- 逻辑连接，不同于真正的物理连接。

- 所有分组都沿着这条逻辑连接按存储转发发送。

- 转发策略：基于分组标签，i.e. 虚电路号

  - Label Switching：![Screen Shot 2021-04-21 at 10.27.18 AM](4-network%20layer.assets/Screen%20Shot%202021-04-21%20at%2010.27.18%20AM.png)
  
  R4根据输入的端口和标签查表得到出口和“标签”，R4需要将原来的标签L1替换为L2再发出去。

- 按需到达。

- 问题：链路中任何一个环节失效则经过它的所有VC失效。

- 

