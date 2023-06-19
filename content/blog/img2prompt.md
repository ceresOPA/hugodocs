---
title: "img2caption"
date: 2023-5-20
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

这个比赛过程没有怎么记录下来有点可惜，最后拿了铜奖，前9%，感觉也还可以，是第一次认真打的kaggle并拿了奖（断断续续做了有一个月的样子）。没有什么太多的个人创新点，主要还是结合了大家分享的方法，然后自己在代码层面上做了些小改，包括后续集成的部分。

<!--more-->

> Kaggle地址：[Stable Diffusion - Image to Prompts | Kaggle](https://www.kaggle.com/competitions/stable-diffusion-image-to-prompts/discussion/398529)

## 一、下载数据

数据地址：https://huggingface.co/datasets/poloclub/diffusiondb

一开始使用了huggingface提供的datasets库进行数据的下载与加载：

```python
diffusion_dataset = load_dataset('poloclub/diffusiondb', '2m_first_10k', cache_dir="./cache", split="train")
```

以为会更加方便，但实际上，因为我需要额外的对原始数据进行处理和过滤，而且后续还会假如别的数据集，所以还不如直接去使用提供的另一个脚本去下载，因为到后面还是要用自己定义的函数进行数据的加载。（2023，4月3日 记）

提供的下载脚本地址：[diffusiondb/download.py at main · poloclub/diffusiondb (github.com)](https://github.com/poloclub/diffusiondb/blob/main/scripts/download.py)

```python
# 修改urllib代理
from urllib.request import ProxyHandler, build_opener

proxy_url = "http://127.0.0.1:7890"
proxy_url_dict = {
    'http': proxy_url,
    'https': proxy_url
}

proxy_handler = ProxyHandler(proxy_url_dict)
opener = build_opener(proxy_handler)

install_opener(opener)
```

难顶，上面两种方法都还是会断掉，即使是在我使用了代理的情况下（使用代理下还需要修改代码，发现底层都是通过urllib这个库来进行下载的，所以还需要在代码里设置代理），不知道是不是我的网太差了。

目前的解决方法是在kaggle上找了别人上传后的数据集，一开始选择了别人上传的原始数据集，但是还是会断掉，主要是一个包就50多G，还不支持断点续传，所以就还挺搞的，所以，现在处理的是别人将图片压缩成224X224的数据集，大小比之前小了很多，虽然说可能会导致模型效果差，但也算是有2百万张图了。





[Stable Diffusion - Image to Prompts | Kaggle](https://www.kaggle.com/competitions/stable-diffusion-image-to-prompts/discussion/396136)

[Use vector search for Diffusion-DB Cleansing | Kaggle](https://www.kaggle.com/code/tomokihirose/use-vector-search-for-diffusion-db-cleansing)

[Stable Diffusion - Image to Prompts | Kaggle](https://www.kaggle.com/competitions/stable-diffusion-image-to-prompts/discussion/388080)