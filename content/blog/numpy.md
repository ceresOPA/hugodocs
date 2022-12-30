---
title: "numpy常用操作"
date: 2022-12-08
draft: false
toc: true
featured : true
tags:
  - machinelearning
categories: ["学习"]
---

## numpy数组的拼接

- 使用np.append()拼接

使用append，会对原numpy数组进行复制，分配新的内存空间，不改变原对象。

最简单的使用如下，会把后一个数组的值都加给前一个数组（注意前面说的，虽然说是加，但其实是加给复制的新数组）：

```python
a = np.array([1,2,3])
b = np.array([4,5,6])
c = np.append(a,b) # [1,2,3,4,5,6]
```

需要知道的是，当np.append()没有指定axis这个参数时，会将后一个数组flatten后给到前一个数组。也就是说，没有给定axis下，是可以后一个数组的shape和前一个不同的：

```python
a = np.array([1,2,3])
b = np.arange(6).reshape(2,3)
c = np.append(a,b)
```



