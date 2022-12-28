---
type: docs 
title: 【Lecture 7】自监督学习
date: 2022-12-28
featured: false
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

## Self-supervised Learning

> 自监督学习属于无监督学习，没有标签，利用自己的一部分样本来训练另一部分的样本。

## Bert

### Masking Input

随机盖住输入的序列中的一些词，然后预测盖住的这些词。

盖住的方式有两种：

- 使用特殊字符[MASK]来替代盖住的词
- 随机使用字典中抽取的词来替代要盖住的词



### 做的两件事情

- Masked token prediction
  - 预测遮住的词，相当于填空
- Next sentence prediction
  - 预测两个句子是不是连贯的