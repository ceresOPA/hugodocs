---
title: Wandb简单使用
date: 2022-12-30
draft: false
toc: true
featured : false
tags:
  - deep learning
categories: ["学习"]
---

wandb可以用于模型训练中的评价指标的跟踪，方便模型的调参、优化等工作，对于实验结果的保存真的非常方便，可以让实验做的非常有条理，可以说是相当高效便捷的工具。

<!--more-->

> 对于在模型训练中的实验结果记录的话，平常用的最简单的，大概就是用list保存一下，然后用matplotlib画画图之类的。有时可能也还会用到tensorboard，方便记录。但是最近在使用服务器跑模型的时候，感觉这两种方法都不太方便，尤其是我在docker容器中，还得端口映射出来才能用tensorboard。所以，不如直接使用wandb来得方便，可以把配置信息都记下来，打印的日志也都有自动保存上传到云端，想要保存的文件都可以上传。

wandb平台：[wandb.ai](https://wandb.ai/)

## wandb安装

直接使用pip安装就可以，但是要稍微注意的话，像我在服务器里不是root用户，然后用pip安装后，wandb并不会配置到环境变量，也就是我在安装完成后，执行wandb命令是不存在的，这个时候要么得自己配置，要么可以用root来安装，因为我是在docker容器内，还是可以切root用户的。

```python
pip install wandb
```

## wandb登录

可以先到wandb网站上，先注册好账号，然后就可以拿到API key，用于登录。

在命令下执行：

```
wandb login
```

粘贴API key，即可完成登录。

## wandb使用

使用的方式也是挺简单的，一个是初始化，一个是保存参数。

`init`	`log`	`finish`

```python
import wandb

wandb.init(
    project="sleepstaging",
    
    config={
    "learning_rate": 0.001,
    "n_epochs": 150,
    "network": "BiLSTM"
    }
)

for idx in n_epochs:
	model training...
	model validating...
	
    wandb.log({"train_loss": np.sum(train_loss), 
    "train_acc": np.mean(train_avg_acc),
    "val_loss": np.sum(validate_loss),
    "val_acc": np.mean(validate_avg_acc)})

wandb.finish()
```

