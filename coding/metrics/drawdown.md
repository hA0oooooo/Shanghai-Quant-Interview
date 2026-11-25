##### 净值、峰谷与最大回撤

设账户初始净值为 1，输入一串按时间升序、已扣成本的日简单收益，取值有限且不低于 $-1$。返回最大回撤的正幅度，以及造成它的峰、谷日编号。交易前为第 0 日，首项收益结束时为第 1 日。约定相同高点保留最早日，相同回撤保留首次；全程上涨或空输入返回 $(0,0,0)$

若前两天先涨 50% 再跌 20%，净值走为 $1\rightarrow1.5\rightarrow1.2$，相对起点仍有盈利，但第二日相对历史高点回撤 20%。枚举所有峰谷对需要 $O(n^2)$，对给定谷底只需此前最高净值即可

```python
from math import isclose


def maximum_drawdown(returns):
    equity = peak = 1.0
    peak_day = 0
    worst, start, end = 0.0, 0, 0
    for day, daily_return in enumerate(returns, start=1):
        equity *= 1 + daily_return
        if equity > peak:
            peak, peak_day = equity, day
        drawdown = 1 - equity / peak
        if drawdown > worst:
            worst, start, end = drawdown, peak_day, day
    return worst, start, end


dd, peak, trough = maximum_drawdown([0.5, -0.2, 0.1])
assert isclose(dd, 0.2) and (peak, trough) == (1, 2)
assert maximum_drawdown([0.1, 0.2]) == (0.0, 0, 0)
assert maximum_drawdown([-1.0]) == (1.0, 0, 1)
assert maximum_drawdown([]) == (0.0, 0, 0)
```

扫描时间 $O(n)$，额外空间 $O(1)$。若只给“最大净值减最小净值”，最小值可能早于最大值，还会混淆金额和百分比。回撤修复日是谷底之后净值首次回到原峰值的日期，样本结束时仍可能未恢复。[Python enumerate](https://docs.python.org/3/library/functions.html#enumerate)
