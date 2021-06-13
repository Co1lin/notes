# 3 Transport Layer

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

![image-20210225163238007](3-transport%20layer.assets/image-20210225163238007.png)

Transport Layer provides different **processes** with logic communication.

Protocol of transport layer only works **in the end system**.

Transport Layer **extend** the host-to- host delivery service provided by the network layer to a **process-to-process** delivery service for applications running on the hosts.

Protocol: TCP and UDP

## Multiplexing and Demultiplexing

Port range: 16bits, 0\~65535

Well-known port numbers: 0~1023

UDP socket can be identified by a two-tuple.

TCP socket can be identified by a quadruple.

## UDP

![Screen Shot 2021-02-25 at 10.49.15 PM](3-transport%20layer.assets/Screen%20Shot%202021-02-25%20at%2010.49.15%20PM.png)

UDP socket can be identified by a **two-tuple**.

There's no source IP, so the receiver will treat UDP segments which has the same source port and dest. port in the same way, although their source IP may be different.

(Source IP is transported by the Network Layer.)

### Checksum

Ref: [UDP Checksum](https://www.securitynik.com/2015/08/calculating-udp-checksum-with-taste-of.html)

![](3-transport%20layer.assets/UDP%20Checksum%20table.jpg)

**Pseudo Header** is not transported. It only serves for computing the checksum.

If the result overflows, we need to "**wrap it around**".

Also note the "reserved" or "padding" zeros.

If the data size is not an integral multiple of 16 bits, we need to pad zeros behind.

#### Checking by Receiver

Add all data (certainly including the checksum computed by the sender) as 16 bits numbers together. If the result contains "0", the process must have some error. Even the result consists of "1", we can not be sure that the process is totally right.

## Reliable Data Transfer

ARQ, Automatic Repeat reQuest: based on pos./neg. acknowledgment

Error detection

Receiver Feedback: ACK and NAK

Retransmission

### Stop and wait

The sender have to wait ACK and NAK before leaving the waiting status and then obtaining following data from upper layer.

#### rdt 2.0

Add **ANK and NAK** to know whether the data is delivered.

#### rdt 2.1

To avoid <u>the corruption of ANK and NAK</u>, add **sequence number**.

#### rdt 3.0

To address <u>the problem of packet loss</u>, introduce the **countdown timer** which causes resending and duplicate data packet.

### Pipelining

Stop and wait has very poor efficiency. We need to stop waiting too much and send more data.

As a result, we have to:

1) Increase the range of sequence numbers.
2) (Optional) Cache in buffer.
3) Know how to respond to lost, corrupted, and overly delayed packets (分组).

#### Go-Back-N

GBN, **sliding-window protocol**.

Animation: [GBN](https://media.pearsoncmg.com/ph/esm/ecs_kurose_compnetwork_8/cw/content/interactiveanimations/go-back-n-protocol/index.html) 

**Features**: Cumulative Ack, base, nextseqnum, 

Typically, consider 2 situations of packet(s) loss: 

1) The packet of sender with sequence number $i$ is lost before it reaches the receiver. 

<u>The receiver</u> expects sequence number $i$, but it only receives $i + 1$. So, it wil drop it (not deliver to the upper layer), and send the packet of ACK with sequence number $i - 1$ to the sender. 

<u>The sender</u> receives $i - 1$, so $base == i$ will not change (cause the reaching of $i - 1$ will lead to $base = (i - 1) + 1 = i$). It will wait the ACK $i$ until timeout, and then re-send it.

2) The responsive packet ACK of the receiver with sequence number $i$ is lost before it reaches the sender.

<u>The sender</u> will not receive ACK with sequence number $i$. However, due to the feature of "Cumulative Ack", the reaching of ACK with sequence number $i + 1$ will tell the sender the successful delivery of data packet $i$. Thus, the loss of ACK with sequence number $i$ has no effect.

#### Selective Repeat

To improve the performance of GBN, because GBN will generate lots of duplicate packets when there are plenty of packets in the pipeline (may caused by the large window length or the large bandwidth-delay product).

**The difference**: The receiver will acknowledge the correctly delivered packets **regardless of the order** (sequence number). So there's **no cumulative ACK**! This leads to a big difference with GBN, which means <u>when the sender receives ACK $i$, it cannot regard $i - 1$ as having been transmitted correctly to the receiver. So the sender should wait until timeout</u>.

So it needs **cache and buffer** and **independent timers**.

