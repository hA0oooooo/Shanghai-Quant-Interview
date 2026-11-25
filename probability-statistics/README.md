##### 概率与金融计量

研究信号有三种不同的不确定性：在已知信息下未来结果仍然随机，历史样本之间相互依赖，研究者还从大量候选里选出了少数结果。相应地，建模需要明确条件分布、时间结构和选择程序

| 分区 | 概念与应用 |
| --- | --- |
| 概率基础 | [条件概率与信息不对称](probability/conditioning.md) · [首次到达时间与状态递推](probability/stopping.md) |
| 时间序列 | [平稳性、协整与误差修正](time-series/stationarity-and-cointegration.md) · [条件方差与波动聚集](time-series/conditional-volatility.md) |
| 统计推断 | [相关样本的配对比较](inference/dependence.md) · [多重检验](inference/selection.md) · [事件研究](inference/event-study.md) |

对金融面板而言，资产数量、时间长度和彼此独立的市场状态数是不同尺度。统计显著性与经济显著性也需要分别检验：前者关心估计误差，后者还需考虑预测幅度、风险和成交成本
