# Batch System

用户库入口 `_start`

- clear bss
- 调用 main ，将返回值告知批处理系统 `exit(main());` exit 是通过 ecall （syscall）实现的

应用程序作为 Kernel 的数据段链接到 Kernel 中

Kernel 中， AppManager 维护了：用户 app 数量；当前 app 编号；每个 app 的起始地址

- `load_app` : 
    - 将给定 id 的 app 加载到固定的 `0x80400000` ；这是与用户程序的约定。
    - 流程：清空 i-cache ，清空加载目标地址（填写 0 ），复制 user app 到目标地址。

OS Kernel 的工作：

- 启动 user app 时， init context of user app ，切换到 U-Mode 开始执行
- 响应 trap/exception
- user app 结束时，通过 `sys_exit` 加载运行下一个

User Stack and Kernel Stack

- Why? 如果 U-Mode 控制流和 S-Mode 控制流使用一个 Stack ，那么 U-Mode 下 user app 就能读取到一些内核函数的地址，不安全
- How? OS 中用汇编实现 stack 的切换，并在 Kernel stack 中保存 user app 控制流的「寄存器状态」

TrapContext: what needs to be save

- Universal Register: Trap 处理的代码中可能修改它们，需要保存
- Control and Status Register:
    - `sstatus.SPP` ： CPU 之前的特权级：考虑嵌套，需要保存
    - `sepc` ：Trap 处理完成后要执行的指令的地址：考虑嵌套，需要保存
    - `scause/stval` Trap 的原因 + 附加信息：总是在 trap 一开始的「分发阶段」「立即」使用，后续不使用，无需保存



