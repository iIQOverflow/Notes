# L1 and L2 Regularization Methods

## L1 正则化（Lasso Regression）
损失函数中增加每个系数的绝对值作为惩罚项。

$$\sum_{i=0}^n(y_i-\sum_{j=1}^px_{ij}\beta_j)^2+\lambda\sum_{j=1}^p|\beta_j|$$
如果$\lambda=0$，相当于没有惩罚项，可能会造成过拟合，$\lambda$过大会导致系数趋近于0而造成欠拟合。
## L2正则化（Ridge Regression）
损失函数中增加每个系数的平方作为惩罚项。

$$\sum_{i=0}^n(y_i-\sum_{j=1}^px_{ij}\beta_j)^2+\lambda\sum_{j=1}^p\beta_j^2$$

如果$\lambda=0$，相当于没有惩罚项，可能会造成过拟合，$\lambda$过大会导致系数趋近于0而造成欠拟合。因此如何选择$\lambda$是十分重要的，在防止过拟合方面它十分有效。

## 总结
L1与L2的差异在惩罚项的不同。L1可以将不是特别重要的系数收缩至0，相当于移除了这个特征，当我们有大量的特征进行选择时这十分有效。


## Reference
[L1 and L2 Regularization Methods](https://towardsdatascience.com/l1-and-l2-regularization-methods-ce25e7fc831c)
