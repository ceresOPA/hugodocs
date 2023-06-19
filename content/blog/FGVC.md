---
title: "FGVC"
date: 2022-6-19
featured: false
draft: true
comment: true
toc: true
pinned: false
carousel: false
math: true
categories: ["学习"]
---

<!--more-->

> 为了能够拿到money！！！

> CVPR 2023 FGVC workshop

训练集大小：48988

测试集大小：12247

**日期：2023年5月8日**

比赛的目的是实现根据给出的图片、section，已经部分词被mask后的headline，预测出被mask后的词。

给出的baseline中只使用了headline，采用deberta-base-uncased模型直接预测出mask后的词，也就是基本的MLM任务。（一点想法：这个任务本身就是个代理任务吧，在bert中和NSP一起用于模型的预训练，并不是常做的下游任务。）

那既然官方给了deberta，所以想着那就找个更新更大的deberta模型，所以就去拿了微软最新出的debertav3-large，但是，直接进行预测后的效果，就并不好。明明是只有英文的句子的mask，结果预测出的mask的词还出现了各种不同的语言，默认输出的词也是首字母大写，不过这个倒是不重要，自己处理一下就好。所以，有在想，这种多语言语料训练后的，反而不太合适？

既然这样的话，还是先用回baseline，先使用baseline中的deberta-base-uncased模型通过训练集进行微调。

这里是单独初始化了model和tokenizer，任务选择fill-mask

```python
model = AutoModelForMaskedLM.from_pretrained(model_name, cache_dir="./model")
tokenizer = AutoTokenizer.from_pretrained(model_name, cache_dir="./model")
unmasker = pipeline('fill-mask', model=model, tokenizer=tokenizer)
```

原始的输入文本：`What 's on TV Monday : ' The Voice ' and ' Gentefied`

调用tokenizer：`{'input_ids': [101, 2054, 1005, 1055, 2006, 2694, 6928, 1024, 1005, 1996, 2376, 1005, 1998, 1005, 8991, 2618, 10451, 1005, 102, 0], 'attention_mask': [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0]}`

```python
t = tokenizer(headline, padding="max_length", truncation=True, max_length=128)
```

使用tokenizer.decode()：`[CLS] what's on tv monday :'the voice'and'gentefied'[SEP] [PAD]`

```python
tokenizer.decode(t["input_ids"])
```

这里的attention_mask指的是为1的是参与训练推理的，为0的不参与。

> 参考链接：[Huggingface🤗NLP笔记5：attention_mask在处理多个序列时的作用 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/414511434)

**日期：2023年5月9日**

昨天自己用deberta-base-uncased训了一下模型，但发现预测出的结果大部分都变为了[PAD]，应该是哪里写的有问题。因为没有找到直接微调的代码，所以自己做了掩码的预处理（15%掩码，其中的80%用mask，10%随机换成词表中的词，剩下的10%不变），然后将input_ids（掩码后的），labels和attention_mask作为输入给模型。总之，训了后的模型完全不可用。

既然这样，那再转变下思路，看能不能通过改变输入的句子，来提高预测的结果。因为给的输入有section、headline和image，这个section就是一个单词，类似于一个主题，是可以直接加到句子里的，所以做了两个尝试。一个是`sentence = f"{section}, {headline}"`（score：22），另一个是`sentence = f"the section is {section}. the headline is {headline}"`（score：20），结果发现效果都更差了。

bert和deberta都成功train起来了，预测结果提高到了24，之前 [MASK] 的时候虽然避开了 [CLS] 和 [SEP]，但没有去避开 [PAD]，可能这就是之前训练完后都预测成PAD的原因。

**日期：2023年5月10日**

现在开始把图片加入进来，先选择一个简单的思路——利用image caption将图片转换为文本描述，其他操作保持与之前一致，相当于多引入了一段描述文本。

这里采用了clip-interrogater去进行caption的生成，不过生成的速度有点慢，打算拆一下数据集，并行处理一下。

**总结**

取得了前三的名次，最终采用的方案比较简单，

1. 先是使用了VQA的方式，构建了两个两个prompt，在BLIP2模型上由图片生成了describe和tone，从而将一个多模态的任务完全转换为了nlp的任务；
2. 进而在一些经典模型上进行训练，包括bert以及bert的变体，deberta、distilbert等；
3. 接着就得到了首次训练好后的模型，进行第一次按概率进行集成，得到预测的masked words；
4. 我们将预测出来的词视为伪标签（pseudo label），再喂给模型进行训练，得到二次训练后的多个模型；
5. 最后，将这些模型预测得到的结果进行集成，得到我们最终的预测结果。