---
type: docs 
title: 【Lecture 2】机器学习调优
date: 2022-11-27
featured: true
draft: false
comment: true
toc: true
reward: false
pinned: false
carousel: false
math: true
series:
  - lhyml
---

在这一章中，首先大概讲了一些机器学习中常见问题的通用解决方法，告诉了我们可以怎样去处理。接着针对类神经网络训练不起来的问题，从局部最小值、批次与动量、学习率和损失函数四个点上分别进行了讲解。

<!--more-->

## 一、机器学习任务攻略

<img src="https://img.yulegend.cn/img/image-20221127163051527.png" alt="image-20221127163051527" style="zoom: 33%;" />

### 训练集上的loss太大了

#### 模型偏差大(Model bias)

- 模型太简单了（need more flexible）
  - 输入更多的特征$x_i$
  - 使用更多的神经元(neuron)
  - 增加网络的深度(layer)
- 当然也是不能让模型过于的复杂，选择一个恰当的点，这是一直需要考虑的问题
  - <img src="https://img.yulegend.cn/img/image-20221127171221552.png" alt="image-20221127171221552" style="zoom:33%;" />

#### 优化问题



#### Model Bias or Optimization Issue ?

- 通过比较不同复杂度的模型的loss
  - 在简单的模型能够得到比较低的loss，但是却在复杂的模型上得到的loss更高，那这个时候就可以考虑是Optimization的问题，因为之前有说，增强模型复杂度，可以缓解Model Bias，但现在明明模型更复杂了，但是loss反而增大了。
- 因此在处理一个新问题时，可以先选择一个简单的模型，线性模型，或者传统机器学习处理，这样一般存在优化的问题。



### 训练集上的loss较小，测试集上的loss大

#### Overfitting

- 更多的训练数据（more training data）
  - 视频中有说更多的数据能够更好地限制住模型
  - 但更准确地说应该是更多的训练数据，会符合整体数据的分布，也就是说只要我的训练数据的分布得到的模型，能够和测试数据的一样，那就能够得到比较好的结果。
- 数据增强（data augmentation）
  - 像图像，可以通过平移、翻转等操作增加出更多有意义的数据
  - 信号的话，也可以通过偏移，来增强数据
- 限制模型的复杂度
  - 简单一些的模型对应的解也会更少，所以可以更快找到比较优的结果（当然也是不能让模型过于简单，太简单的话又会回到欠拟合的情况下）
  - Less parameters(减少神经元)，sharing parameters(CNN)
    - <img src="https://img.yulegend.cn/img/image-20221127171029692.png" alt="image-20221127171029692" style="zoom:25%;" />
  - 更少的特征
  - 早停（early stopping）
  - 正则化（regularization）
  - Dropout

#### mismatch

- 有点类似overfitting，但overfittin更多说的是训练和测试数据的分布应该是差不多的
- 但是在mismatch的情况下，由于测试数据的分布和训练数据的分布差距很大，这样在测试集上的结果肯定是不准的。



### 将训练数据划分为训练集和验证集

<img src="https://img.yulegend.cn/img/image-20221127182418546.png" alt="image-20221127182418546" style="zoom: 33%;" />

这里考虑的点，让我有种醍醐灌顶的感觉。

就是为什么在比赛通常会划分为public和private的测试集？

- 以前我个人以为只是做进一步的评估，这样会更充分一些，因为测试数据越多也越有说服力，或者说private的测试集的分布会有些差异。
- 但其实我们的前提就是训练集和测试集的分布要相似的，这是需要明确的。
- 而为什么会有private测试集，是因为我们在public的测试集上能够看到得分，那只要不断地对照在public上的分数，随机生成结果提交，最终一定能得到比较高的分数，但这其实是毫无意义的。
- 所以需要将训练数据拆分出验证集，然后我们根据验证集上的最好结果，来进行提交。这个时候，虽然能够看得到在public测试集上的得分，但最好也不要去在意，因为如果这个时候又去根据public上的结果去调整，那又回到了上个小点追求高的public测试集的分，但在private上得分低的情况。
- 因此，只要关注验证集就好，那为了让验证集上的结果也更可靠，可以使用K交叉验证(K-Fold cross validation)的方法。

