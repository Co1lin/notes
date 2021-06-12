# Naive Bayes Classifier

## Bayes' Thm.

$$
\begin{aligned}
P(Y = c_k | X = x) 
&= {P(Y = c_k, X = x) \over P(X = x)} \\
&= {P(X = x | Y = c_k)P(Y = c_k) \over P(X = x)}
\end{aligned}
$$

事件 X 是由原因 $Y = c_k$ 引起的概率。

## Bayes Classifier

给定X（一个n维向量，每一维代表一个特征），判断它是类别集合Y中的哪一类。

那么就要<u>选取一个类别 $c_k \in Y$ ，使得 $P(Y = c_k | X = x)$ 最大</u>。

（下面用Y代表类别变量；X代表输入样本变量；y是预测的类别）

$$
\begin{aligned}
y &= \arg\max_{c_k} P(Y = c_k | X = x) \\
&= \arg\max_{c_k} {P(X = x | Y = c_k)P(Y = c_k) \over P(X = x)} \\
&= \arg\max_{c_k} {P(X = x | Y = c_k)P(Y = c_k)}
\end{aligned}
$$

## Naive Bayes Classifier

想求 $P(X = x | Y = c_k)$ ，但是参数太多，没法算，因为这相当于在给定类别的条件下，寻找恰好符合一组特征的样本。输入样本为n维向量，对应n个特征，每个特征又对应不同的取值，这就产生了一个n维的联合分布，空间是很大的。

因此做出「独立性假设」简化之：假设特征之间的取值相互独立！于是：

$$
\begin{aligned}
y &= \arg\max_{c_k} {P(X = x | Y = c_k)P(Y = c_k)} \\
&= \arg\max_{c_k} P(Y = c_k) ~ \prod_{j=1}^n P(X^{(j)} = x^{(j)} | Y = c_k)
\end{aligned}
$$

$P(X^{(j)} = x^{(j)} | Y = c_k)$ 表示类别为 $c_k$ 的所有的样本中，特征的第 $j$ 维是 $x^{(j)}$ ，i.e. 输入样本第 $j$ 个特征的值，的概率。

### Laplace Smoothing

为了避免 $P(X^{(j)} = x^{(j)} | Y = c_k)$ 由于训练集覆盖不全面而为0，进而导致整个概率为0，加入 Laplace Smoothing 进行修正：

$$
\begin{aligned}
P(X^{(j)} = x^{(j)} | Y = c_k) 
&= {\text{Count}(X^{(j)} = x^{(j)} , Y = c_k) + \lambda \over \text{Count}(Y = c_k) + S_j \lambda}
\end{aligned}
$$

$S_j$ 为第 $j$ 维对应特征的所有可能的取值的数目。

$\lambda$ 通常取1。

本质上，就是对于第 $j$ 维对应特征所有可能的取值情况，都加了个1，i.e. 保证第 $j$ 维每种可能的取值都至少有1个样本。

## Practice

给定：

- 训练集；每个样本为 $(x^{(1)}, x^{(2)}, \dots, x^{(n)}, c)$ ，即具有 n 个特征的样本对应一个实际类别 c 。
- 待预测样本 $x$ ；需要求出预测类别 y 。

利用公式计算：

$$
\begin{aligned}
y 
&= \arg\max_{c_k} P(Y = c_k) ~ \prod_{j=1}^n P(X^{(j)} = x^{(j)} | Y = c_k) \\
&= \arg\max_{c_k} {\text{Count}(Y = c_k) \over |\text{Training Set}|} ~ \prod_{j=1}^n
{\text{Count}(X^{(j)} = x^{(j)} , Y = c_k) + \lambda \over \text{Count}(Y = c_k) + S_j \lambda}
\end{aligned}
$$

- 需要代入每种 $c_k$ ，最后 y 取让后面的概率最大的 $c_k$
- 遍历 n 维特征算连乘



