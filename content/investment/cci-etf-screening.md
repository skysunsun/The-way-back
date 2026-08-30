---
title: "把通达信 CCI<-80 选股公式转写成 Python（ETF 筛选）"
date: 2026-08-26
draft: false
tags: ["ETF", "量化筛选", "ptrade"]
math: false
---

我把通达信里「CCI < -80 超卖」的选股思路，转写成 Python 脚本，在 ptrade
平台上对 9 只 ETF 做日频筛选。核心就一句：**CCI 跌破 -80 视为超卖，进入
观察池。**

<!--more-->

## 通达信原公式（参考）

```text
CCI:=(CLOSE-MA(CLOSE,14))/(0.015*AVEDEV(CLOSE,14));
XG: CCI < -80;
```

## Python 转写（pandas）

```python
import pandas as pd

def cci(close: pd.Series, n: int = 14) -> pd.Series:
    ma = close.rolling(n).mean()
    md = (close - ma).abs().rolling(n).mean()   # 平均绝对偏差
    return (close - ma) / (0.015 * md)

def screen(df: pd.DataFrame, n: int = 14, thr: float = -80.0) -> pd.DataFrame:
    df = df.copy()
    df["CCI"] = cci(df["close"], n)
    return df[df["CCI"] < thr]

# 对 9 只 ETF 循环调用 screen()，命中即加入观察池
etfs = ["159351","159209","561580","515180","510880",
        "159399","510720","510180","513690"]
```

## 在 ptrade 上的用法

- 日频任务：收盘后跑一遍 `screen`，输出命中列表。
- 配合「永续持仓 + 波段」框架：超卖 ETF 作为波段加仓候选，不单独择时。
- CCI 仅是信号之一，需结合量能与均线确认，避免钝化期频繁触发。

> 说明：以上为个人策略复盘，用于方法演示，不构成投资建议。
