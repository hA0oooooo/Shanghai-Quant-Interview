##### 并列排名下的截面 Rank IC

在每个时点记录多只资产的信号和之后实际实现的收益，按资产已对齐，每行两个有限数或 None。删除任何一边缺失的整对，再计算两列的平均并列名次及名次间的 Pearson 相关（Spearman 相关）。有效资产少于两只或任一列是常数时返回 None

信号 $(1,1,3)$ 的名次是 $(1.5,1.5,3)$，相同信号值得到相同名次。若收益名次是 $(1,2,3)$，两列名次减均值后分别为 $(-0.5,-0.5,1)$、$(-1,0,1)$，相关为 $1.5/\sqrt{3}\approx0.866$。常见的 $1-6\sum d_i^2/[n(n^2-1)]$ 简式需要无并列的条件，存在并列时直接计算两组平均名次间的 Pearson 相关

```python
from math import isclose, sqrt


def average_ranks(values):
    order = sorted(range(len(values)), key=values.__getitem__)
    ranks = [0.0] * len(values)
    lo = 0
    while lo < len(order):
        hi = lo + 1
        while hi < len(order) and values[order[hi]] == values[order[lo]]:
            hi += 1
        rank = (lo + 1 + hi) / 2
        for place in range(lo, hi):
            ranks[order[place]] = rank
        lo = hi
    return ranks


def rank_ic(rows):
    pairs = [(x, y) for x, y in rows if x is not None and y is not None]
    if len(pairs) < 2:
        return None
    x, y = zip(*pairs)
    mean = (len(pairs) + 1) / 2
    sx = [r - mean for r in average_ranks(x)]
    sy = [r - mean for r in average_ranks(y)]
    norm = sqrt(sum(r*r for r in sx) * sum(r*r for r in sy))
    return sum(a*b for a, b in zip(sx, sy)) / norm if norm else None


assert isclose(rank_ic([(1, 1), (1, 2), (3, 3)]), sqrt(3) / 2)
assert isclose(rank_ic([(3, -1), (2, 0), (1, 1), (None, 100)]), -1.0)
assert rank_ic([(2, 1), (2, 3)]) is None
assert rank_ic([]) is None
```

排序主导时间 $O(n\log n)$，额外空间 $O(n)$。若有多天数据，通常每个时点独立对同一截面的信号与未来收益评分，再汇总每日 IC 的均值和波动。直接把所有行堆叠算一个相关，会混进日期间的整体水平差异。名次评价不检查信号幅度，也没有代入交易成本。[pandas 的并列排名方法](https://pandas.pydata.org/docs/reference/api/pandas.Series.rank.html)
