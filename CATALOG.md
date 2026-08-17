# 目录

> 数据快照：2026-08-05。项目活跃度、星标、版本、许可证和 API 兼容性会变化，请在采用前核验上游文档与条款。

| 分类 | 条目 | 适用场景 |
| --- | --- | --- |
| [金融 AI](catalog/financial-ai.md) | Kronos、FinRL | K线/量价序列预测、金融时间序列研究、金融强化学习实验与回测原型 |
| [量化研究与市场数据](catalog/quant-data.md) | A-Stock Data、stock-api、TA-Lib Python、LLMQuant Hermes、QuantSpace、Lumibot、WorldQuant Miner、Vibe-Trading | A 股数据、MCP 行情、技术指标、AI 投研、研究工程、回测与受控交易连接器 |
| [MCP、API 与 Agent 工具桥接](catalog/mcp-api-tools.md) | mcp2cli | 将受信任的 MCP、OpenAPI、GraphQL 发现并受限地转为 CLI/Agent 工具 |
| [Agent 浏览器](catalog/agent-browser.md) | ego lite | Agent 浏览器自动化、复用本机登录态 |
| [浏览器、网页 Agent 与自动化](catalog/browser-web-agent.md) | Agent QA、GitReverse、CloakBrowser、Page Agent、scroll-world | Agent 驱动 QA、仓库/网站反向 Prompt、自家网页 Copilot、浏览器运行时、沉浸式品牌页 Skill |
| [网页采集](catalog/web-scraping.md) | Scrapling | 动态站点、反爬场景、自适应选择器、规模化采集 |
| [视觉 AI、CV 与办公](catalog/vision-office.md) | Qwen 多角度 3D Camera、Supervision、Larkboat | 图像多视角、检测/分割后处理、文档/表格/PPT 办公 |
| [设计资源](catalog/design-resources.md) | Lucide、Phosphor、Tabler、Radix、Uiverse Galaxy、Emil Kowalski Skills | 产品界面、原型、前端实现、UI 动效和 Agent 设计决策参考 |
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
