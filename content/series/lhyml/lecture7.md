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

### Next Sentence Prediction

判断两个句子的是不是应该接在一起

改进的方法：

- ALBERT，SOP方法，Sentence Order Prediction
- 预测句子的顺序，不只是简单地判断是不是可以连在一起，而是判断哪个句子是在前面，哪个句子是在后面的。

### Fine-tune

Bert是一个pre-train的model，可以通过fine-tune，也就是微调，用于各种不同的下游任务。

### How to use Bert

#### sentiment analysis

通常使用[CLS]特殊字符的输出作为最后分类器的输入

#### POS tagging 词性标注

每一个token的输出都输入到一个分类器

#### Natural Language Inferencee

输入两个句子，输出一个分类，可以用于看两个句子是不是矛盾或合理的任务。

<img src="https://img.yulegend.cn/img/image-20221228204317781.png" alt="image-20221228204317781" style="zoom: 33%;" />

#### 问答系统

- 适用于从文中找问题的答案的任务
- 输入的是问题和文章，输出两个整数

<img src="https://img.yulegend.cn/img/image-20221228204359443.png" alt="image-20221228204359443" style="zoom:33%;" />

详细的过程如下图，会有两个额外的随机初始化权值的向量，分别与文章部分输出的token进行内积，再经过一个softmax，拿概率最高的token的下标，这两个额外的向量与文章的token进行运算后，得到的两个下标，s和e，分别表示的就是输出的回答在文中的开始与结尾。

<img src="https://img.yulegend.cn/img/image-20221228204759119.png" alt="image-20221228204759119" style="zoom:33%;" />

### 做的两件事情

- Masked token prediction
  - 预测遮住的词，相当于填空
- Next sentence prediction
  - 预测两个句子是不是连贯的