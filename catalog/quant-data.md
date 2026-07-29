# 量化研究、交易框架与市场数据

> **风险与边界**：本页项目用于研究、数据工程、回测、模拟交易或执行基础设施，不构成投资建议。回测、模型评分、AI 报告与作者展示的历史业绩都不等于未来表现。接入真实经纪商、付费数据、个人账户或评估平台前，应完成权限隔离、交易成本模拟、数据许可核验、额度控制和人工风控审批。

## 快速选型：四个高星项目如何分工

| 项目 | 解决的核心问题 | 最适合的阶段 | 不应误解为 |
| --- | --- | --- | --- |
| [daily_stock_analysis](#daily_stock_analysis--多市场-ai-日报与决策看板) | 多市场自选股的每日分析、通知与 Web 看板 | 每日信息汇总、候选池跟踪 | 可自动保证盈利的交易系统 |
| [Fincept Terminal](#fincept-terminal--原生金融研究终端) | 一体化金融终端、数据连接器与 AI/量化工作台 | 手动探索、建模、跨市场研究 | 可无条件商用的纯 MIT/Apache 软件 |
| [LEAN](#lean--quantconnect-事件驱动量化引擎) | 策略研究、回测、优化、paper/live trading | 可复现策略工程与交易生命周期 | 不用数据/经纪商配置即可稳定实盘的引擎 |
| [AI Berkshire](#ai-berkshire--claude-codecodex-价值投研-skill-框架) | 结构化价值投资研究与多 Agent 对抗式分析 | 单公司深度研究、财报/行业/组合框架 | 被验证的收益承诺或自动下单系统 |

一个相对安全的组合方式是：

```text
市场/基本面数据与每日候选池
  → daily_stock_analysis（日报、风险提示、通知）
  → Fincept Terminal（人工探索、可视化、数据/模型工作台）
  → AI Berkshire（对少量候选做结构化深度研究）
  → LEAN（把可形式化的假设做回测、成本检验、paper trading）
  → 人工投资决策 / 独立风控
```

不要让任一 LLM 报告直接触发交易；研究、回测、模拟与实盘必须分层。

## A 股与跨市场数据

### A-Stock Data — A 股全栈 Agent 数据工具包

| 字段 | 信息 |
| --- | --- |
| 上游 | [simonlin1212/a-stock-data](https://github.com/simonlin1212/a-stock-data) |
| 许可证 | MIT |
| 实现 | Python、FastAPI/MCP 风格接口 |
| 收录时星标 | 约 200 |

**定位**：面向 Agent 的 A 股数据接口集合。README 描述为 10 层架构、44 个端点、15 个数据源，并主张多数接口无需鉴权；覆盖行情、K线、资金流、北向、龙虎榜、研报、公告、财报、ETF、期权和舆情等。

**适合**：快速原型、研究工具的多源 fallback、为 Agent 提供受控的本地查询层。接入前应逐一核验每个数据源的时效、限流、使用条款、字段口径及“零鉴权”是否仍成立。

**工程边界**：公开上游接口不应被视为稳定的生产行情授权。生产研究应记录每次数据的来源、时间戳、复权方式和版本；关键字段要交叉验证，避免将演示级数据混入回测/实盘。

### stock-api — 轻量行情 CLI / Node API / MCP

| 字段 | 信息 |
| --- | --- |
| 上游 | [zhangxiangliang/stock-api](https://github.com/zhangxiangliang/stock-api) |
| 许可证 | MIT |
| 语言 | TypeScript / Node.js |
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

### daily_stock_analysis — 多市场 AI 日报与决策看板

| 字段 | 信息 |
| --- | --- |
| 上游 | [ZhuLinsen/daily_stock_analysis](https://github.com/ZhuLinsen/daily_stock_analysis) |
| 官网 | [dsa.zhulinsen.tech](https://dsa.zhulinsen.tech/) |
| 许可证 | MIT |
| 语言 | Python 3.10+ |
| 收录时星标 | 59,384 |
| 上游最近推送 | 2026-07-28 |

**定位**：一个以 LLM 为分析与解释层的自选股工作台。它面向 A 股、港股、美股、日股、韩股、台股和 ETF，聚合行情、K 线、技术指标、新闻、公告、基本面与辅助研究数据；每日输出评分、趋势、买卖点、风险提示、催化因素和操作检查清单，并能推送“决策仪表盘”。

**核心能力**：

- **多市场/多源行情**：免费源包括 AkShare、Baostock、YFinance 等；可选 Tushare、Longbridge、TickFlow 等 Token 型源提升稳定性。
- **AI 分析与 15 种策略入口**：均线、缠论、波浪、趋势、热点、事件、成长、预期等，通过 Web/Bot/API 进行追问与分析。
- **信息层**：可配置 SerpAPI、Tavily、Bocha、Brave、SearXNG 等搜索/新闻源；部分美股场景可接社交情绪源。
- **交付层**：Web/桌面工作台、历史报告、回测/持仓页面，以及企业微信、飞书、Telegram、Discord、Slack、邮件推送。
- **部署层**：GitHub Actions 定时、Docker、本地定时或 FastAPI 服务。上游提供 fork 后配置 Secrets、在工作日北京时间 18:00 运行的入门路径。

**与 Codex/Agent 的协作方式**：适合让 Agent 做配置审计、数据源 fallback 检查、报告质量抽检和部署脚本维护。不要让 Agent 直接把模型结论映射为自动下单；先把输出定位为“候选池、研究线索和风险提醒”。

**建议的安全部署方式**：

1. Fork 后先只使用 `--dry-run` 或少量公开标的，确认市场代码、时区、复权和推送格式。
2. 所有 Key 写入 GitHub Actions Secrets / `.env` / Secret 管理器，绝不提交到仓库、报告或截图。
3. 对每一种数据源记录可用市场、授权、延迟、限流与降级规则；免费源不作为关键交易判断的唯一依据。
4. 给通知机器人最小权限；不要在飞书/Slack 报告中回显 Secret、完整账户持仓或敏感 webhook。
5. GitHub Actions 适合低频日报，不等价于低延迟交易基础设施。

**限制与风险**：评分、买卖点和 LLM 解释均可能受数据缺失、提示词、模型变更、新闻噪声和幸存者偏差影响。README 的“零成本/5 分钟部署”描述的是运行便利性，而不是研究质量、实时性或收益保证。

## 研究终端与投研工作流

### Fincept Terminal — 原生金融研究终端

| 字段 | 信息 |
| --- | --- |
| 上游 | [Fincept-Corporation/FinceptTerminal](https://github.com/Fincept-Corporation/FinceptTerminal) |
| 官网 | [fincept.in](https://fincept.in/) |
| 许可证 | 仓库 API 未声明 SPDX；README 标注 AGPL-3.0，并另有商业许可证文档；采用/分发前须复核 [`LICENSE`](https://github.com/Fincept-Corporation/FinceptTerminal/blob/main/LICENSE) 与商业条款 |
| 技术栈 | C++20 + Qt6 原生 UI + 嵌入式 Python |
| 收录时星标 | 29,267 |
| 上游最近推送 | 2026-07-25 |
| 维护状态 | README 说明自 2026-06 起公开版改为约每月更新一次，团队重点转向订阅制私有版与 Quantcept；公开仓库仍保留 |

**定位**：一个桌面级、跨资产的金融研究与分析终端，目标类似可自定义的金融情报工作台，而不是单纯的行情看板。它强调原生性能、数据连接器、估值/风险模型、可视工作流和 AI 辅助研究。

**功能范围**：

- **分析与建模**：DCF、组合优化、VaR、Sharpe、衍生品定价、固定收益、波动率和随机过程等；README 称含 18 个 QuantLib 相关量化模块。
- **Agent/LLM**：37 个 Trader/Investor、宏观与地缘政治风格 Agent；支持 OpenAI、Anthropic、Gemini、Groq、DeepSeek、MiniMax、OpenRouter、Ollama 等本地/云模型。
- **数据接入**：README 称 100+ 连接器，列举 DBnomics、Polygon、Kraken、Yahoo Finance、FRED、IMF、World Bank、AkShare 与政府 API 等。具体可用性、费用、速率和许可取决于各数据商。
- **交易/模拟**：覆盖 crypto WebSocket、paper trading、算法交易和多家 broker 集成。README 中的 broker 覆盖不代表所有地区、账户或版本均可用。
- **扩展研究**：海事跟踪、地缘分析、关系图谱、卫星数据、节点编辑器、MCP 工具集成、ML/因子/强化学习等。

**适合谁**：希望在一台桌面机器上把数据探索、财务模型、宏观研究、风险分析和 AI 辅助放到同一 UI 的高级个人研究者。若你只需要稳定 A 股数据/日报，先用现有 QuantDB/Market Breadth 或 daily_stock_analysis，避免为“全功能终端”承担大量数据商和配置复杂度。

**接入建议**：

1. 在隔离环境安装并只连接无敏感数据的公开/试用数据源。
2. 先关闭实盘凭据与自动交易，使用历史数据和 paper account 验证字段、币种、时区、复权和费用模型。
3. 对每个外部 API 单独创建最小权限 Key，设置额度/告警；不要共用银行、券商或主账号密钥。
4. 在接入 Agent/MCP 前先核对其工具权限，尤其避免读取本地敏感文件、自动执行脚本或直接下单。
5. 若改造、内嵌、对外分发或商用，先处理 AGPL/商业许可问题；不要只看 GitHub “开源”标签。

**风险**：这是庞大、连接器众多的终端。每增加一个数据源、Agent、插件或 broker 都增加供应链、凭据和攻击面。其公开维护频率下降也是选型因素；建议锁定版本、审查 release、保留可回退配置。

### AI Berkshire — Claude Code/Codex 价值投研 Skill 框架

| 字段 | 信息 |
| --- | --- |
| 上游 | [xbtlin/ai-berkshire](https://github.com/xbtlin/ai-berkshire) |
| 许可证 | MIT |
| 语言/目标运行时 | Markdown Skills + Python 工具；Claude Code、Codex |
| 收录时星标 | 14,566 |
| 上游最近推送 | 2026-07-26 |

**定位**：将价值投资研究流程做成可调用的 Skill 套件，而不是一个行情终端或自动交易机器人。它把巴菲特、芒格、段永平、李录等研究视角拆成结构化模板、工具与多 Agent 对抗式分析流程，目标是让单人使用 Claude Code/Codex 时获得更一致、可复查的公司研究产物。

**架构**：

```text
Skill 层：选择研究入口与固定产出结构
→ Agent 层：团队型任务由 Team Lead 组织多个独立研究视角
→ Tool 层：精确计算、实时检索、报告/数据抽检
→ 人工研究者：核验来源、判断证据质量、决定是否行动
```

**上游主要入口**：

- **深度研究**：`/investment-research`、`/investment-team`、管理层纵深、未上市公司研究、长篇公司研究系列。
- **财报研究**：`/earnings-review`、`/earnings-team`，强调优先阅读原始财报。
- **行业/筛选**：产业链扫描、行业漏斗、质量筛选、供应链瓶颈与买入前 checklist。
- **组合管理**：组合复盘、投资论点追踪、论点漂移检测、异动新闻归因。
- **方法与数据严谨性**：要求关键数据交叉核验、使用 `decimal.Decimal` 做关键金融计算等。

**与 Codex 的实际使用建议**：

1. 不要把整个项目盲目加入全局指令；先挑一个 Skill，以一个已公开披露、资料充足的公司做演练。
2. 给每次研究设定明确输入：交易所/代码、研究日期、币种、允许来源、报告语言、最大成本与时限。
3. 要求每个关键数字附原始来源、报告日期、单位、币种与计算式；对市值、每股指标、汇率和复权单独交叉验证。
4. 保留研究版本、来源快照、模型/提示词版本和未解决问题；把“未知”作为合法结论，而不是让模型补全。
5. 多 Agent 的价值在独立证据和相互挑战，不在于把同一个提示词重复跑四遍。用户的 Codex 环境若限制子 Agent，应只使用无需并行 Agent 的 Skill，或由用户明确授权并行研究。

**关于作者展示的业绩**：README 包含作者个人账户历史收益展示及“历史收益不代表未来表现”的免责声明。此类信息不能视作可独立验证的策略回测、收益承诺或任何人可复现的结果；录入时仅将其视为作者陈述。

**风险**：LLM 对财务单位、日期、口径和引用可能出错，且多 Agent 会提高 API 成本和信息噪声。禁止把报告直接变成下单指令；研究输出应经过资料核验、估值假设审计、组合风险控制和人工决策。

## 研究与回测框架

### TA-Lib Python — 成熟的技术指标与 K 线形态识别库

| 字段 | 信息 |
| --- | --- |
| 上游 | [TA-Lib/ta-lib-python](https://github.com/TA-Lib/ta-lib-python) |
| 官方文档 | [ta-lib.github.io/ta-lib-python](https://ta-lib.github.io/ta-lib-python/) · [PyPI：TA-Lib](https://pypi.org/project/TA-Lib/) |
| 许可证 | BSD-2-Clause |
| 语言/依赖 | Python >= 3.9；Cython、NumPy；封装底层 TA-Lib C 库 |
| 收录时版本 | v0.7.1（PyPI `TA-Lib` 0.7.1） |
| 收录时星标 | 约 12.2k |
| 上游最近发布 | 2026-07-16（v0.7.1） |

**定位**：TA-Lib 的高性能 Python 绑定，是量化研究和技术分析中常用的“指标计算层”，不是行情源、回测引擎、策略框架或交易系统。它以 Cython 而非旧版 SWIG 绑定底层库；上游称相对旧绑定可获得更好的效率，并支持 `numpy.ndarray`、Pandas `Series` 与 Polars `Series`。

**核心能力**：

- 提供 150+ 技术分析函数，包括 SMA/EMA、MACD、RSI、ADX、ATR、布林带、Stochastic、成交量及价格变换等；不同函数有各自的 warm-up/lookback 要求。
- 支持 K 线形态识别（candlestick pattern recognition）；形态命中是规则结果，不代表预测结论或交易建议。
- 同时提供直接函数 API 和抽象 API，适合在数据清洗、特征工程、选股研究、回测前指标计算或研究报告中调用。
- 当前发行版提供 macOS（含 Apple Silicon）、Linux、Windows 等平台的二进制 wheel；大多数常见环境可以直接 `python -m pip install TA-Lib`。当 wheel 不可用时，需先安装底层 TA-Lib C 库；macOS 可用 `brew install ta-lib`，并按需设置 `TA_INCLUDE_PATH` / `TA_LIBRARY_PATH`。

**接入示例**：

```bash
python -m pip install TA-Lib pandas
```

```python
import talib

# close 应是按时间升序、经过复权/口径确认的价格数组或 Series
rsi_14 = talib.RSI(close, timeperiod=14)
macd, signal, histogram = talib.MACD(
    close, fastperiod=12, slowperiod=26, signalperiod=9
)
upper, middle, lower = talib.BBANDS(close, timeperiod=20)
```

**适合**：已经有合法行情数据、希望稳定且快速计算经典技术指标的 Python 研究项目；可作为 LEAN、Lumibot、pandas/Polars 研究管线或自建 Agent 工具的底层特征计算组件。若要用 Agent 调用，建议只暴露经过白名单约束的指标、标的、日期区间与数据集，而不是让模型任意执行 Python 或读取本地账户配置。

**工程与数据边界**：

1. **输入先于指标**：库不提供市场数据。必须单独处理数据供应商许可、复权、时区、停牌、缺失值、币种与延迟；同一个指标在不同数据口径下可得不同结果。
2. **NaN 与预热期**：每个指标都可能在开头输出 NaN；底层 TA-Lib 对中间 NaN 通常会向后传播。不能直接把 NaN、未预热指标或不同时间粒度的数据送进策略判断。
3. **指标不是信号**：RSI/MACD/K 线形态是历史价格变换，不能单独证明可交易性。任何规则都应做样本外验证，并纳入费用、滑点、流动性、停牌和幸存者偏差。
4. **安装与供应链**：优先使用官方 PyPI wheel 或固定版本；源码构建依赖底层 C 库与编译环境，部署时要记录 `TA-Lib`、Python、NumPy 和操作系统版本。不要从非官方二进制下载站引入未核验 wheel。
5. **许可**：Python 绑定采用 BSD-2-Clause，便于集成；但底层 TA-Lib C 库、行情数据商、经纪商接口及项目自身代码各有独立条款，商业分发前应逐项复核。

### LEAN — QuantConnect 事件驱动量化引擎

| 字段 | 信息 |
| --- | --- |
| 上游 | [QuantConnect/Lean](https://github.com/QuantConnect/Lean) |
| 官网/文档 | [lean.io](https://lean.io/) · [QuantConnect 文档](https://www.quantconnect.com/docs/) |
| 许可证 | Apache-2.0 |
| 核心语言 | C#；策略支持 Python 与 C# |
| 收录时星标 | 20,900 |
| 上游最近推送 | 2026-07-28 |

**定位**：专业级事件驱动算法交易引擎，覆盖从研究、算法开发、历史回测、参数优化到 paper/live trading 的完整策略生命周期。它不是“提供交易观点”的模型，而是把你明确写出的策略按数据、时间、订单、手续费、滑点、经纪商和风控规则进行可复现执行的基础设施。

**核心组件**：

- **算法引擎**：事件驱动数据处理、证券/订阅管理、指标、订单状态、组合和风险模型；组件大多可替换或扩展。
- **策略语言**：Python 与 C#。研究原型可用 Python；性能敏感或需要更深引擎集成时可考虑 C#。
- **LEAN CLI**：`lean project-create`、`lean research`、`lean backtest`、`lean optimize`、`lean live` 等命令，将本地 Docker、研究环境和 QuantConnect 工作流串联。
- **数据/经纪商层**：支持多资产、另类数据与实盘 broker，但具体可用数据集、账单、地区限制和 broker 配置需单独核验。

**安全的学习路径**：

```bash
pip install lean
lean project-create
lean research       # 本地研究环境，通常依赖 Docker
lean backtest       # 固定版本数据与成本模型
lean optimize       # 注意过拟合与样本外验证
# → paper trading → 足够长的监控与人工审批 → 才评估 live
```

**与 Codex 的协作方式**：让 Codex 协助生成算法骨架、测试、数据校验、统计报告和配置审查，但将策略规则、数据许可、订单权限和 live brokerage Secret 保持在人类审核下。所有改动必须运行回测与样本外检验；不要把“通过编译”误认为策略有效。

**常见误区**：

- 回测不含真实滑点、成交容量、停牌、数据修订和 survivorship bias 时，表现会虚高。
- 参数优化若反复看同一段历史，容易数据窥探；必须划分训练/验证/样本外周期。
- `lean live` 是真实交易能力，不应在未设置 broker 权限、最大仓位、日损失、熔断、告警和人工停止开关前启用。
- Apache-2.0 使引擎代码的再利用较灵活，但市场数据、broker API 和 QuantConnect 云服务条款是独立约束。

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
