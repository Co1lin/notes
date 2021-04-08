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

A netmask is a shorthand for **describing a range of IP addresses**.

The left hand side of a netmask (e.g. `192.168.0.1`) specifies a the host IP address. The right hand side specifies (e.g. `/32`) how many digits of the host address are significant, when considered as a binary number. **Non-significant bits in the binary form are treated as a wild-card.**

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

![Screen Shot 2021-02-28 at 9.42.24 PM](4-network%20layer.assets/Screen%20Shot%202021-02-28%20at%209.42.24%20PM.png)

#### IPv4 Datagram Fragmentation





