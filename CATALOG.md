# 目录

> 数据快照：2026-07-27。项目活跃度、星标、版本、许可证和 API 兼容性会变化，请在采用前核验上游文档与条款。

| 分类 | 条目 | 适用场景 |
| --- | --- | --- |
| [金融 AI](catalog/financial-ai.md) | Kronos | K线/量价序列预测、金融时间序列研究 |
| [量化研究与市场数据](catalog/quant-data.md) | A-Stock Data、stock-api、TA-Lib Python、LLMQuant Hermes、QuantSpace、Lumibot、WorldQuant Miner | A 股数据、MCP 行情、技术指标、研究工程、回测与交易框架 |
| [Agent 浏览器](catalog/agent-browser.md) | ego lite | Agent 浏览器自动化、复用本机登录态 |
| [浏览器、网页 Agent 与自动化](catalog/browser-web-agent.md) | CloakBrowser、Page Agent、scroll-world | 自家网页 Copilot、浏览器运行时、沉浸式品牌页 Skill |
| [网页采集](catalog/web-scraping.md) | Scrapling | 动态站点、反爬场景、自适应选择器、规模化采集 |
| [视觉 AI、CV 与办公](catalog/vision-office.md) | Qwen 多角度 3D Camera、Supervision、Larkboat | 图像多视角、检测/分割后处理、文档/表格/PPT 办公 |
| [设计资源](catalog/design-resources.md) | Lucide、Phosphor、Tabler、Radix、Uiverse Galaxy | 产品界面、原型、前端实现和 Agent 生成 UI 的素材参考 |
| [资源目录与邮箱](catalog/resources-email.md) | FMHY、Public APIs、邮箱助手 | 资源发现、API 候选、邮箱服务导航 |

## 快速选择

- 想做市场 OHLCV/K线预测或微调金融序列模型：看 **Kronos**。
- 想快速给 Agent 增加 A/港/美股查询：看 **stock-api**；想进行多源 A 股研究：看 **A-Stock Data**。
- 想在 Python/Pandas/Polars 管线中计算 RSI、MACD、布林带、ADX 或 K 线形态：看 **TA-Lib Python**；它只做指标计算，仍需自备合规行情、完成验证和风控。
- 想搭建可维护的 AI 量化研究仓库：看 **QuantSpace**；想做 Python 回测或 paper/live broker 策略：看 **Lumibot**。
- 已部署 Hermes，想将市场数据查询、晨报、财报/13F/持仓观察做成可控的 MCP + 定时工作流：看 **LLMQuant Hermes**；它需要独立的 LLMQuant Data 凭据、预算控制与人工风控。
- 想让 Codex、Claude Code 等 Agent 在不抢占日常标签页的前提下完成浏览器任务：看 **ego lite**。
- 想把自然语言 GUI 助手嵌入自己的网站：看 **Page Agent**。有明确授权的自动化/检测测试需要时再评估 **CloakBrowser**。
- 想写 Python 采集脚本，且目标站点可能改版或有反自动化限制：看 **Scrapling**。
- 想统一产品图标风格：通常先看 **Lucide** 或 **Tabler**；需要字重变化看 **Phosphor**；需要紧凑栅格看 **Radix Icons**；需要现成 CSS/Tailwind 灵感看 **Uiverse Galaxy**。
- 想做模型无关的 CV 后处理、标注、跟踪和区域计数：看 **Supervision**。
- 想发现公共 API、免费资源或邮箱服务：看 **Public APIs / FMHY / 邮箱助手**，但所有外链都必须单独核验。