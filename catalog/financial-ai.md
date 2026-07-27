# 金融 AI

## Kronos — 金融市场语言基础模型

| 字段 | 信息 |
| --- | --- |
| 上游 | [shiyu-coder/Kronos](https://github.com/shiyu-coder/Kronos) |
| 文档/中文介绍 | [Kronos 中文文档](https://www.zdoc.app/zh/shiyu-coder/Kronos) |
| 模型权重 | [NeoQuasar on Hugging Face](https://huggingface.co/NeoQuasar) |
| 论文 | [arXiv:2508.02739](https://arxiv.org/abs/2508.02739) |
| 许可证 | MIT |
| 技术形态 | Python、decoder-only Transformer、OHLCV/K线 tokenizer |
| 收录时星标 | 34.5k |
| 上游最近推送 | 2026-04-13 |

### 是什么

Kronos 是针对金融 K 线（OHLCV）序列预训练的开源基础模型家族。它先将连续的多维 K 线数据量化为分层离散 token，再以自回归 Transformer 建模序列；目标是服务预测、补全和下游量化研究等任务。上游说明其训练数据覆盖 45+ 全球交易所。

### 适合什么

- 对单一或多个交易标的做 OHLCV 时序预测实验。
- 在自己的市场数据上做微调、比较或特征研究。
- 将通用时间序列模型与金融专用预训练模型做基准对比。
- 研究框架原型；不应直接视作可上线的交易决策系统。

### 关键资产

上游模型表中包含轻量到更大规模的权重，例如 `Kronos-mini`（4.1M）与 `Kronos-small`（24.7M），并提供 tokenizer 和 Hugging Face 权重。具体模型、上下文长度及输入格式以其 README 和模型卡为准。

### 接入建议

1. 先用独立、可复现的研究环境加载最小模型和官方示例。
2. 明确数据频率、复权/未复权、时区、停牌和缺失数据处理；这些往往比换模型更影响结果。
3. 采用按时间滚动的 train/validation/test 切分，防止未来信息泄漏。
4. 用交易成本、滑点、换手和回撤评估策略层；预测误差低不等于可交易。

### 注意事项

- 这是研究/建模工具，不构成投资建议。
- 仓库虽在 2026-07 仍有社区活动，但其收录时最后代码推送为 2026-04；采用时应检查 Issue、权重可用性和依赖兼容性。
- 模型和代码均需分别遵守上游许可与 Hugging Face 模型卡条款。
