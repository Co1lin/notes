# SVM

![Screen Shot 2021-05-27 at 11.08.29 AM](SVM.assets/Screen%20Shot%202021-05-27%20at%2011.08.29%20AM.png)

## Linear Separable

线性可分训练集：

- 类标记只有 +1、-1 两种取值，即 $y_i \in \{+1, -1\}$
- 通过间隔最大化得到分类超平面 $\boldsymbol{w^T}\boldsymbol{x} + b = 0$
- 决策函数 $f(\boldsymbol{x}) = \text{sign} (\boldsymbol{w^T}\boldsymbol{x} + b)$

### 函数间隔

超平面关于样本点的函数间隔为

$$
\hat{\gamma_i} = y_i (\boldsymbol{w^T}\boldsymbol{x_i} + b)
$$

超平面关于训练集 T 的函数间隔为：

$$
\hat{\gamma} = \min_i \hat{\gamma_i}
$$

### 几何间隔

类比函数间隔，除掉 w 的模长：

$$
\begin{aligned}
\hat{\gamma_i} = y_i ({\boldsymbol{w^T} \over ||w||} \cdot \boldsymbol{x_i} + {b \over ||w||}) \\
\gamma = \min_i \gamma_i
\end{aligned}
$$

### 优化：间隔最大化

#### 从最大化到最小化

目的：选取 $(w, b)$ ，使得超平面到两侧样本点的最小距离最大。

点到超平面的距离：

$$
{|\boldsymbol{w^T x} + b| \over |\boldsymbol{w}|}
$$

优化式：

$$
\begin{aligned}
\max_{\boldsymbol{w}, b} \min_{x_i} {1 \over ||\boldsymbol{w}||} y_i (\boldsymbol{w^T x_i} + b) \\
= \max_{\boldsymbol{w}, b} {1 \over ||\boldsymbol{w}||} \min_{x_i} y_i (\boldsymbol{w^T x_i} + b)
\end{aligned}
$$

由于后面的函数间隔的式子其实是可以随意缩放的，所以不妨把它固定为1（约束为1），最后得到的 $(w,b)$ 是一样的；于是优化式等价于：

$$
\begin{aligned}
& \max_{\boldsymbol{w}, b} {1 \over ||\boldsymbol{w}||} \\
\text{s.t.} ~ & \min_{x_i} y_i (\boldsymbol{w^T x_i} + b) = 1 \\
\text{i.e.} ~ & y_i (\boldsymbol{w^T x_i} + b) \ge 1 ~ (\forall i)
\end{aligned}
$$

继续等价变形：

$$
\begin{aligned}
\min_{\boldsymbol{w}, b} {1 \over 2} ||\boldsymbol{w}||^2,~
\text{s.t.} ~ y_i (\boldsymbol{w^T x_i} + b) \ge ~ 1 (\forall i, i = 1, 2, \dots N)
\end{aligned}
$$

**支持向量**：使约束条件等号成立的点构成支持向量。表现在图上就是两侧距离最优分界面最近的那些点。

#### Lagrangian Function

##### Lagrange Multiplier Method

拉格朗日乘数法与上述优化问题的区别是，前者的约束条件时等式，而现在却是不等式。

##### KKT

Karush-Kuhn-Tucker（KKT）条件是非线性规划（nonlinear programming）最佳解的**必要条件**。

KKT条件将 Lagrange乘数法（Lagrange multipliers）所处理涉及等式的约束优化问题**推广至不等式**。

与等式的最大区别：<u>不等式乘上的 Lagrange Multiplier 多了一个非负的限制条件</u>。

> **完整的 KKT 条件**：
> 
> 如果想解优化问题：
>
> $$
> \begin{aligned}
> &\min f(\boldsymbol{x}) \\
> \text{s.t.} ~ 
> g_j(\boldsymbol{x}) &= 0 ~ (j = 1, \dots, m) \\
> h_k(\boldsymbol{x}) &\le 0 ~ (k = 1, \dots, p)
> \end{aligned}
> $$
> 
> 则定义 Lagrangian Function ：$L(\boldsymbol{x}, \lambda, \mu) = f(\boldsymbol{x}) + \sum_j \lambda_j g_j(\boldsymbol{x}) + \sum_k \mu_k h_k(\boldsymbol{x})$
> 
> 根据最佳解满足的 KKT 必要条件：
> 
> $$
> \begin{aligned}
> \nabla L &= 0 \\
> g_j(\boldsymbol{x}) &= 0 ~ (j = 1, \dots, m) \\
> h_k(\boldsymbol{x}) &\le 0 \\
> \mu_k &\ge 0 \\
> \mu_k h_k(\boldsymbol{x}) &= 0 ~ (k = 1, \dots, p)
> \end{aligned}
> $$

> 可以解出相关的参数。

于是这里有：

