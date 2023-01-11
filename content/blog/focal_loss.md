---
title: "Focal Loss损失函数"
date: 2022-12-31
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

有听说Focal Loss可以用来解决类别不平衡的问题，但看了后发现，更确切的应该说是在解决某些样本难以训练的问题，该损失函数是在[Focal Loss for Dense Object Detection](https://arxiv.org/pdf/1708.02002%3E)这篇论文中提出的，这里给出一个代码实现，方便以后使用。

<!--more-->

> 参考链接：[https://zhuanlan.zhihu.com/p/266023273](https://zhuanlan.zhihu.com/p/266023273)

简单总结：

1. focal loss是交叉熵损失函数的改进方法，在原交叉熵函数的基础上引入了调制参数。
2. focal loss不同于平衡交叉熵损失函数，平衡交叉熵损失函数是给不平衡的类别赋予不同的权值，而focal loss关注的是降低难以训练的样本的难度。

## Focal Loss的Pytorch实现

> 参考链接：[https://github.com/DingKe/pytorch_workplace/blob/master/focalloss/loss.py](https://github.com/DingKe/pytorch_workplace/blob/master/focalloss/loss.py)

参考了网上的代码后，微改的一个精简版Focal Loss：

```python
import torch
import torch.nn as nn
import torch.nn.functional as F

class FocalLoss(nn.Module):

    def __init__(self, gamma=0, eps=1e-7):
        super(NewFocalLoss, self).__init__()
        self.gamma = gamma
        self.eps = eps

    def forward(self, input, target):
        y = F.one_hot(target, input.size(-1))
        logit = F.softmax(input, dim=-1)
        logit = logit.clamp(self.eps, 1. - self.eps)

        loss = -1 * y * torch.log(logit) # cross entropy
        loss = loss * (1 - logit) ** self.gamma # focal loss

        return loss.mean()
```