# 4 Network Layer

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





