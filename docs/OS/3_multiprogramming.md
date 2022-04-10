# Multiprogramming

批处理：内存中只放一个程序，处理器一次只能运行一个程序，一个执行完毕后将另一个**调入**内存（每个应用程序地址空间的起始地址一样）；不能交错执行程序。

多道：内存中可以放置多个程序， OS 可以交错地执行多个程序；每个程序地址空间的起始地址不同

- 协作式：应用程序主动放弃 CPU
- 抢占式 Time Sharing/Multitasking ：时钟中断

rCore 实验中：

- 通过 `build.py` 为每个应用程序修改 linker script 中的 `BASE_ADDRESS` ，来实现互不重叠的内存布局。
- `__switch` 实现任务切换：两个参数：
    - 当前任务的 Context 所在的地址
    - 下一个任务的 Context 所在的地址

![../_images/switch.png](3_multiprogramming.assets/switch.png)

- 静态的 TaskManager 中为每个任务申请了空间存储其 Context
- 静态的 Kernel stack 和 User stack **数组**存储了每个任务的两个 stack
- 在 `__switch` 中存储当前任务的 Context ，恢复下一个任务的 Context ； `sp` 的改变实现了 Kernel stack 的切换；然后跳转到 `__restore` 便可「恢复」 TrapContext 中的一些内容，设置 `sp` 指向用户栈，从而执行这个 app 。

## 协作式调度

实现 `sys_yield` 和 `sys_exit` 。

## 分时与抢占式调度

RISC-V 的中断可以分成三类：

- **软件中断** (Software Interrupt)：由软件控制发出的中断
- **时钟中断** (Timer Interrupt)：由时钟电路发出的中断
- **外部中断** (External Interrupt)：由外设发出的中断

中断特权级决定了中断是否会被屏蔽，以及 Trap 到哪个特权级处理

- 中断特权级低于 CPU 的，被屏蔽
- 中断特权级不低于 CPU 的，读取 CSR 判断是否被屏蔽

默认情况下，所有的中断都需要到 M 特权级处理。 rust-sbi 设置了中断代理 CSR ，使得可以到 rCore 的 S-Mode 处理中断。

Trap 嵌套：

- 允许 Kernel 处理系统调用时被打断，从而优先处理某些中断，提高对部分中断的响应速度
- 目前 rCore 实验中， S-Mode 中屏蔽了时钟中断，因此 Kernel 代码不会被打断

