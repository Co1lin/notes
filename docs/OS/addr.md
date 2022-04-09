# Address

逻辑地址 --(段式内存管理转换)--> 线性地址（虚拟地址） --(页式内存管理转换)--> 物理地址



## Memory Allocation

![Screen Shot 2022-04-09 at 16.35.06](addr.assets/Screen%20Shot%202022-04-09%20at%2016.35.06.png)

静态分配： global, static, code

`.bss` 需要初始化为 0

`.data` 不需要初始化？

动态分配

- stack 编译器管理
- heap 人工管理

连续内存分配

- 动态分区分配： OS 分配给进程一段分区（VA 连续）

    - First Fit

    - Best Fit ：释放时，检查是否可与临近的空闲分区合并

    - Worst Fit ：释放时，检查是否可与临近的空闲分区合并

- Buddy System: 针对以 $2^U$ 大小为单位申请/归还**连续内存**块做优化

非连续内存分配

- segmentation
    - CPU 拿到地址（段号+段内偏移），查段表（两项：段基址+段长度），通过基址 + offset 得到物理地址
- Paging
    - Frame 物理地址空间划分出的基本分配单位
    - Page 虚拟地址……
    - 单级页表相当于一个很大的数组；解决方法：
        - 多级页表
        - 反置页表： Hash(pid, vpn)
        - 段页式



## Virtual Address

Why?

- 内存不够用 -> 用外存来补

How?

- Overlay 函数覆盖：以函数/模块为单位**手动**换入换出

    - programmer 自己控制程序中的各个模块

    - 程序段：程序中功能独立的段

    - 覆盖段：无需同时装入内存的一些程序段组成一组

    - ![Screen Shot 2022-04-09 at 10.01.20](addr.assets/Screen%20Shot%202022-04-09%20at%2010.01.20.png)

        （最优划分方式？）

    - Drawbacks

        - 编程困难
        - 增加执行时间（从外存装入覆盖模块？）

- Swapping 程序交换：以程序为单位**自动**换入换出

    - swap in/out 将一个程序的整个地址空间读入到内存/保存至外存
    - 只有 mem 不够（或可能不够）时 swap out
    - swap 区大小：存放所有用户进程到所有 mem image 的 copy
    - swap out 后 swap in 不一定在原处！某种机制保证寻址正确性

- Virtual Storage 虚拟存储：以页为单位**自动**换入换出

    - 结构：内存 + 外存
    - 不常用：暂存到外存； CPU 要访问：装入内存
    - 基本特征：
        - 不连续
        - 大用户空间：给用户的虚拟内存 > 实际物理内存
        - 部分交换
    - 实现
        - Hardware (MMU/TLB/PageTable): 地址转换、硬件异常
        - OS: mem 中建立 page table ；管理换入/换出



Processing Page Fault

![Screen Shot 2022-04-09 at 10.21.33](addr.assets/Screen%20Shot%202022-04-09%20at%2010.21.33.png)

注： PageTable Entry 中的 Present Bit 指示该地址是在内存中还是在磁盘中。

PTE 中可以存储扇区地址引导 OS 寻找 disk 上的页。

性能衡量： effective memory access time, EAT

## Page Replacement Algorithm

Why?

- Page Fault 而 mem 已满，需要置换 page

Goal

- 减少 Miss rate ， swap in/out 次数

When?

- max/min threshold of free mem.

Who?

- NOT locked pages: frame locking: OS 的关键部分，常驻内存； PTE 中 lock bit

How?

- 局部置换：仅限于当前进程占用的物理页面
- 全局置换：所有可换出的物理页面

### 局部置换

Belady 现象

- 有： FIFO 、 Clock （包括改进的）、 不恢复计数的 LFU
- 无： LRU 、 恢复计数的 LFU

FIFO 、 Clock （包括改进的）出现 Belady 现象的例子： 123412512345

#### Optimal

思路：置换在未来最长时间不访问的页面

实现：
- Page Fault 时，计算当前 Mem. 中每个页面的下一次访问时间
- 置换下次访问时间距离现在最远的页

性质：最理想情况；无法实现（除非先模拟一遍）

#### FIFO

思路：选择在 Mem. 中驻留时间最长的页置换

实现：维护 Linked List ， front 是驻留时间最长的， end 是最短的； front pop ， end push ；只有置换时才修改 List

性质：性能较差；调出的页面可能经常访问（驻留时间长，但可能访问频率很高）

**Belady 现象**：分配的物理页面数增加，缺页并不一定减少

#### LRU

Least Recently Used

思路：选最长时间没被访问的页面置换

实现：

- 记录内存中每个页面的上一次访问时间；将上次访问到当前时间最长的置换

- List： front 为最近刚使用的； end 为最久未使用的；

    访存时将该页从链表中某一个位置移到 front ；置换 end

特征：开销大；近似 Opt ？

#### Clock

![Screen Shot 2022-04-09 at 11.58.40](addr.assets/Screen%20Shot%202022-04-09%20at%2011.58.40.png)

特征：

- 访问时不修改 list 中页面的顺序，仅做标记（FIFO）
- 缺页时修改 list （LRU）
- 未访问的页面，和 LRU 一样好
- 对于访问过的，不像 LRU 一样记录了访问顺序

#### LFU

思路：置换访问次数最少的页面

实现：每个页面设置一个访问计数

特征：

- 开销大
- 一开始频繁使用，之后不使用的页很难被替换 -> 解决策略：计数定期右移

### 全局置换

Why?

- 局部置换算法没有考虑不同进程的访存差异

How?

- 为进程分配可变数目的物理页面
- 在不同阶段有所变化

工作集：当前时间往前大小为 $\tau$ 的窗口内，逻辑页面的集合

工作集置换算法

缺页率置换算法



Thrashing 抖动

- 进程物理页面太少，不能包含工作集，造成大量缺页与置换
- 进程数目增加，分给每个进程的物理页面数不断减少
- Trade-off 并发效率和缺页率







