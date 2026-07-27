# 量化研究、交易框架与市场数据

> 重要：本页项目用于研究、数据工程、回测或执行基础设施，不构成投资建议。回测结果不等于实盘表现；接入真实经纪商或付费评估平台前，应完成权限隔离、交易成本模拟、数据许可核验和人工风控审批。

## A 股与跨市场数据

### A-Stock Data — A 股全栈 Agent 数据工具包

| 字段 | 信息 |
| --- | --- |
| 上游 | [simonlin1212/a-stock-data](https://github.com/simonlin1212/a-stock-data) |
| 许可证 | Apache-2.0 |
| 交付形态 | `SKILL.md`；面向 Agent 的数据工具包/工作流 |
| 收录时星标 | 8.0k |
| 上游最近推送 | 2026-07-26 |

**定位**：面向中国 A 股研究的多源数据 Skill。上游称其覆盖约十层能力、43 个端点（含官方备用源）和 15 个数据源：行情、研报、资金面、筹码、公告、涨停/炸板、ETF/期权、舆情与互动问答等。核心思路是主源不可用时按独立风控面切换备用源。

**适合**：让 AI Coding Agent 做小规模、可追溯的 A 股研究/信息收集；尤其适合需要把行情、公告、资金、板块、ETF/期权和主题热度放到同一调查流程的场景。

**能力示例**：个股/行业/概念行情、资金流、龙虎榜、融资融券、研报/公告、涨停池、ETF 和期权报价/Greeks、互动易问答、市场热度；其文档提供四套内置调研流程和降级策略。

**注意**：
- “零鉴权”不等于数据可无限商用；每一个上游数据源都有独立条款、频控与版权边界。
- 不要把网页抓取的盘口、公告或研报数据视为权威行情源；生产交易应使用合规授权的数据服务。
- 接到 Agent 后应限制批量请求、缓存结果、记录来源与时间戳，并将“查询/研究”与“下单”严格隔离。

### stock-api — 多市场行情的 TypeScript/CLI/MCP 工具

| 字段 | 信息 |
| --- | --- |
| 上游 | [zhangxiangliang/stock-api](https://github.com/zhangxiangliang/stock-api) |
| 项目页 | [GitHub Pages 文档](https://zhangxiangliang.github.io/stock-api/) |
| 许可证 | MIT |
| 语言 | TypeScript；Node.js >=18；浏览器、CLI、MCP |
| 收录时星标 | 1.8k |
| 上游最近推送 | 2026-07-27 |

**定位**：轻量、零运行时依赖的行情查询工具，覆盖 A 股、港股、美股与场内基金。默认 `stocks.auto` 按腾讯 → 新浪 → 东方财富进行自动兜底；可直接指定 provider。

**接入方式**：

```bash
# 临时 CLI 查询
npx stock-api get-stock SH510500
npx stock-api get-klines SH600519 --period day --count 120

# MCP（支持 MCP 的客户端）
npx -y stock-api mcp
```

其 MCP 包含 `get_stock`、`get_stocks`、`get_klines`、`search_stocks` 和 `inspect_stock` 等工具；也可让 Agent 阅读上游 `SKILL.md` 获得命令说明。

**何时采用**：网页/Node 项目需要简单行情展示；或希望快速给 MCP Agent 增加小范围的股票查询能力。

**限制**：第三方公开接口会失效、延迟或返回异常；不保证实时性、完整性或持续可用。不要将其用于高频、自动下单、结算或需要授权行情的生产用途。

## 研究与回测框架

### QuantSpace — AI-native 量化投研工程骨架

| 字段 | 信息 |
| --- | --- |
| 上游 | [quantskills/agent-quantspace](https://github.com/quantskills/agent-quantspace) |
| 许可证 | GPL-3.0 |
| 语言/环境 | Python >=3.10、uv、Parquet/DuckDB；可选 PandaData/PyCaret |
| 收录时星标 | 48 |
| 上游最近推送 | 2026-07-25 |

**定位**：给 AI Coding Agent 使用的量化研究项目结构。它把“想法→数据接入→本地数据仓→因子/分析→ML→回测→报告”约束成目录、数据契约与一组 `skills/`。目标不是替你做正确投资判断，而是让 Agent 不把研究代码散落成一次性脚本。

**架构**：`ingest` 获取/规范数据；`store` 管理 Parquet/DuckDB；`compute` 计算指标/标签；`analyze` 做因子诊断；`ml` 训练/预测；`backtest` 做向量化执行、组合、成本与风控覆盖；`report` 生成 HTML/Markdown 报告。策略按横截面和时间序列组织。

**适合**：新建一个有长期可维护目标的量化研究仓库，并希望 Codex/Claude Code/Cursor 等沿既定边界工作。

**接入建议**：先使用项目自带的合成 OHLCV fixture 跑通 tests 与 demo；确认数据契约后再引入真实数据。真实数据/账号写入 `.env` 或 Secret，永不提交。GPL-3.0 对下游分发约束较强，若计划闭源分发/嵌入商业产品，应先做许可证评估。

### Lumibot — Python 策略、回测与交易 Agent 运行框架

| 字段 | 信息 |
| --- | --- |
| 上游 | [Lumiwealth/lumibot](https://github.com/Lumiwealth/lumibot) |
| 文档 | [lumibot.lumiwealth.com](https://lumibot.lumiwealth.com/) |
| 许可证 | GPL-3.0（以仓库 API 元数据为准；采用前复核具体版本 LICENSE） |
| 语言 | Python |
| 收录时星标 | 1.9k |
| 上游最近推送 | 2026-07-16 |

**定位**：将规则策略、AI Agent、历史回测、模拟交易和经纪商执行放到同一个 Python 策略生命周期中。上游覆盖股票、期权、加密、期货、外汇、SEC 文件和 FRED 宏观数据，并提供 Alpaca 等 broker 的纸面/实盘衔接。

**AI Agent 设计**：可以把研究员、看多、看空、交易员/组合经理等角色放在一个策略内；Agent 读取市场、账户、订单、指标、文件、宏观或本地记忆，再由策略决定是否提交订单。规则门槛与 Agent 推理可组合。

**推荐路径**：
1. 只跑本地、固定数据的回测；保存输入、版本、交易成本、随机种子和结果工件。
2. 接入 paper trading，限制标的、仓位、频率、最大损失和订单类型。
3. 经过足够长时间的实盘外验证、监控和人工风控后，才单独评估是否启用实盘。

**高风险点**：LLM 幻觉、工具调用错误、数据延迟、滑点、经纪商 API 异常和提示词注入都可能转化为资金风险。AI Agent 默认应只读；下单应在确定性风控层、额度限制和人工确认之后。

### WorldQuant Miner — WorldQuant Alpha 生成/提交脚本集合

| 字段 | 信息 |
| --- | --- |
| 上游 | [zhutoutoutousan/worldquant-miner](https://github.com/zhutoutoutousan/worldquant-miner) |
| 官网 | [worldquant-miner.world](https://www.worldquant-miner.world/) |
| 许可证 | Apache-2.0 |
| 实现 | Rust、Python、TypeScript；含 Docker/Ollama 路径 |
| 收录时星标 | 716 |
| 上游最近推送 | 2026-02-22 |

**定位**：围绕 WorldQuant 平台的 alpha 表达式生成、筛选、提交与监控的社区脚本集合。仓库含多代实现；README 推荐本地 Ollama/GPU + Docker 的自动化路径，并含 Web Dashboard 与 24/7 调度描述。

**适合**：仅在你有合法 WorldQuant 账户、理解平台规则，并希望研究 alpha 想法管理/筛选流水线时评估。

**关键风险**：它需要平台凭据并涉及自动提交；不可把账号密码存入 `credential.txt` 后提交到仓库或分享。自动生成/提交可能违反平台规则、造成账户风险或浪费配额。先在隔离环境阅读代码与 `credential.example.txt`，使用最小权限账户，关闭自动提交，保留人工审核与速率限制。上游最后代码推送为 2026-02，采用前要检查依赖、平台 API 和安全更新。
