##### 量化数据工程与数值实现

一个结果能否复现，取决于数据在何时可见、行与资产如何对齐、跨批次保留了哪些状态，以及评分能否按相同口径合并。以下实现以小样本明确这些约定，再给出时间与空间复杂度

| 分区 | 内容 |
| --- | --- |
| 数据与事件时钟 | [点时连接](alignment/asof-alignment.md) · [数据版本与历史资产池](alignment/vintages-and-universe.md) · [流式滚动窗口](alignment/rolling-window.md) · [离线与实时回放](alignment/offline-replay.md) |
| 评估算子 | [分块加权决定系数](metrics/streaming-r2.md) · [并列秩 IC](metrics/rank-ic.md) · [净值回撤](metrics/drawdown.md) |
| 动态规划 | [含手续费的离线交易上界](optimization/trading-dp.md) |

例如：按资产更新历史均值之前，先决定当前观测是否计入；输出预测后，按资产键对齐标签与权重；最后合并各时间块的误差和与基准能量。这三个步骤中任何一个改变，都可能使相同参数的模型得到不同评分
