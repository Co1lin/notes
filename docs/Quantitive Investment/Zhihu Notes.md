# Zhihu Notes

## Basis

量化投资=数据+模型，包括：

- 量化择时和选股
- 套利
- 资产组合与风险管理
- 算法交易

被动投资：跟随市场平均水平

主动投资：希望投资组合的收益能够超过市场表现

efficient-market hypothesis 有效市场假说：市场的定价一直处于一个有效的状态下，市场定价已经综合反映了各方面的信息，想再根据这些信息和历史规律来试图找到一个更为有效的定价是徒劳的

市场有效的程度：

- weak form efficiency ：无法从**历史**？的价量中找到帮助盈利的规律
- semi-strong form efficiency ：从**公开渠道**获得的所有信息都无助于找到有用规律
- strong form efficiency ：从任何渠道获得的信息都无法帮助盈利

fundamental analysis 基本面分析：依靠：

- 公司财务数据：总资产、总负债、每股收益
- 宏观经济数据： GDP 增速、国家外汇、货币供应量、黄金储备
- 新闻、公告、舆情

technical analysis 技术面分析，依靠：

- 历史交易价格、成交量

## Models

### MPT

modern <u>portfolio</u> theory 现代<u>资产组合</u>理论

风险的衡量：（收益率的）方差

portfolio ：

- n 个可投资标的？ 每个的权重表示为 n 维向量，和为 1

- 收益：加权和

- 风险：两两相乘的权重乘上对应协方差的总和
    $$
    \begin{aligned}
    r_p &= \sum_{i=1}^n w_i r_i \\
    \text{var}(r_p) &= \sum_{i=1}^n \sum_{j=1}^n w_i w_j \cdot \text{cov}(r_i, r_j)
    \end{aligned}
    $$

> **Covariance**: the expected value of the ***product*** of their deviations from their individual expected values
>
> $$
> \begin{aligned}
> \text{cov}(X,Y) &= \mathbb{E}( (X - \mathbb{E}(X)) (Y - \mathbb{E}(Y)) ) \\
> &= \mathbb{E}(XY) - \mathbb{E}(X) \mathbb{E}(Y)
> \end{aligned}
> $$
>
> ***协方差***为正，两个随机变量是同向变化；为负，反向变化。
>
> 再除以二者标准差之积，消除变化幅度影响，则为***相关系数***；描述了二者变化的相似程度。


目标：达到期望收益的情况下，最小化风险：
$$
\begin{aligned}
\bold{w} = \arg\min_{} \text{var}(r_p) \text{ , s.t. } \mathbb{E}(r_p) \ge \mu
\end{aligned}
$$

Return 收益 - Risk 风险曲线（efficient frontier, Markowitz bullet）

![v2-cc1ba4f1c764aad69f0905b3e49b04c0_1440w](Zhihu%20Notes.assets/v2-cc1ba4f1c764aad69f0905b3e49b04c0_1440w.jpg)

- **NOTE**: Risk 是标准差 $\sigma$ ，不是方差！

- 红点：无风险利率 $r_f$

- 绿点： **Market Portfolio 市场组合**：不包括无风险资产

    $\text{Sharpe} = {\mathbb{E}(r_p) - r_f \over \sigma(r_p)}$ 最高；

    Sharpe Ratio ：比无风险投资的多的收益 over 承受的风险，

    衡量「收益-风险比」， i.e. 投资效率；

- Efficient Frontier （蓝）上的点：给定 Return 下 Risk 最小的组合；在诸多有风险资产上配置能达到的最佳情况； Market Portfolio 是效率最高的

- Capital Market Line （橙）资本市场线：通过加入无风险资产之后能配置出来的

### CAPM

Capital Asset Pricing Model ，资本资产定价模型
$$
\begin{aligned}
\mathbb{E}(r_s) &= r_f + \beta_s (\mathbb{E}(r_M) - r_f) \\
\text{where } \beta_s &= {\text{cov}(r_s, r_M) \over \text{var}(r_M)}
\end{aligned}
$$

- $r_s$ ：风险资产的收益率
- $r_f$ ：无风险资产的收益率
- $r_M$ ：Market Portfolio 市场组合（点处）的收益率
- $\beta_s$ ：风险资产相对于市场的“敏感度”，描述其与市场组合的相对变化
    - $\beta_s$ 作用于市场组合相对于无风险资产的超额收益
    - 大于 1 时，风险更大，在牛市 $r_s$ 将比市场组合获得更大收益
    - 小于 1 时，风险较小，在熊市 $r_s$ 将比市场组合有着更小的损失
    - 小于 1 时，在熊市赚钱（空头？）





