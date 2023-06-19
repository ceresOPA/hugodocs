---
title: "BERT"
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

<!--more-->

# BERT

> 视频链接：[BERT从零详细解读，通俗易懂！！_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Ey4y1874y?p=1&vd_source=c2f7f4fff13c57e0c204150799a5d8e4)

## 一、bert整体模型架构

![image-20230507095857730](https://img.yulegend.cn/img/image-20230507095857730.png)

bert-base有12层的transformer encoder

bert-large为24层

原transformer

## 二、bert的输入

![image-20230507100622563](https://img.yulegend.cn/img/image-20230507100622563.png)

[SEP]是为了区分句子，[CLS]用于NSP(next sentence predicting)任务时的后续接二分类

segment embeddings 是用来区分不同的句子

## 三、如何做预训练

### MLM

不同与AR（自回归）任务，依赖于MLM属于AE（自编码）

![image-20230507101828428](https://img.yulegend.cn/img/image-20230507101828428.png)

### NSP

![image-20230507102616928](https://img.yulegend.cn/img/image-20230507102616928.png)

## 四、下游任务

![image-20230507103206532](https://img.yulegend.cn/img/image-20230507103206532.png)



![image-20230507103222211](https://img.yulegend.cn/img/image-20230507103222211.png)



![image-20230507103300845](https://img.yulegend.cn/img/image-20230507103300845.png)