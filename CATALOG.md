# 目录

> 数据快照：2026-08-05。项目活跃度、星标、版本、许可证和 API 兼容性会变化，请在采用前核验上游文档与条款。

| 分类 | 条目 | 适用场景 |
| --- | --- | --- |
| [金融 AI](catalog/financial-ai.md) | Kronos、FinRL | K线/量价序列预测、金融时间序列研究、金融强化学习实验与回测原型 |
| [量化研究与市场数据](catalog/quant-data.md) | A-Stock Data、stock-api、TA-Lib Python、PineTS、LLMQuant Hermes、QuantSpace、Lumibot、WorldQuant Miner、Vibe-Trading、QuestDB TSP（tick-stock-panel）、| A 股数据、MCP 行情、技术指标、AI 投研、研究工程、回测与受控交易连接器 |
| [MCP、API 与 Agent 工具桥接](catalog/mcp-api-tools.md) | mcp2cli、AOCI-CODE、Agent Lightning | 将受信任的 MCP、OpenAPI、GraphQL 发现并受限地转为 CLI/Agent 工具、Agent 训练基础设施 |
| [Agent 浏览器](catalog/agent-browser.md) | ego lite | Agent 浏览器自动化、复用本机登录态 |
| [浏览器、网页 Agent 与自动化](catalog/browser-web-agent.md) | GitReverse、CloakBrowser、Page Agent、scroll-world、UI-TARS | 仓库/网站反向 Prompt、自家网页 Copilot、浏览器运行时、沉浸式品牌页 Skill、视觉 GUI Agent |
| [网页采集](catalog/web-scraping.md) | Scrapling | 动态站点、反爬场景、自适应选择器、规模化采集 |
| [视觉 AI、CV 与办公](catalog/vision-office.md) | Qwen 多角度 3D Camera、Supervision、Larkboat、patent-disclosure-skill、AI剪口播 | 图像多视角、检测/分割后处理、文档/表格/PPT 办公、专利技术文档、口播转录与剪辑工程 |
| [设计资源](catalog/design-resources.md) | Lucide、Phosphor、Tabler、Radix、Uiverse Galaxy、Emil Kowalski Skills、Oil UI、IP as Logo | 产品界面、原型、前端实现、UI 动效和 Agent 设计决策参考、吉祥物候选图 |
| [资源目录与邮箱](catalog/resources-email.md) | FMHY、Public APIs、邮箱助手 | 资源发现、API 候选、邮箱服务导航 |

## 快速选择

- 想做市场 OHLCV/K线预测或微调金融序列模型：看 **Kronos**。
- 想以 Gym 风格环境比较 A2C、DDPG、PPO、SAC、TD3 等 DRL 策略，或复现教学/研究型 train-test-trade 流程：看 **FinRL**。它是研究框架；新建生产化交易系统应进一步评估上游推荐的 **FinRL-X / FinRL-Trading**，并独立完成风控、合规和实盘验证。
- 想快速给 Agent 增加 A/港/美股查询：看 **stock-api**；想进行多源 A 股研究：看 **A-Stock Data**。
- 想在 Python/Pandas/Polars 管线中计算 RSI、MACD、布林带、ADX 或 K 线形态：看 **TA-Lib Python**；它只做指标计算，仍需自备合规行情、完成验证和风控。
- 想搭建可维护的 AI 量化研究仓库：看 **QuantSpace**；想把自然语言投研、跨市场回测、MCP/Swarm 和连接器放进一个研究工作台：看 **Vibe-Trading**；想做 Python 回测或 paper/live broker 策略：看 **Lumibot**。
- 已部署 Hermes，想将市场数据查询、晨报、财报/13F/持仓观察做成可控的 MCP + 定时工作流：看 **LLMQuant Hermes**；它需要独立的 LLMQuant Data 凭据、预算控制与人工风控。
- 想把受信任的 MCP、OpenAPI 或 GraphQL 服务转换为可筛选、可脚本化的 CLI，降低 Agent 工具 schema 负担：看 **mcp2cli**。先 `--list` 审查，再用只读白名单、最小 OAuth scope 与隔离 Secret；不要把它当作生产权限/审批系统。
- 想让 Codex、Claude Code 等 Agent 在不抢占日常标签页的前提下完成浏览器任务：看 **ego lite**。
- 想快速把公开 GitHub 仓库压缩成供 Coding Agent 参考的合成 Prompt：看 **GitReverse**；它只读取有限公开上下文，不能替代源码、许可证和安全审查。
- 想把自然语言 GUI 助手嵌入自己的网站：看 **Page Agent**。有明确授权的自动化/检测测试需要时再评估 **CloakBrowser**。
- 想写 Python 采集脚本，且目标站点可能改版或有反自动化限制：看 **Scrapling**。
- 想统一产品图标风格：通常先看 **Lucide** 或 **Tabler**；需要字重变化看 **Phosphor**；需要紧凑栅格看 **Radix Icons**；需要现成 CSS/Tailwind 灵感看 **Uiverse Galaxy**。
- 想让 Agent 做更高质量的 UI 动效决策、审查或原型变体：看 **Emil Kowalski Skills**；按任务选择 `animate`、`review-animations`、`improve-animations` 或 `prototype`，并在真实设备与 reduced-motion 场景验收。

