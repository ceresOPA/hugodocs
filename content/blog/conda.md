---
title: "conda常用操作"
date: 2022-12-13
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

anaconda 是一款能够创建与管理python虚拟环境的工具，这里记录下日常需要用到的conda命令。

<!--more--> 

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

## 配置conda镜像源

> 参考链接：[(9条消息) 使用.condarc对Anaconda更换国内镜像_如何 在.condarc 添加国内镜像 升级 anaconda_不同林的博客](https://blog.csdn.net/aclplr/article/details/107166476)

可以查看目前配置的源

```
conda config --show-sources
```

在用户目录下创建~/.condarc，复制下面内容过去：

```
channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/menpo/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
  - defaults
show_channel_urls: true
```

还原默认镜像源可以：

```
conda config --remove-key channels
```

