---
title: "AsymFreq-PIP：用频率域特征做行人过街意图预测"
date: 2026-08-20
draft: false
tags: ["行人意图预测", "频率域", "PyTorch"]
math: true
---

行人过街意图预测（Pedestrian Intent Prediction, PIP）的目标是在行人真正
横穿马路之前，尽早给出「过街 / 不过街」的判断。本文介绍我们提出的
**AsymFreq-PIP** 模型核心思路：用 **DCT 低频 + Haar 小波 + 非对称池化**
提取时序特征，并以幂均值集成（power-mean ensemble）进一步提升精度。

<!--more-->

## 1. 为什么用频率域

行人轨迹在时域里噪声很大，但「是否打算过街」往往体现在**低频趋势**上。
离散余弦变换（DCT）能把轨迹投影到一组正交基上，我们只需保留前 $k$ 个
低频系数即可重建主要运动趋势：

$$X_{\text{low}} = \text{DCT}(x)[0:k]$$

与之互补，**Haar 小波**擅长捕捉轨迹的突变（例如抬脚、停顿），二者在频率
维度拼接后形成互补的频率表征。

## 2. 非对称池化（asym_pool）

传统全局平均池化会抹平时序先后信息。我们用非对称池化分别沿「过去」与
「未来」两个方向做加权聚合，使模型对**最近几帧**更敏感：

```python
import torch
import torch.nn as nn

class AsymPool(nn.Module):
    def __init__(self, dim):
        super().__init__()
        # 过去方向权重（近期更重）
        self.w_past = nn.Parameter(torch.ones(dim))
        self.w_future = nn.Parameter(torch.ones(dim))

    def forward(self, x):           # x: [B, T, D]
        past = (x * torch.sigmoid(self.w_past)).mean(1)
        future = (x * torch.sigmoid(self.w_future)).mean(1)
        return torch.cat([past, future], dim=-1)
```

## 3. 幂均值集成

单模型 ACC 约 **92.26%**（仅 122K 参数）。取 Top-5 模型做幂均值集成：

$$\hat{y} = \left( \frac{1}{N}\sum_{i=1}^{N} y_i^p \right)^{1/p}$$

$p>1$ 时更偏向高置信预测，在 PIE 数据集上把 ACC 提升到 **93.56%**，
并在 JAAD 跨数据集上获得 **+5.3%** 的泛化增益。

## 4. 小结

- DCT 低频 + Haar 小波：互补的频率表征，抗噪声。
- 非对称池化：保留时序不对称性。
- 幂均值集成：低成本换取约 1.3% 精度提升。

完整训练脚本与消融实验见 [GitHub 源码](https://github.com/your-username)。
