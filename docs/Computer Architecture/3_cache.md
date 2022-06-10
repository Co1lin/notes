# Cache

缺失率计算：注意 Cache 以 cache line 为单位操作。

写策略：

Write Through v.s. Write Back

发生写不命中时，是否调入相应的块，有两种选择：

- write allocate 写分配：将所写单元从主存调入 cache ，然后写入。
- no-write allocate / write around 绕写法：直接写入下一级存储器，不将对应块调入 cache 。
    - 对于连续区域的写操作，采用绕写法时缺失率很高（100%），可以通过先读取一遍将内容加载至 cache 中。

一般：写回搭配写分配；写直达搭配绕写。