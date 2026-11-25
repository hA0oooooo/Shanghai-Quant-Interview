##### 加权零均值决定系数的分块归约

评测数据按批到达，每行包含标签 $y$、预测 $p$ 与非负官方权重 $w$，均为有限数。要跨批次或机器合并整段加权零均值 $R^2$，只需保存误差平方和 $E=\sum w(y-p)^2$ 和零基准平方和 $D=\sum wy^2$。每个分块累积这两个数，合并时分别相加，最后算 $1-E/D$；若 $D=0$，该统计量无定义，示例返回 None

```python
from math import isclose


def moments(rows):
    error = 0.0
    baseline = 0.0
    for y, pred, weight in rows:
        error += weight * (y - pred) ** 2
        baseline += weight * y ** 2
    return error, baseline


def pooled_score(parts):
    error = 0.0
    baseline = 0.0
    for block_error, block_baseline in parts:
        error += block_error
        baseline += block_baseline
    return 1 - error / baseline if baseline > 0 else None


a = [(2, 2, 1)]
b = [(1, 0, 1)]
assert isclose(pooled_score([moments(a), moments(b)]), 0.8)
assert pooled_score([moments(a + b)]) == pooled_score([moments(a), moments(b)])
assert pooled_score([moments([(0, 3, 1)])]) is None
assert pooled_score([moments([(1, 3, 1)])]) == -3
```

##### 不同分母的聚合

第一批单独得 1，第二批单独得 0，算术平均是 0.5；全局误差和为 1、分母为 5，总分为 0.8。一个 $D=0$ 的分块依然可能贡献预测误差，聚合时应保留它的 $E$。对 $n$ 条输入时间 $O(n)$，每个进程额外状态 $O(1)$。实际浮点并行合并顺序可能造成末位差异，可用容差比较，长序列精度要求高时可考虑分块精确求和。[Python 浮点求和](https://docs.python.org/3/library/math.html#math.fsum) · [比赛的评分公式](../../kaggle-competition/xhth/README.md)
