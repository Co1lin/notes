# Notes for Developing a Pipeline

Ref: [盘点我跳过的科研天坑，进坑就是半年白干](https://mp.weixin.qq.com/s/8-5X_W-aWYKMuo0l9gstNg)

## Data

1. Basic statistics: min, max, mean, std, ...

2. Approximate probability distribution and the method of normalization

3. Data volume: is it enough? -> augmentation, cross-validation, ...

4. Data quality: balanced or unbalanced? Noisy or not? 

    -> data splitting (Are all classes included in training, val. and testing set?)



## Finetuning

Grid search



## Evaluation

### Precision is not all

TODO: 

> 数据采集不均衡的情况很常见。例如，很多的自动驾驶的数据集中，行人、自行车、卡车的数量加起来还没有小轿车多。这种情况下，用模型对交通参与物分类的精度作为衡量模型表现的标准，恐怕意义不大。这种情况下，***应当先对不同类别样本的分类精度进行一致性检验，或者采用一些适用于不均衡数据的评估指标***，例如Kappa系数，Matthews相关系数等。

### Use Statistics to Compare Models

**McNemar test** for classifier

**Student's T test** for statistical significance (note the requirements)

**ANOVA** for evaluate the difference of two means

**Bonferroni correction**: to counteract the problem of multiple comparisons

**Effect size**: the significance level can be effected by the sample size

- Cohen's d
- Kolmogorov-Smirnov







