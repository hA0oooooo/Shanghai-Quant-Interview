##### 按资产维护滞后滚动统计量

输入为按时间顺序到达的资产观测。对每一行，输出同一资产此前最近 $k$ 次观测的均值，再接收当前值。窗口不满或包含缺失值便输出 None。输入允许不同资产交错，每个资产内部的时间严格递增，非缺失值为有限数

假设 A 的数列依次为 2、4、10，$k=2$。到 10 时用于决策的特征应为 3；若先把 10 入队才读均值，得到的是 7。这两种特征可分别适用于不同的决策时点，这里使用严格滞后定义。十个“观测步”也未必等于十分钟

```python
from collections import defaultdict, deque


def prior_means(events, k):
    if k <= 0:
        raise ValueError("k must be positive")
    queues = defaultdict(deque)
    totals = defaultdict(float)
    missing = defaultdict(int)
    for asset, value in events:
        window = queues[asset]
        ready = len(window) == k and missing[asset] == 0
        yield totals[asset] / k if ready else None
        if len(window) == k:
            old = window.popleft()
            if old is None:
                missing[asset] -= 1
            else:
                totals[asset] -= old
        window.append(value)
        if value is None:
            missing[asset] += 1
        else:
            totals[asset] += value


events = [("A", 2), ("B", 9), ("A", 4), ("A", 10),
          ("A", None), ("A", 1), ("A", 3), ("A", 6)]
assert list(prior_means(events, 2)) == [None, None, None, 3, 7, None, None, 2]
assert list(prior_means([], 1)) == []
assert list(prior_means([("A", 4), ("A", 8)], 1)) == [None, 4]
```

##### 更新成本与窗口语义

每行最多一次入队、一次出队，时间 $O(n)$，状态空间 $O(Ak)$，$A$ 为已见资产数。生成器逐行输出。若规则改为最近五分钟，要保留时间戳并反复弹出超时观测；若规则改为窗口内忽略缺失，分母应改为有效观测数。高精度的大规模浮点滚动累加还需关注长期消减误差。[Python 的 deque](https://docs.python.org/3/library/collections.html#collections.deque)