$$
\begin{aligned}
L(\boldsymbol{w}, b, \boldsymbol{\alpha}) = 
{1 \over 2} ||\boldsymbol{w}||^2 +
\sum_{i=1}^{N} \alpha_i [1 - y_i (\boldsymbol{w^T x_i} + b)] \\
\alpha_i \ge 0, ~ 
1 - y_i (\boldsymbol{w^T x_i} + b) \le 0, \\
\alpha_i [1 - y_i (\boldsymbol{w^T x_i} + b)] = 0
\end{aligned}
$$

（最后一个等式为「**互补松弛性**」（complementary slackness），

意思是要么是 $\alpha_i = 0$ ，要么是样本 $(\boldsymbol{x_i}, y_i)$ 使后面的中括号等于0。）

**建立 Lagrange 函数与原优化问题的关系**：

因为后面的约束项是 ≤ 0 的，而 α 又非负，因此后面的求和项最大也不过是0。考虑：

$$
\max_{\boldsymbol{\alpha}} L(\boldsymbol{w}, b, \boldsymbol{\alpha})
$$

如果 ≤ 0 的约束满足，那么该最大值为 $\min_{\boldsymbol{w}, b} {1 \over 2} ||\boldsymbol{w}||^2$ ；如果不满足，则为 $+\infty$ 。因此，对该最大值再取最小值的结果是统一的：

$$
\min_{\boldsymbol{w}, b} \max_{\boldsymbol{\alpha}} L(\boldsymbol{w}, b, \boldsymbol{\alpha})
= \min_{\boldsymbol{w}, b} {1 \over 2} ||\boldsymbol{w}||^2
$$

至此，我们找到了原优化问题的等价式！

#### Duality

但其实上面的先取 max 再取 min 的东西还不好算，因此继续转化为「**对偶问题**」。

根据：弱对偶关系：「极大的极小 ≥ 极小的极大」（“凤尾 ≥ 鸡头”），有：

$$
\begin{aligned}
\min_{\boldsymbol{w}, b} L(\boldsymbol{w}, b, \boldsymbol{\alpha}) \le
L(\boldsymbol{w}, b, \boldsymbol{\alpha}) \le
\max_{\boldsymbol{\alpha}} L(\boldsymbol{w}, b, \boldsymbol{\alpha}) \\
\Rightarrow
\max_{\boldsymbol{\alpha}} \min_{\boldsymbol{w}, b} L(\boldsymbol{w}, b, \boldsymbol{\alpha}) 
\le
\min_{\boldsymbol{w}, b} \max_{\boldsymbol{\alpha}} L(\boldsymbol{w}, b, \boldsymbol{\alpha})
\end{aligned}
$$

而满足 KKT 条件时不等号变为等号，即强对偶成立，因此原问题最终转化为：

$$
\max_{\boldsymbol{\alpha}} \min_{\boldsymbol{w}, b} L(\boldsymbol{w}, b, \boldsymbol{\alpha}) 
$$

#### Solution

1) $L$ 对 $\boldsymbol{w}, b$ 分别求偏导，令结果为0，产生两个等式。

2) 将两个等式代入 $L$ ，等价变形，可将优化问题转化为：
   
   $$
   \begin{aligned}
   \min_{\alpha} ~~{1\over 2} \sum_{i=1}^N \sum_{j=1}^N \alpha_i \alpha_j y_i y_j \boldsymbol{x_i}^T \boldsymbol{x_j} - \sum_{i=1}^N \alpha_i \\
   \text{s.t.}~ \alpha_i \ge 0, ~ \sum_{i=1}^N \alpha_i y_i = 0
   \end{aligned}
   $$

3) 利用第二行的约束条件，可以求解 α ：

   先利用样本标签与 α 乘积求和的式子消去一个 α 。

   再代入第一行，得到关于 α 的：$s(\alpha_i, \dots, \alpha_{n-1})$ 表达式。

   通过令偏导等于0，可以求出极值点 $(\alpha_i, \dots, \alpha_{n-1})$ 。

   1）若极值点满足每个 α  非负，则找到了一组 α 。

   2）若不满足每个 α  非负，说明最小值<u>在边界上</u>！那么就需要检查每个 α = 0 的边界情况，从中选取使得 $s$ 最小的一组 α 。

4) 确定了 α ，接下来求解超平面。结合 KKT 条件，可以得到：
   
   $$
   \begin{aligned}
   \boldsymbol{w^*} &= \sum_{i=1}^N \alpha_i^* y_i \boldsymbol{x_i} \\
   b^* &= y_k - \sum_{i=1}^N \alpha_i^* y_i \boldsymbol{x_i}^T \boldsymbol{x_k}
   \end{aligned}
   $$
   
   其中，$(\boldsymbol{x_k}, y_k)$ 为某 $\alpha_k \neq 0$ 对应的样本。

   i.e. 选定某 $\alpha_k \neq 0$ ，将其对应的样本代入“固定”，然后再根据求和符号遍历所有样本，计算出 $b^*$ 。

5) 写出超平面，以及决策函数。
