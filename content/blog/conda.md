---
title: "conda常用操作"
date: 2022-12-13
draft: false
toc: true
featured : true
categories: ["学习"]
---

anaconda 是一款能够创建与管理python虚拟环境的工具，这里记录下日常需要用到的conda命令。

<!-- more --> 

## 查看帮助

> 很重要，不记得了很多时候通过help就参数就可以解决

```shell
conda -h
conda create -h
...
```

## 创建虚拟环境

```shell
conda create -n env_name python=3.8 
```

## 进入虚拟环境

一般的话，在创建完成之后，可以通过activate直接进入，现在新的版本应该需要conda activate才可以

```shell
conda activate env_name
```

但首次进入的时候可能会失败，这个时候可以尝试使用下面的命令，来激活环境：

```shel
source activate env_name
```

