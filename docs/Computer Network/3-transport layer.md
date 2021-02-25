# 3 Transport Layer

![image-20210225163238007](transport%20layer.assets/image-20210225163238007.png)

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

![Screen Shot 2021-02-25 at 10.49.15 PM](transport%20layer.assets/Screen%20Shot%202021-02-25%20at%2010.49.15%20PM.png)

UDP socket can be identified by a **two-tuple**.

There's no source IP, so the receiver will treat UDP segments which has the same source port and dest. port in the same way, although their source IP may be different.

(Source IP is transported by the Network Layer.)

### Checksum

Ref: [UDP Checksum](https://www.securitynik.com/2015/08/calculating-udp-checksum-with-taste-of.html)

![](transport%20layer.assets/UDP%20Checksum%20table.jpg)

**Pseudo Header** is not transported. It only serves for computing the checksum.

If the result overflows, we need to "**wrap it around**".

Also note the "reserved" or "padding" zeros.

If the data size is not an integral multiple of 16 bits, we need to pad zeros behind.

#### Checking by Receiver

Add all data (certainly including the checksum computed by the sender) as 16 bits numbers together. If the result contains "0", the process must have some error. Even the result consists of "1", we can not be sure that the process is totally right.

## Reliable Data Transfer

