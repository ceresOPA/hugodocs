---
title: "Linux服务器配置xrdp远程桌面连接"
date: 2023-05-08
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

> 参考链接：[(9条消息) 如何实现远程RDP连接到Centos7_centos 开启rdp远程访问_Mr.BlackJake的博客-CSDN博客](https://blog.csdn.net/qq_43767539/article/details/115257441#:~:text=3.1在键盘上输入win%2BR，输入"mstsc"启动远程桌面连接应用程序，然后输入主机名或IP： 3.2输入用户名root和服务器密码，点击ok连接CentOS,7服务器： 3.3您现在可以使用GNOME通过xRDP连接到CentOS 7服务器：)

## 安装图形化界面

可以通过下面命令来查看是否已安装图形化界面

```
systemctl get-default
```

若输出为`graphical.target`，则表示已安装图形化界面。即可跳过该步骤。

若没有，则可以按下面命令安装gnome，其他图形化界面也可自由选择。

```
yum groupinstall "GNOME DESKTOP" -y
```

## 安装xrdp

```
yum install xrdp -y
```

## 启动并运行xrdp服务

```
systemctl enable xrdp
systemctl start xrdp
```

## 开放防火墙端口

```
firewall-cmd --add-port=3389/tcp --permanent
firewall-cmd  --reload
```

关于firewall-cmd命令的使用，可以参考[使用firewall-cmd命令开放及查看端口开放情况 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/554046297)

## 使用windows进行连接

打开mstsc，输入对应的ip，即可进行连接

![image-20230508091704232](https://img.yulegend.cn/img/image-20230508091704232.png)