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

信息存储时的 X 字节对齐： trade-off between speed and space

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

**理论最短平均编码长度**：根据各种指令出现的频度，计算**信息熵**
$$
信息冗余量 = { 某种编码方式的平均码长 - 理论最短平均码长 \over 某种编码方式的平均码长 }
$$


Huffman Encoding: 平均码长最短

基于 Huffman Encoding 的扩展操作码：减少码长变化的程度，设置离散的、固定的几种码长

等长扩展码：

- 原则：短码不能为长码的前缀



