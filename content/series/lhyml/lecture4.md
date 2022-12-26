---
type: docs 
title: 【Lecture 4】自注意力机制
date: 2022-12-03
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

## 多种多样的Self-Attention机制

### 1. 存在的问题

- 计算量大

### 2. 其他类型的Self-Attention

#### 使用人工的方式，只计算部分的attention，其他部分直接设定

- Local Attention
- Stride Attention
- Global Attention
  - 只计算special token与其他的vector之间的attention，其他vector也只考虑与special token的关系

#### 学习attention metrics的位置

- learnable

#### 减小attention metrics的大小

- 选出有代表性的K个**K**
- Compressed attention
- Linformer

#### 改变矩阵乘法的顺序

- KQV
- $exp(Q*K) = f(Q)*f(K)$
- K Q first$->$V K first

#### Do we need q and k to compute attention?

- Synthesizer