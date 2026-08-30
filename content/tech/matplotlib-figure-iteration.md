---
title: "matplotlib 配图迭代：从柱状图到权衡曲线"
date: 2026-08-23
draft: false
tags: ["matplotlib", "可视化", "论文配图"]
math: false
---

论文里的图不是一次画对的。以 AAFA 的「特征稀疏性」图为例，我们经历了
`柱状图 → 雷达图 → 权衡曲线` 三轮迭代，才找到最能支撑 claim 的画法。
本文把每版的代码与取舍都留下来，方便复用。

<!--more-->

## 第 1 版：柱状图（看绝对差异）

```python
import matplotlib.pyplot as plt

ratios = [0.12, 0.25, 0.40, 0.55, 1.00]
acc = [88.1, 89.6, 90.3, 90.5, 90.5]

fig, ax = plt.subplots(figsize=(6, 4))
ax.bar([str(r) for r in ratios], acc, color="#2f6f4f")
ax.set_ylabel("ACC (%)"); ax.set_xlabel("atoms 比例")
plt.tight_layout(); plt.savefig("v1_bar.png", dpi=150)
```

> 问题：柱子之间是离散的，看不出「比例越多、精度收敛」的连续趋势。

## 第 2 版：雷达图（看多维贡献）

雷达图适合展示每个 atom 的单独贡献，但当核心信息是「一条权衡曲线」时，
雷达反而让读者抓不住重点，遂弃用。

## 第 3 版：权衡曲线（最终选定）

x 轴为 atoms 比例、y 轴为 ACC，一条平滑曲线同时讲清「省了多少特征、
丢了多少精度」。

```python
import numpy as np
from scipy.interpolate import make_interp_spline

x = np.array(ratios) * 100
y = np.array(acc)
xs = np.linspace(x.min(), x.max(), 200)
ys = make_interp_spline(x, y)(xs)

fig, ax = plt.subplots(figsize=(6.5, 4))
ax.plot(xs, ys, color="#2f6f4f", lw=2.2)
ax.scatter(x, y, color="#2f6f4f", zorder=5)
ax.set_xlabel("使用的 atoms 比例 (%)")
ax.set_ylabel("ACC (%)")
ax.set_ylim(87, 91)
ax.grid(alpha=.3)
plt.tight_layout(); plt.savefig("v3_tradeoff.png", dpi=150)
```

## 经验小结

- 先想清楚要支撑的 **claim**，再选图，而不是反过来。
- 离散比较用柱状图，多维结构用雷达图，连续权衡用曲线。
- 固定 `dpi`、统一字体与配色，论文多图才协调。
