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

---

## FinRL — 金融强化学习研究框架

| 字段 | 信息 |
| --- | --- |
| 官方上游 | [AI4Finance-Foundation/FinRL](https://github.com/AI4Finance-Foundation/FinRL) |
| 官方文档 | [FinRL Docs](https://finrl.readthedocs.io/) |
| PyPI | [FinRL](https://pypi.org/project/FinRL/) |
| 许可证 | MIT（仓库 `LICENSE`） |
| 技术形态 | Python；Gym 风格市场环境、数据处理、DRL Agent 与 train-test-trade 工作流 |
| 收录快照 | 2026-08-05：15,919 stars、3,452 forks；未归档 |
| 上游维护快照 | 默认分支 `master`；最近代码提交 2026-07-12（[`2334a5f`](https://github.com/AI4Finance-Foundation/FinRL/commit/2334a5fe6d30629157f13c3b0319e1637e15e123)）；最新 GitHub Release 为 [v0.3.8](https://github.com/AI4Finance-Foundation/FinRL/releases/tag/v0.3.8)，发布于 2026-03-20 |

### 是什么

FinRL 是 AI4Finance Foundation 维护的开源金融深度强化学习（DRL）框架，面向教学、基准评测和研究原型。它将市场数据处理、Gym 风格交易环境、DRL Agent 和 train-test-trade 流程串联起来；上游示例覆盖股票、组合配置、加密资产与高频等研究任务，并集成 A2C、DDPG、PPO、SAC、TD3 等常见算法层。

上游已明确将此仓库定位为**经典、端到端的 FinRL**；若目标是新的生产型量化交易架构、实盘部署或多账户风控，应优先评估其后续项目 [FinRL-X / FinRL-Trading](https://github.com/AI4Finance-Foundation/FinRL-Trading)，而不是把 FinRL 直接当作生产交易系统。

### 适合什么

- 将强化学习交易策略作为可复现实验：统一数据、环境、训练、测试与回测流程。
- 对不同 DRL 算法、资产池、状态特征或奖励函数做时间序列外样本比较。
- 作为课程、论文复现和量化研究原型的参考实现；结合自己的合规数据源重建实验。
- 需要研究中国市场时，可评估其数据处理器所列的 AkShare、Baostock、JoinQuant、Tushare 等来源，但数据可用性与授权应逐一核验。

### 接入建议

1. 使用隔离 Python 环境，先按官方方式安装：`python3 -m venv .venv`、激活后 `pip install -e .`；固定依赖版本并记录 Python、数据源和随机种子。
2. 从官方小型示例开始，明确标的、频率、时区、复权、幸存者偏差、停牌与缺失值规则。不要将数据下载成功误当成数据可用于回测。
3. 以严格时间顺序切分 train/validation/test，并使用 walk-forward 或滚动窗口；所有归一化、特征选择与调参均只能使用当时可得的信息。
4. 评估应纳入手续费、滑点、冲击成本、借券/融资约束、成交容量、换手、最大回撤和极端行情；与买入持有等基线对照。
5. 若接入券商、交易所或付费数据 API，密钥放在环境变量/Secret 管理中，不写入代码或本目录；先使用 paper trading 和人工审批闸门。

### 安全、合规与能力边界

- 该项目是研究软件，不构成投资建议，也不保证回测收益、模型泛化或实盘安全。强化学习尤其容易受到奖励设计、泄漏、过拟合和非平稳市场的影响。
- 数据源、经纪商和交易所各自有账户、地域、再分发、速率与商业使用条款；MIT 仅覆盖 FinRL 代码，并**不**授予第三方市场数据、模型、API 或品牌的使用权。
- 切勿把生产账户凭据、个人数据或交易决策权直接交给实验脚本。实盘前应建立资金/仓位/损失上限、订单预检、审计日志、熔断与人工复核。
- 上游 README 已推荐 FinRL-X 作为现代生产部署方向；即便采用后续项目，仍需自行完成安全测试、合规审查与风控验证。