- 想做模型无关的 CV 后处理、标注、跟踪和区域计数：看 **Supervision**。
- 想发现公共 API、免费资源或邮箱服务：看 **Public APIs / FMHY / 邮箱助手**，但所有外链都必须单独核验。

- 想将研发材料整理为中国专利交底/申请底稿，或建立私有专利解读与地图知识库：看 [**patent-disclosure-skill（中国专利.skill）**](catalog/vision-office.md)。先审核技能与数据外发边界；工具不能替代专业代理审核、全面查新或官方申请，未公开技术须授权和脱敏。

- 想围绕产品探索差异明显的视觉方向、并排比较原型、截图还原并以真实画面迭代：看 [**Oil UI**](catalog/design-resources.md)。它是设计 Skill，不是组件库；区分 MIT 开源版与付费 Pro，审查更新检查、素材外发和宿主权限，不替代业务正确性与无障碍测试。

- 想存储持续流入的 tick、订单簿、成交或遥测事件，并用时间序列 SQL 做实时聚合与 ASOF 对齐：看 [**QuestDB**](catalog/quant-data.md)。它不是数据供应商或交易系统；区分 Apache-2.0 开源版和 Enterprise 的高可用、安全与自动分层能力，先验证数据契约、权限、负载及备份恢复。

- 想让 Agent 跨会话复用 Git 版本化的代码/schema 语义索引，并治理范围、漂移和索引更新：看 [**AOCI-CODE**](catalog/mcp-api-tools.md)。当前为 RC、FSL-1.1-MIT 源码可见而非 OSI 开源；商业竞争性用途受限，各版本提供满两年后另授 MIT。索引不是源码或测试的替代，宿主模型仍可能外发源码。

- 想把有权使用的 Pine Script 指标迁移到 Node.js/浏览器，用自有 OHLCV 做扫描和图表计算：看 [**PineTS**](catalog/quant-data.md)。它不等于 TradingView 完整环境；原生脚本支持仍需逐 bar 验证，AGPL-3.0/商业双许可须在闭源或 SaaS 接入前评估。

- 想自托管 A 股选股、因子/策略回测、盘中监控与带确认卡的数据问答：看 [**TSP（tick-stock-panel）**](catalog/quant-data.md)。先用只读 MCP scope 和隔离数据评估；注意 Compose 的全接口监听及 Codex 凭据挂载，不将研究工作台当作真实交易或收益保证系统。

- 想了解 **Agent Lightning**：看 [Agent Lightning](catalog/mcp-api-tools.md)。先明确训练奖励、GPU 预算与隔离执行权限，v1.0 不沿用旧版接口。

- 想了解 **IP as Logo**：看 [IP as Logo](catalog/design-resources.md)。先确认生成数量、模型费用和品牌权利，不保证生成资产可注册商标。

- 想了解 **UI-TARS**：看 [UI-TARS](catalog/browser-web-agent.md)。先在授权测试设备验证动作和坐标，敏感操作人工批准；模型、客户端和权重许可分别核验。

- 想了解 **AI剪口播**：看 [AI剪口播](catalog/vision-office.md)。音频会上传火山引擎，需人工审核切点；AGPL 与云转录条款分别评估。
