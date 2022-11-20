---
# type: docs 
title: "关于个人博客的迁移"
date: 2022-11-20
featured: false
draft: false
comment: true
toc: true
reward: false
pinned: true
carousel: false
---

{{< alert >}}原来的博客使用的是github pages，然后用cloudflare的CDN加速的，网站访问上确实没什么问题了，但还是图片的问题，一开始的图床放在gitee，但gitee搞了个防盗链(貌似是会检测请求头的refer)，弄不了图床了，后面就直接换到了github图床，但是发现最近用jsDelivr加速github图床也会存在图片加载不出来的问题。所以，一方面是换一换博客主题 (不写笔记，博客主题倒是换的勤快:cry:)，另一方面是现在直接换用了cloudflare pages，以及还采用了cloudflare workers去代理了图床，cloudflare全家桶了，美滋滋，访问起来还是挺不错的。{{< /alert >}}

之前的博客旧址可以访问[https://old.yulegend.cn](https://old.yulegend.cn)，以前的笔记大部分都在那边。

<!--more-->
