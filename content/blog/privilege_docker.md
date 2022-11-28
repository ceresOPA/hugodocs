---
title: "普通用户运行docker容器"
date: 2022-11-28
draft: false
toc: true
featured : false
tags:
  - docker
categories: ["学习"]
---

为了给实验室的服务器控制权限，所以测试下用普通用户运行docker容器，并保持容器内外的权限一致为普通用户。

<!--more-->

> 说实话，docker出现这种内外权限不一致的问题，感觉还是在于docker容器内的gid和uid和宿主机的竟然是有关联的，导致容器内的root用户操作，竟然在容器外也会变成root操作，离谱，这真的不算bug吗？不知道有没有更好的处理方法，现在这样处理还是不太好。

## 1. 新增测试用户

在服务器上新增测试的普通用户及分组：

```shell
groupadd testuser
useradd -g testuser testuser
```

## 2. 分配给docker组

因为需要能够操作docker，所以需要分配到docker组

```shell
gpasswd -a testuser docker
```

## 3. 在工作目录下创建docker容器的挂载目录

因为如果不手动创建的话，由于docker是由root用户启动的，所以会导致自动创建的挂载目录的拥有者会是root，从而导致我们自己无法操作。

```shell
cd ~
mkdir test
```

可以用`ls -l`查看目录权限

![image-20221128164025660](https://img.yulegend.cn/img/image-20221128164025660.png)

## 4. 启动一个新的docker容器

这里启动的容器， 特别指定了默认的uid和gid，这是为了让容器内的权限和外部的一致

```shell
nvidia-docker run --network=host -id --name=cuda_test --rm -p 8889:8888 \
--user $(id -u):$(id -g) -v ~/test:/workspace
--ipc=host --ulimit memlock=-1 --ulimit stack=67108864 \
nvcr.io/nvidia/pytorch:21.09-py3
```

关于这里为什么把挂载端口设置为了8889:8888，这主要是考虑到可能会用到jupyter notebook，notebook的默认启动端口就是8888，所以把容器内的8888端口给映射了出来，如果不用notebook，其实也就不用挂载了。

可以查看启动的容器列表：

![image-20221128164622018](https://img.yulegend.cn/img/image-20221128164622018.png)

## 5. 以默认用户进入容器

这里因为之前在`docker run`的时候指定了`user`，所以默认会以当前的uid和gid进入容器。

```shell
docker exec -it cuda_test bash
```

![image-20221128165113238](https://img.yulegend.cn/img/image-20221128165113238.png)

其实到这里就已经可以用了，内部的gid和uid和外部保持了一致，这样在内部创建文件，或外部修改文件，也不会导致权限不一致的问题了。但如果说，看着I have no name！不舒服的话，那可以再做接下来的操作。

## 6. 以root用户进入容器并添加当前宿主机用户的uid和gid

root身份进入容器后，执行下面的命令就可以了，对应的1003和1004，自己可以通过在宿主机上执行`id`命令查看自己的uid和所在的gid。

```shell
addgroup --gid 1002 yule2022
adduser --uid 1002 yule2022 --ingroup yule2022 --disabled-password
```

这样设置完成后，重新再进入容器中，就不再是I have no name!了。

![image-20221128165429551](https://img.yulegend.cn/img/image-20221128165429551.png)



