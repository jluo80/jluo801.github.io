---
title: 树系列笔记？：XGBoost: A Scalable Tree Boosting System
date: 2017-08-16 10:49:31
---

XGBoost是由大神Tianqi Chen在另一位大神Friedman的“Stochastic gradient boosting”论文基础上的改进。XGBoost目前是很多数据比赛运用的主流方法，因XGBoost的树结构简单、可并行等特性而得到广泛使用。基于树结构的机器学习算法，本空间会在后续中记录其余不同方法。

### XGBoost算法原理
简单的说XGBoost就是通过建立多棵树，每棵树分别对上一棵树的（分类或者回归）结果与实际值的残差，进行继续拟合，从而通过不断地建树迭代使得结果最优，损失函数最小。

#### Regularized Learning Object
$ y_k=\varphi(u_k+v_k)$ 
$J\alpha(x) = \sum{m=0}^\infty \frac{(-1)^m}{m! \Gamma (m + \alpha + 1)} {\left({ \frac{x}{2} }\right)}^{2m + \alpha}$
注意下面的写法：(右对齐)
$$ y_k=\varphi(u_k+v_k)$$


























