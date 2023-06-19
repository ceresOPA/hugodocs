---
title: matplotlib常用操作
date: 2022-12-30
featured : true
draft: false
comment: true
toc: true
reward: false
pinned: false
carousel: false
math: true
categories: ["学习"]
---

<!--more-->

## 清除画布

```python
plt.clf()
```

清除之前绘制的内容，可以重新绘制

## 取消坐标显示

```python
plt.axis("off")
plt.savefig('Wake.png', dpi=100, bbox_inches='tight', pad_inches=0) # 取消周围白边
```

## 子图绘制

### subplot

```python
plt.figure(figsize=(16,3))
ax1 = plt.subplot(121)
ax1.set_title("original")
ax1.plot(ecg_signal[:500])
ax2 = plt.subplot(122)
ax2.set_title("reconstructed")
ax2.plot(reconstructed_signal[:500])
```

### subplots

```python
fig, axs = plt.subplots(2, 2)
axs[0, 0].plot(x, y)
axs[1, 1].scatter(x, y)
```



