---
title: "GPT、GPT2、GPT3学习笔记"
date: 2022-01-09
featured: false
draft: false
comment: true
toc: true
reward: false
pinned: false
carousel: false
math: true
series:
  - paper
---



## GPT

> 学习链接：[GPT，GPT-2，GPT-3 论文精读【论文精读】_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1AF411b7xQ/?spm_id_from=333.337.search-card.all.click&vd_source=c2f7f4fff13c57e0c204150799a5d8e4)

### 一、在GPT之前，NLP中存在哪些问题？

- 有标签的数据量太少
  - 不仅有标签的数据量少，而且一句话中所含的信息是远小于图像中的信息，所以需要更多数据训练。
  - 所以，GPT中使用了先通过无标签的数据训练一个大型的语言模型，然后再在不同的下游任务上微调，进行有监督的学习。
- 优化目标
  - 对于没有标签的数据，应该去怎么设计优化目标来训练语言模型。
  - 
- 有效地将文本表示传递到子任务
  - nlp中不同的子任务依赖的文本特征表示差别比较大



### 二、GPT与BERT的区别

- 结构上的区别：
  - GPT用的是transformer的decoder，BERT用的是transformer的encoder
  - 但这样设计的原因是由它们的优化目标所决定的，GPT中设计的任务是利用极大自然估计的n-gram，和传统的语言模型是一样的，根据前n-1个词预测第n个词。
