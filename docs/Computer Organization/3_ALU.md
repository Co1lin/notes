# ALU

## 乘法

### 原码乘法

结果 = 被乘数 × 乘数

基本算法：

- 若乘数的当前位为 1 ，将被乘数和部分积求和
- 若乘数的当前位为 0 ， skip
- 部分积左移
- 循环 32 次

### 补码乘法

布斯算法：

原理推导：

![Screen Shot 2021-12-24 at 12.45.50 AM](3_ALU.assets/Screen%20Shot%202021-12-24%20at%2012.45.50%20AM.png)

操作过程：

![Screen Shot 2021-12-24 at 12.46.41 AM](3_ALU.assets/Screen%20Shot%202021-12-24%20at%2012.46.41%20AM.png)



### 除法

#### 原码一位除

![Screen Shot 2021-12-24 at 10.51.42 AM](3_ALU.assets/Screen%20Shot%202021-12-24%20at%2010.51.42%20AM.png)

#### 加减交替除法

原理：

![Screen Shot 2021-12-24 at 10.54.22 AM](3_ALU.assets/Screen%20Shot%202021-12-24%20at%2010.54.22%20AM.png)

具体操作（by Acha）：

![image-20211222202357313](3_ALU.assets/image-20211222202357313.png)

若求中间某一步的余数，则看其正负；若为负，则需加上除数 Y （带符号的补码形式）。

