# Basis  基础知识

## 计算机系统结构的概念

计算机系统结构的分类方法

**Flynn's taxonomy**: concerning the number of concurrent instr. streams and data streams

![Screen Shot 2022-02-24 at 11.59.18 AM](1_basis.assets/Screen%20Shot%202022-02-24%20at%2011.59.18%20AM.png)

- SISD: 标量流水线处理机；传统串行计算机
- SIMD: 向量流水线处理机
- MISD
- MIMD: 多核架构

## 性能指标

### CPU 性能公式

CPU 时间 = IC × CPI × 时钟周期时间（频率倒数）

注意： CPI 低不一定意味着性能更高；如果时钟周期数大，总时间可能更长。

### Amdahl's Law

make the common case fast

![Screen Shot 2022-02-24 at 12.09.35 PM](1_basis.assets/Screen%20Shot%202022-02-24%20at%2012.09.35%20PM.png)





Turning Point in Computer Arch:

- instr. parallelization ->  threads and data parallelization





