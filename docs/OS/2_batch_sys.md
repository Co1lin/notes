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

How to Trap?

- `ecall` 进入内核态（`stvec` Trap 处理代码的入口地址；使用时需要左移），保存被打断的 user app 的 Context 至 Kernel Stack
- OS 读取 CSR ，分发处理
- 完成 syscall ，恢复 user app 的 trap context ，sret 继续执行 user app

How to save TrapContext? In `__alltraps` :

- `sscratch` 在 Kernel 完成初始化之后，跳转到 U-Mode 之前被设置为 Kernel stack ！

    这一步是「复用」 `__restore` 实现的

- 最开始，交换 `sp` 与 `sscratch` ，使得 `sp` 为 Kernel stack ， `sscratch` 为 User stack

- `sp` 下移，申请一个 TrapContext 的空间，保存通用寄存器

- 经由通用寄存器中转，保存 `sstatus` and `sepc`

- 经由通用寄存器中转，保存 `sscratch` （此时指向之前的 User stack ）

- 将 `sp` （此时指向 Kernel stack ） mv 到 `a0` ，其作为 `trap_handler` 的第一个参数使后者能访问到 Kernel stack 里的 TrapContext ，因为 `trap_handler` 此时需要通过 TrapContext 才能读取到未被其它操作修改的、正确的寄存器的值。

`trap_handler` 中的 trick ：传入 `cx` ，返回还是 `cx` ，使得其之后的 `__restore` 可以拿到这个 `cx` （即 Kernel stack）

`__restore` 的工作：

- 接受传入的下一个任务的 TrapContext ，通过「恢复」机制设置执行这个 user app 的 Context 。
- 恢复上述 Context 后，它就没必要继续存在了，因此恢复 sp 释放刚刚这个 Context 所占用的 Kernel stack 的空间。
- 全局来看，每个任务开始时， User stack 的 sp 是一样的， Kernel stack 的 sp 也是一样的。

`run_next_app` 中 how to run the next app/the first app?

- 构造即将运行的 user app 的 TrapContext ，压入 Kernel stack
- 将压入 TrapContext 之后的 Kernel stack 的 sp 传给 `__restore` ，那么 `a0` 便为 Kernel stack sp 。这使得两种进入 `__restore` 的方式均能保证 `a0` 为 Kernel stack sp 。

