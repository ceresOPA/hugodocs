---
title: KL散度
date: 2022-12-28
featured: false
draft: false
comment: true
toc: true
reward: false
pinned: false
carousel: false
math: true
categories: ["学习"]
---



KL散度



<!-- more -->



## 什么是KL散度

KL散度，也叫作相对熵，全称是Kullback-Leibler divergence，是由Kullback和Leibler在1951年提出的。

KL散度用于衡量两个概率分布之间的相似程度，如对于随机变量Q的概率分布，相对于随机变量P的概率分布的KL散度定义为：
$$
D_{KL}(P||Q) = H(P,Q)-H(P)
$$
其中$H(P)$表示随机变量$P$的熵，离散形式为$H(P) = \sum_{i}p(i)log{p(i)}$，类似的$H(P,Q) = \sum_{i}p(i){logq(i)}$，$p(i)$为P的概率分布，$q(i)$为Q的概率分布。（其实也就是交叉熵公式）计算过程有：
$$
\begin{align}
 D_{KL}(P||Q) &= H(P,Q)-H(P) \\
 &= \sum_{i}p(i)log{p(i)}-\sum_{i}p(i)log{q(i)} \\
 &= \sum_{i}p(i)log{\frac{p(i)}{q(i)}}
\end{align}
$$
上面的是离散形式，相对的，还有连续形式：
$$
D_{KL}(P||Q) = \int p(x)log{\frac{p(x)}{q(x)}} dx
$$

## KL散度的两个特点

### 1. 非对称性

对于概率分布p和概率分布q而言，$D_{KL}(P||Q)\neq D_{KL}(Q||P)$，其中$D_{KL}(P||Q)$指的是Q相当于P的KL散度。

为了解决KL散度非对称性的问题，因此，有提出JS散度，它的定义如下：
$$
D_{JS} = \frac{D_{KL}(P||M)+D_{KL}(Q||M)}{2}
$$
其中$M = \frac{P+Q}{2}$，是两个概率分布的平均值。

### 2. 非负性

当$P$和$Q$分布是一样时，此时$D_{KL}$等于0，当分布不同时，可以证明$D_{KL}$是大于0的。

因为$logx\le x-1$，

所以，
$$
-D_{KL} = -\sum_{i}p(i)log\frac{p(i)}{q(i)} = \\
\sum_{i}p(i)log\frac{q(i)}{p(i)} \le \\
\sum_{i}p(i) \left( \frac{q(i)}{p(i)} -1 \right) = 0
$$
即有，$D_{KL} \ge 0$。

