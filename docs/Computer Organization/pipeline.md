# Pipeline Design

理想情况下，流水线处理器上的指令执行时间 = 非流水线处理器上的执行时间 / 流水线级数

流水线提高**吞吐率**，而非单个指令的执行时间（CPI）。

## Hazard

### Structural Hazards

同时访问一个存储器

### Data Hazards

后一个指令的 src register 为前一个指令的 destination register 

解决方案： forwarding / bypassing

![Screen Shot 2021-10-29 at 4.16.37 PM](pipeline.assets/Screen%20Shot%202021-10-29%20at%204.16.37%20PM.png)

For load-use data hazard: insert a bubble

![Screen Shot 2021-10-29 at 4.17.15 PM](pipeline.assets/Screen%20Shot%202021-10-29%20at%204.17.15%20PM.png)

### Control Hazards

branch 指令

解决方案：在第二阶段算出目标 PC ，并插气泡；分支预测； delayed decision