The sender:

```
When receiving ACK:
		if seqnum in window:
				receive and "ACK" it!
		if seqnum == send_base:
				window move forward to min(seqnum that hasn't yet ACK'd)
```

The receiver:

```
window: [rcv_base, rcv_base + N - 1]

When receiving packet:
		if seqnum in window:
				send corresponding ACK
				if seqnum/packet is not received before:
						cache it
				if seqnum == rcv_base:
						deliver {packets whose seqnum starting with rcv_base, until the last cached one} to the upper layer
		elif seqnum in [rcv_base - N, rcv_base - 1]:
				send corresponding ACK!!!
		else:
				do nothing (ignore)
```

Note for "!!!": If doing nothing there, the window of sender will not be able to move forward, since the moving forward can only be triggered by `seqnum == send_base`. Such case will happen when an ACK is lost on the way coming back to sender, and then timeout of this "seqnum" will cause re-sending. So the receiver must respond to this re-sent packet although it has already cached it.

Issue: The window size must be **less than or equal to half the size** of the sequence number space for SR protocols!

## TCP

**three-way handshake**

**maximum segment size (MSS)**: the maximum amount of **application-layer data** in the segment, not the maximum size of the TCP segment including headers.

**maximum transmission unit, MTU**: the length of the **largest link-layer frame** that can be sent by the local sending host; <u>setting the MSS based on the path MTU value</u>

How many flags ???

![Screen Shot 2021-02-26 at 10.34.21 PM](3-transport%20layer.assets/Screen%20Shot%202021-02-26%20at%2010.34.21%20PM.png)

**Sequence number for a segment** is the serial number (序号) (in the whole byte-stream) of the first byte of the segment.

???"Ack. number" in TCP is different from "seq. num" of ACK packets in GBN or SR!!! *The acknowledgment number that Host A puts in its segment is the sequence number of the next byte Host A is expecting from Host B.* (**Cumulative Acknowledgments**)

### Telnet

![Screen Shot 2021-02-27 at 10.07.08 AM](3-transport%20layer.assets/Screen%20Shot%202021-02-27%20at%2010.07.08%20AM.png)

Note that the acknowledgment for client-to-server data is carried in a segment carrying server-to-client data; this acknowledgment is said to be **piggybacked** on the server-to-client data segment.

### RTT Estimation and Timeout Determination

SampleRTT: the amount of time between when the segment is sent (that is, passed to IP) and when

an acknowledgment for the segment is received.

2 Rules: The *SampleRTT* is being estimated for only one of the **transmitted but currently unacknowledged** segments, and for segments that have been transmitted **once**.

$EstimatedRTT=(1−α)⋅EstimatedRTT+α⋅SampleRTT$; recommended $\alpha = 0.125$

???EstimatedRTT puts more weight on recent samples than on old samples. **Exponential weighted moving average (EWMA)**.

$DevRTT=(1−β)⋅DevRTT+β⋅|SampleRTT−EstimatedRTT|$; recommended $\beta = 0.25$

**Determine** $TimeoutInterval=EstimatedRTT+4⋅DevRTT$

(Init: 1s; When a timeout occurs, the value of *TimeoutInterval* is **doubled**; Updated when *EstimatedRTT* is updated)

### Fast retransmit

