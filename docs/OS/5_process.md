# Process

如何让用户控制程序的执行？交互？



三个系统调用；两个特殊的用户态程序

相关数据结构：拆分任务管理与 CPU 当前状态的管理；存储进程的一些信息； PID 索引；资源回收

进程管理：创建；切换；调度；生成；资源回收； I/O



## Concepts

fork

- 内核初始化完毕后，以硬编码方式创建唯一一个进程 Initial Process （用户初始进程）；其它所有进程通过 fork 来创建。
- A fork 出 B ，二者建立父子关系：
    - 相同的：代码段等数据（但在两个独立的 Address Space 中）、 Universal Reg （PC, SP）
    - 不同的：返回值 a0 （A 为 B 的 PID ， B 为 0 ）



waitpid

- Why? 一个进程调用 exit 退出后，它所占用的资源不能立即全部回收，比如 kernel stack 还在被用来做 syscall 。
- How?
    - 进程退出时， kernel 立即回收一部分资源，标记 Zombie Process ；
    - 之后，父进程通过 waitpid `pub fn sys_waitpid(pid: isize, exit_code: *mut i32) -> isize;` 收集返回状态，回收全部资源。
    - 若一个进程先于其子进程结束，
        - 它的所有子进程成为进程树的 root
        - 这些子进程成为 ***Initial Process*** 的子进程；后者为前者的父进程



exec `pub fn sys_exec(path: &str) -> isize;`

- 清空当前进程的 address space ，加载特定的可执行文件，返回用户态后开始执行
- exec 与 fork 结合，实现创建指定可执行文件的进程（ Unix 的做法）



Kernel 初始化 ➡️ 加载并执行 initproc ➡️ initproc 中 fork 并 exec 来运行 user shell



