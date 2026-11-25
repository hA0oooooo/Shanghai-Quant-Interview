##### Shanghai-Quant-interview｜量化研究知识库

从价格、成交、公告和资产特征出发，量化研究需要依次解决四个问题：在决策时点能观察到什么，模型试图预测什么，时间外证据能支持多强的结论，信号在风险与成本约束下能转化为多少收益。每个专题都有独立入口，概念、实现、竞赛实证与论文通过相对链接相连

##### 专题地图

| 专题 | 内容 |
| --- | --- |
| [量价因子与市场微观结构](price-volume-factors/README.md) | 截面与时序信号、日内量比、订单簿与流动性、期货期限结构、因子增量 |
| [概率与金融计量](probability-statistics/README.md) | 条件概率、平稳性与协整、条件异方差、事件研究、多重检验与相关样本推断 |
| [机器学习](machine-learning/README.md) | 回归损失、正则化、树模型、时间验证、特征增量与预测校准 |
| [深度学习](deep-learning/README.md) | 时间与资产两个维度上的表征、多任务监督、序列模型、优化、语言模型与金融文本 |
| [组合风险与交易执行](portfolio-risk/README.md) | 因子协方差、风险收缩、指数增强、中性对冲、换手、实现缺口与容量 |
| [数据工程与数值实现](coding/README.md) | 历史资产池、修订数据、点时连接、滚动状态、离线回放与分块归约 |
| [量化竞赛研究](kaggle-competition/README.md) | 匿名面板、收盘集合竞价、波动率、延迟标签、财报事件等公开任务的比较 |
| [论文专题](papers/README.md) | ICML、ICLR 的金融研究与金融期刊实证；由目标、方法和对照理解结果 |

##### 三条贯穿各分区的知识线

**从量价信号到组合**　同一笔成交既能进入价格动量，也能成为流动性成本的一部分。由[订单簿与订单流](price-volume-factors/microstructure/order-book.md)理解数据，再用[截面中性化](price-volume-factors/factor-evaluation/incremental-alpha.md)判断信号是否重复暴露，最后用[组合与换手](portfolio-risk/construction/allocation-and-turnover.md)评估净贡献

**从数据可得性到时间外验证**　先区分报告期、披露时点和数据修订版本，再决定特征、训练标签和预测目标各占用哪个时间区间。即使没有特征前视，反复筛选候选仍会使验证结果偏乐观。这条线连接[数据版本](coding/alignment/vintages-and-universe.md)、[模型验证](machine-learning/README.md)和[多重检验](probability-statistics/inference/selection.md)

**从共享表征到金融实证**　跨资产网络可以输出共享表示，经济意义要结合目标和约束判断。预测下一期收益、重建当期协动、控制组合风险和解释价格事件对应不同实验。可串读[深度学习](deep-learning/README.md)、[因子风险模型](portfolio-risk/risk-models/exposure-model.md)、[财报事件研究](probability-statistics/inference/event-study.md)与[论文精读](papers/README.md)

维护方式见 [AGENTS.md](AGENTS.md)
