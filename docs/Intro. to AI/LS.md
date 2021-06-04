# Local Search

## 邻域

$D$ 是问题的定义域。映射 $N$ s.t.

$$
\begin{align}
N: ~ S \in D \rightarrow N(S) \in 2^D
\end{align}
$$
即把定义域中的某个元素 S 映射到定义域的幂集（子集的集合）中的某个元素（i.e. 映射到定义域的某个子集，包含一些定义域中的元素）。

aka. 从一种状态，“变异”出其它几种邻近的状态。

## 算法

- 随机选择一个初始的可能解 $x_0 \in D, x_b = x_0$ ，计算邻域 $P=N(x_b)$ ；
- 若不满足结束条件：
    - 选择子集 $P' \subset P$ ， $x_n$ 为 $P'$ 中的最优解；
    - 若找到了更优的，i.e. $f(x_n) < f(x_b)$ ，则  $x_b = x_n, P = N(x_b)$ ；判断是否满足结束条件；
    - 否则 $P = P - P'$ ，从剩下的中选取子集 $P' \subset P$ ，迭代。

## 问题

局部最优问题：可以设一定的概率不选指标函数最好的点。

步长问题：变步长。

起始点问题：随机生成一些初始点，从每个初始点出发搜索找到各自的最优解，最后全局取最优。
