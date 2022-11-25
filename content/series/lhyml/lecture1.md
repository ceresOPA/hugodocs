---
type: docs 
title: 【Lecture 1】机器学习基本概念简介【笔记】
date: 2022-11-25
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

<!--more-->

## 一. 什么是机器学习？

* 就是要找到一个函数
    * 给定一个输入，给出一个输出
* 任务
    * Regression
    * Classification
    * Structured learning(黑暗大陆)
        * 我们不仅是要机器能够做出选择，

### 1. Function with Unknown Parameters

- 为什么是$y=wx+b$ ?
  - 需要依赖于Domain Knowledge(领域知识 猜测)
      - 对于预测问题，我用前一天预测后一天，我有理由认为后一天(y)与前一天(x)相关，所以给x一个系数w，表示和前一天(x)相关，但是又不可能完全相似，所以又给了一个修正b。
  - 带未知参数的函数就是Model($y=wx+b$)，其中$x$是feature，$w$是weight，$b$是bias。

### 2. Define Loss from Training Data

* Loss也是一个函数，$Loss(b,w)$
* 用来衡量与真实标签的差距
* 损失函数
    * MAE，mean absolute error
    * MSE，mean square error
    * cross-entropy
* Error Surface(图+代码)

### 3. Optimization

* Gradient Descent
    * Randomly Pick an initial value $w^0$
    * Compute $\frac{\partial L}{\partial w}|_{w=w^0}$
        * $w^1 \leftarrow  w^0 - \eta\frac{dL}{dw}|_{w=w^0}$
        * $\eta$ learning rate
    * Update w iteratively
    * 局部最优不是梯度下降真正的痛点
