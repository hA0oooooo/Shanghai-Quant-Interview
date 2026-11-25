##### 组合风险与交易执行

预测、组合和成交分别回答三个问题：哪些资产相对有吸引力，资金实际暴露于哪些风险，在给定的流动性下能实现多少收益。一个较高的截面 IC 只能回答其中的第一部分

| 层次 | 方法 |
| --- | --- |
| 风险模型 | [因子暴露与风险贡献](risk-models/exposure-model.md) · [协方差收缩](risk-models/covariance-shrinkage.md) |
| 持仓与目标 | [约束、换手与成本](construction/allocation-and-turnover.md) · [指数增强与对冲](construction/benchmark-and-hedge.md) · [策略分散与增量](construction/strategy-diversification.md) |
| 交易执行 | [实现缺口与容量](execution/shortfall-and-capacity.md) · [有效价差与价格冲击](../price-volume-factors/microstructure/spread-decomposition.md) |

一条完整的研究链可以从[截面因子](../price-volume-factors/factor-evaluation/incremental-alpha.md)开始，先用风险模型估计组合敞口，再根据成本把目标仓位与实际可成交仓位对齐。收益归因时仍需使用实际的持仓路径