??? The reason that the sending side has to wait until the third duplicate ACK is described in [RFC2001](https://www.isi.edu/nsnam/DIRECTED_RESEARCH/DR_WANIDA/DR/testfiles/rfc2001.html) as follows:

> " Since TCP does not know whether a duplicate ACK is caused by a lost segment or just a reordering of segments, it waits for a small number of duplicate ACKs to be received. It is assumed that if there is just a reordering of the segments, there will be only one or two duplicate ACKs before the reordered segment is processed, which will then generate a new ACK. If three or more duplicate ACKs are received in a row, it is a strong indication that a segment has been lost. "

![image-20210227195617321](3-transport%20layer.assets/image-20210227195617321.png)

### Selective Acknowledgement

Ref: [SACK](https://packetlife.net/blog/2010/jun/17/tcp-selective-acknowledgments-sack/)

SACK dedicates to avoid the unnecessary retransmission.

**TCP uses cumulative Ack.** By implementing GBN, the lost of packet 2 will lead to retransmission of packet 3, 4, ... The receiver has no way to tell the sender that it has received packet 3, 4 or the following ones correctly. So TCP appends a **SACK option** in ACK packets to deliver these informations about the following ones. (See ref. for an example.)

### Comparison with GBN and SR

TCP is a hybrid protocol of GBN and SR.

Ref: [An article about comparison](http://www.bytes.xin/2020/02/15/GBN%E5%8D%8F%E8%AE%AE%E3%80%81SR%E5%8D%8F%E8%AE%AE%E4%BB%A5%E5%8F%8ATCP%E5%8D%8F%E8%AE%AE%E5%8C%BA%E5%88%AB/)

### Flow Control

Animation: [Flow Control](https://media.pearsoncmg.com/ph/esm/ecs_kurose_compnetwork_8/cw/content/interactiveanimations/flow-control/index.html)

For the receiver, $rwnd=RcvBuffer−[LastByteRcvd−LastByteRead]$

![Screen Shot 2021-02-27 at 2.33.16 PM](3-transport%20layer.assets/Screen%20Shot%202021-02-27%20at%202.33.16%20PM.png)

The receiver tells the sender how much spare room it has in the connection buffer by placing its current value of $rwnd$ in the receive window field ("Window Size" in the TCP segment figure before) of every segment it sends to the sender.

For the sender, $LastByteSent – LastByteAcked$, is the amount of unacknowledged data that A has sent into the connection.

Thus, it makes sure throughout the connection’s life that $LastByteSent−LastByteAcked≤rwnd$

**Issue Addressing**: The TCP specification requires Host A to continue to send segments with one data byte when B’s receive window is zero.

### TCP Connection Management

![Screen Shot 2021-02-27 at 3.18.20 PM](3-transport%20layer.assets/Screen%20Shot%202021-02-27%20at%203.18.20%20PM.png)

1) SYN: SYN = 1; 2) SYNACK: SYN = 1; 3) SYN = 0

(SYN: synchronize)

![Screen Shot 2021-02-27 at 3.18.37 PM](3-transport%20layer.assets/Screen%20Shot%202021-02-27%20at%203.18.37%20PM.png)

When receiving a segment at wrong ports, the host will send **RST (reset)**.

### Congestion Control

Animation: [Congestion Control](https://media.pearsoncmg.com/ph/esm/ecs_kurose_compnetwork_8/cw/content/interactiveanimations/tcp-congestion/index.html)

End-to-end congestion control

Network-assisted congestion control

Congestion window: $cwnd$

$LastByteSent - LastByteAcked \le min\{cwnd, rwnd\}$

1) limit: adjusting the value of $cwnd$

2) perceive: "loss event": timeout or 4 ACKs (1 original + 3 duplicate)

3) change: increase/decrease $cwnd$ according to the rate at which ACKs arrive (**self-clocking**)

How to determine the rate? Guiding Principles:

1) Lost segments, lower rate

2) ACK segments, higher rate

3) Keep probing the bandwidth

#### Slow Start

ssthresh: slow start threshold

#### Congestion Avoidance



#### Fast Recovery



**FSM Summary**:

The additive-increase/multiplicative-decrease (*AIMD*) feedback control algorithm

![Screen Shot 2021-02-27 at 8.51.09 PM](3-transport%20layer.assets/Screen%20Shot%202021-02-27%20at%208.51.09%20PM.png)

Throughput: associated with loss rate

High-bandwidth: average throughput ~ loss rate (L), RTT, maximum segment size (MSS): $aver(throughput) = {1.22MSS \over RTT \sqrt L}$

#### Fairness

Ideal model with same MSS and RTT for a single associated TCP connection:

![Screen Shot 2021-02-27 at 10.37.08 PM](3-transport%20layer.assets/Screen%20Shot%202021-02-27%20at%2010.37.08%20PM.png)

In real world, those sessions with **a smaller RTT** will enjoy higher throughput. (open their congestion windows faster ???)

It is possible for UDP sources to crowd out TCP traffic.

There is nothing to stop a TCP-based application from using **multiple parallel connections**.

#### ECN

Explicit Congestion Notification

Need recent extensions to work.

2 bits "Type of Service" field of the IP datagram header are used for ECN. 

One for the router, another for the sending host (to inform routers the ECN capability).

When the receiving host receives an ECN indication, it will setting the **ECE (Explicit Congestion Notification Echo)** bit in TCP ACK segment.

Then the TCP sender, reacts to an ACK with an ECE congestion indication by **halving the congestion window**.

