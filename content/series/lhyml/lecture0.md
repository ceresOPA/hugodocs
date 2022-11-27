---
type: docs 
title: 【Lecture 0】机器学习概况
date: 2022-11-25
featured: false
draft: false
comment: true
toc: true
reward: false
pinned: false
carousel: false
weight: 1
series:
  - lhyml
---

介绍了什么是机器学习，整个课程会分为15章，会依次讲解不同类型的机器学习方法（如有监督学习、自监督学习、生成对抗网络、强化学习、异常检测、可解释的AI、模型压缩等内容）。

<!--more-->

### 1. 什么是机器学习？

- 一类特殊的函数

### 2. 课程结构

- 有监督学习 （lecture1-5）
  - training data
  - label
- 自监督学习 Self-supervised Learning（lecture 7）
  - Pre-train （基本功 unlabeled）先学习基本功，就能够在下游任务上得到好的结果。
    - Pre-trained Model (<font color="red">Foundation Model</font>, example. BERT) vs. Downstream Tasks
    - 类似于 Operation System vs. Software Application

- 生成对抗网络 （lecture 6）
  - 无监督语音转换ASR
  - 无监督机器翻译
- 强化学习
- 异常检测 Anomaly Detection （lecture 8）
- 可解释性的AI  Explainable AI（lecture 9）

- 模型攻击 Model Attack (lecture 10)
  - 加入噪声，模型的预测完全改变
- Domain Adaptation (lecture 11)
- Network Compression (lecture 13)
  - 模型压缩，用于各种嵌入式应用
- Life-long Learning (lecture 14)
  - 为什么机器无法做到一直学习？
- Meta learning = learned to learn
  - 不使用人为设置的算法，而从历史算法中由机器设计出新的算法
  - Few-shot learning
    - 少样本学习会依赖于Meta learning来实现，因为没有人设计的算法能够在Few-shot下得到好的结果，需要meta learning由机器设计出的算法。

