---
title: "Positional Encoding"
date: 2022-1-4
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

最近在弄transformer处理时序数据的时候，发现怎么用transformer的效果并不怎么好，结果发现原来torch里的transformerencoder并没有去实现positional encoding。。。

<!--more-->

## Positional Encoding的Pytorch实现

下面的实现是使用了sin和cos函数进行位置编码的方式：

```python
class PositionalEncoding(nn.Module):
    "Implement the PE function."

    def __init__(self, d_model, dropout=0, seq_len=5000):
        super(PositionalEncoding, self).__init__()
        self.dropout = nn.Dropout(p=dropout)

        # 初始化Shape为(seq_len, d_model)的PE (positional encoding)
        pe = torch.zeros(seq_len, d_model)
        # 初始化一个tensor [[0, 1, 2, 3, ...]]
        position = torch.arange(0, seq_len).unsqueeze(1)
        # 这里就是sin和cos括号中的内容，通过e和ln进行了变换
        div_term = torch.exp(
            torch.arange(0, d_model, 2) * -(math.log(10000.0) / d_model)
        )
        # 计算PE(pos, 2i)
        pe[:, 0::2] = torch.sin(position * div_term)
        # 计算PE(pos, 2i+1)
        pe[:, 1::2] = torch.cos(position * div_term)
        # 为了方便计算，在最外面在unsqueeze出一个batch
        pe = pe.unsqueeze(0)
        # 如果一个参数不参与梯度下降，但又希望保存model的时候将其保存下来
        # 这个时候就可以用register_buffer
        self.register_buffer("pe", pe)

    def forward(self, x):
        """
        x 为embedding后的inputs，例如(1,7, 128)，batch size为1,7个单词，单词维度为128
        """
        # 将x和positional encoding相加。
        x = x + self.pe[:, : x.size(1)].requires_grad_(False)
        return self.dropout(x)
```

