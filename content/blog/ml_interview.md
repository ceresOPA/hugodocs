---
title: 百面机器学习-精简问答版
date: 2022-12-28
featured: true
draft: false
comment: true
toc: true
reward: false
pinned: false
carousel: false
math: true
categories: ["学习"]
---

本文为在阅读了《百面机器学习》这本书，加入自己的一定理解后，采用较为精简的语言来回答机器学习相关的问题，主要用于知识点的复习与应对面试。

<!--more-->

## 一、特征工程

### 1. 特征归一化

#### 问题1 什么是特征归一化？

答：特征归一化将处于不同数值范围大小的数值特征统一到一个大致相同的数值区间内。常用的归一化方法有Min-Max Scaling和Z-Score Normalization两种方式。

- Min-Max Scaling

线性函数归一化，对原始数据进行线性变换，使结果映射到[0,1]范围。公式为：$X_{norm} = \frac{X-X_{min}}{X_{max}-X_{min}}$，其中$X$为原始数据，$X_{min}$和$X_{max}$分别为原始数据中的最小值和最大值。

- Z-Score Normalization

零均值归一化，将原始数据映射到均值为0，标准差为1的分布上。公式为：$X_{norm} = \frac{X-\mu}{\sigma}$，其中$X$为原始数据，$\mu$为均值，$\sigma$为标准差。

#### 问题2 为什么要对数值特征进行归一化？

答：因为在进行梯度下降，更新梯度的时候，相同学习率下，不同数值范围的特征的权值更新速度会不一样，如果能够归一化到一致的数值范围，就可以更容易更快地找到最优解。

> 归一化可以适用于使用梯度下降求解的算法，但不适用于决策树模型，如C4.5方式下，使用信息增益比进行节点分裂，但是归一化后并不会改变信息增益比。

### 2. 类别型特征

#### 问题1 在对数据进行预处理时，应当怎样处理类别型特征？

答：常用的处理方法有序号编码、独热编码(One-Hot)和二进制编码。

序号编码通常用于处理具有大小关系的特征，如大、中、小这种，就可以编码为3、2、1；

独热编码通常用于处理不具有大小关系的特征，得到的特征向量只有对应类别的维度为1，其他都为0，是一种稀疏矩阵；

二进制编码有两个步骤，会先用序号编码转换为数值，然后将数值对应的二进制编码作为结果。

### 3. 组合特征

> 组合特征这块不太了解，没怎么理解

#### 问题1 什么是组合特征？

答：将一阶离散特征进行两两组合，构成高阶组合特征，适合于离散特征的取值比较少的情况，如果取值个数过多，则两两组合会导致参数规模过大。

#### 问题2 为什么要使用组合特征？

答：使用组合特征可以提高模型的拟合能力。

#### 问题3 怎样有效地找到组合特征？

答：并不是任一特征的组合都是有意义的，因此可以采用基于决策树的特征组合寻找方法。

### 4. 文本表示模型

#### 问题1 有哪些文本表示模型？它们各有什么优缺点？

> 知乎参考链接：[语言模型 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/90741508)

答：目前主要有基于统计方法的语言模型和基于神经网络的语言模型。

基于统计方法的语言模型常用的有词袋模型，通常的做法可以直接对词频进行统计，得到的文本的特征向量的维度会等于字典的长度，而其中的每一维表示的是这个词出现在这个文本中的次数。除此之外，也可以采用TF-IDF的方法来评估每个维度的词对文本的重要程度，也就是权值，计算方法为$TF-IDF(t,d) = TF(t,d)*IDF(t)$，

其中$TF(t,d)$表示单词$t$出现在文本$d$中的频率，$IDF(t) = log\frac{文本总数}{出现单词t的文本数+1}$，$IDF(t)$反映了单词$t$对表达语义所起的重要性，若每个文本都有这个单词，那这个单词就不具备区分文本的能力了。

基于神经网络的语言模型， 常见的有Word2Vec、Glove、ELMo和Bert等。

Word2Vec的话，有CBOW和Skip-Gram两种网络结构，CBOW是用当前词去预测上下文的词，而Skip-Gram是用上下文的词去预测当前词。

GloVe是一个基于全局词频统计的词表征模型，生成的词向量可以捕捉到词之间的语义特征，可以计算出不同单词之间的相似性。

ELMo能够基于不同的上下文对同一词生成不同的词向量，因为对于之前的Word2Vec和GloVe得到的词向量都是上下文无关的，无法很好地处理多义词的问题，整体架构是基于双层的LSTM的。

Bert，基于transformer的encoder部分，在训练的时候主要进行了两项任务：1. 填词，会随机mask一些词，然后让模型去预测这些词；2. 判断两个句子的顺序，看下一句是否是在前一句之后的。

### 5. 数据增强

#### 问题1 训练数据不足会带来什么样的影响？应该怎么解决？

答：训练数据不足，可能会导致过拟合，让模型在训练集上表现良好，但是在测试集上表现很差。可以通过L1、L2正则化、Dropout等来降低模型的复杂度，提高泛化能力。也可以进行数据增强，扩充数据集，对于图像数据集，可以采用平移、旋转等预处理方法，对于信号，则可以采用信号偏移等方法。

## 二、 模型评估



| (True Positive) TP  | (False Positive) FP |
| ------------------- | ------------------- |
| (False Negative) FN | (True Negative) TN  |



$precision = \frac{TP}{TP+FP}$ ，预测为正样本的里面，有多少是真正的正样本。

$recall = \frac{TP}{TP+FN}$，实际为正样本的里面，预测出多少正样本。

$sensitively = \frac{TP}{TP+FN}$，灵敏度，实际为正样本里面，预测出多少正样本。

$specifically = \frac{TN}{TN+FP}$，特异度，实际为负样本里面，预测出多少负样本。