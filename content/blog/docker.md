---
title: "docker 基本操作"
date: 2022-11-28
draft: false
toc: true
featured : false
tags:
  - docker
categories: ["学习"]
---



## Centos更新yum源

下载镜像仓库文件

```
wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
```

clean

```
yum clean all
```

makecache

```
yum makecache
```

安装epel，用以提供额外的安装包

```
yum install epel-release
```

替换清华源

```
sudo sed -e 's!^metalink=!#metalink=!g' \
    -e 's!^#baseurl=!baseurl=!g' \
    -e 's!http://download\.fedoraproject\.org/pub/epel!https://mirrors.tuna.tsinghua.edu.cn/epel!g' \
    -e 's!http://download\.example/pub/epel!https://mirrors.tuna.tsinghua.edu.cn/epel!g' \
    -i /etc/yum.repos.d/epel*.repo
```

docker的yum下载源：

```
wget -P /etc/yum.repos.d/ https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```



## docker清理

[如何清理 Docker 占用的磁盘空间 - 腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/1581147)

查看docker的磁盘占用：

```
docker system df
```

一键删除所有已经停止的容器：

```
docker container prune
```

清除镜像：

```
docker image prune
```

清除所有可回收磁盘占用：

```
docker system prune -a --force
```

-a 删除全部未使用的镜像，-f 或 --force 不经过确认强行删除。



## docker升级

[Docker: 教程07 - ( 如何对 Docker 进行降级和升级） - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/307945574)