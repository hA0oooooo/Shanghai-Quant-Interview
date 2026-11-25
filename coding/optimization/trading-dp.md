##### 固定手续费下的离线交易动态规划

给定完整的 $n$ 天全部价格，每天可买或卖一股，最多持有一股，卖出付固定非负手续费，最终要求空仓，允许多次交易。输出最优利润。全序列在手，这里求的是事后最优上界，不能据此宣称实现了可在线交易的策略

收盘状态只有两种：$C_t$ 是第 $t$ 天动作后空仓的最大累计现金，$H_t$ 是持有一股时剩余的最大累计现金。买入发生在前一天空仓的路径，卖出发生在前一天持仓的路径。设今日价格为 $p_t$、手续费为 $f$，则

$$
C_t=\max(C_{t-1},H_{t-1}+p_t-f),\qquad H_t=\max(H_{t-1},C_{t-1}-p_t)
$$

```python
def best_cash(prices, fee):
    cash, holding = 0, float("-inf")
    for price in prices:
        cash, holding = (max(cash, holding + price - fee),
                         max(holding, cash - price))
    return cash


assert best_cash([1, 3, 2, 8, 4, 9], 2) == 8
assert best_cash([5, 4, 3], 1) == 0
assert best_cash([3], 2) == 0
assert best_cash([], 2) == 0
```

初始持仓现金为负无穷，排除“开场先卖一股”的非法路径。Python 的同时赋值先评估右侧，因此两项转移读的都是昨日状态。最优示例在 1 买、8 卖，得到 5，再在 4 买、9 卖，得到 3，共 8。时间 $O(n)$，额外空间 $O(1)$。若需还原交易日期，则应保留前驱状态，可以保存各天的最优转移；若有冷静期，则买入转移还要跟踪允许重新开仓的最早日期。[Python 赋值求值顺序](https://docs.python.org/3/reference/expressions.html#evaluation-order)
