# ISA

## Taxonomy of ISA

操作数的存储单元： stack, accumulator, register

早期计算机： stack or accumulator

- 字长短
- 频繁访问 stack or accumulator -> bottleneck

80 年以后： 通用寄存器型结构

- register 访问速度快
- 灵活分配
- 相对 mem 地址短

## Addressing Mode

编址方式：按字节/字编址

访问方式：信息存储时的 X 字节对齐： trade-off between speed and space

## ISA Design and Optimization

### Principles

ISA is the interface between hardware and software

- **频繁**、简单的事物交给硬件

### Branches

控制指令中占比最大的：分支 branch （70 ～ 80%）

分支的三种实现：

- CC 条件码：比较指令设置 CC
- 条件寄存器：比较指令的结果存入 register
- compare and branch 组合

### Opcode Optimization

指令 = OPC (操作码) + A (地址码)

操作码如何优化？

**理论最短平均编码长度**：根据各种指令出现的频度，计算**信息熵** $H = - \sum_i p_i \log_2 p_i$ 即可求得。
$$
信息冗余量 = { 某种编码方式的平均码长 - 理论最短平均码长 \over 某种编码方式的平均码长 }
$$

Huffman Encoding: 平均码长最短

- 根据频度构造 Huffman 树
- 根据 Huffman 树得到每个操作码的编码
- 根据频度和每个编码的长度求出平均码长，从而可求出信息冗余量

基于 Huffman Encoding 的扩展操作码：减少码长变化的程度，设置离散的、固定的几种码长

- 如：根据 Huffman 编码，码长有 2，3，4 ；可以统一至 2、4 两种。
- 给频度较高的操作码分配短的编码；频度较低的用拓展法，分配到较长的编码。

等长扩展码：

- 原则：短码不能为长码的前缀

- 分级译码：具体方案的选择取决于频度分布

    - 15/15/15 ： 4-bit 的 16 个码点中，前 15 个分配给操作码，最后一个用于拓展到下一个 4-bit 。

        更一般的情况：留出 k 个用于拓展到下一级，下一级的可用码点： $2^4 k$ 。

    - 8/64/512 ： 0XXX 表示最常用的 8 种；1XXX0XXX 表示后续的 64 种；1XXX1XXX0XXX ……


定长操作码：

- 存储器越来越大，无需过度节省空间；
- 减少硬件上译码的复杂度，采用定长操作码（尤其是 RISC ）。





