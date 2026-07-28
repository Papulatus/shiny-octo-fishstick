# 浏览器、网页 Agent 与自动化

本页区分三类能力：**网页内 Copilot**（嵌入自己产品）、**Agent 浏览器运行环境**（代替/增强 Playwright）和**设计/内容生成 Skill**。它们的权限边界完全不同：可操控真实登录态或规避检测的工具，必须采用最小权限、人工确认和审计日志。

## CloakBrowser — 源码级指纹修补的 Chromium 自动化运行时

| 字段 | 信息 |
| --- | --- |
| 上游 | [CloakHQ/CloakBrowser](https://github.com/CloakHQ/CloakBrowser) |
| 官网 | [cloakbrowser.dev](https://cloakbrowser.dev/) |
| 许可证 | MIT（仓库代码；最新二进制/并发能力有免费与 Pro 区别） |
| 语言/接入 | Python、Node.js/Playwright、Puppeteer、.NET；Docker |
| 收录时星标 | 29.3k |
| 上游最近推送 | 2026-07-27 |

### 定位与原理

CloakBrowser 提供经过源码层 C++ patch 的 Chromium 二进制，目标是让自动化浏览器的 canvas、WebGL、字体、GPU、音频、屏幕、WebRTC、CDP 输入行为和网络时序等特征更接近普通浏览器。它并非单纯注入 JS 或改启动 flag；上游宣称可作为 Playwright/Puppeteer 的近似 drop-in replacement。首次运行会自动下载约 200MB 浏览器二进制。

### 能力与接入

- Python：`pip install cloakbrowser`，以 `launch()` 创建浏览器。
- JS：结合 `playwright-core` 或 `puppeteer-core` 安装；多数页面操作 API 与原生态相近。
- 可选 `humanize=True`，使鼠标、键盘与滚动以拟人化节奏运行。
- 支持持久化 profile、扩展加载、HTTP/SOCKS5 代理，以及基于代理出口匹配时区/语言的 `geoip` 选项。
- 上游提供 `docker run --rm cloakhq/cloakbrowser cloaktest` 作为环境检测入口。

### 适合什么

- 已获授权的站点 QA、兼容性回归和内部流程自动化。
- 自己拥有/运营的网站上，对自动化检测策略做防御性测试。
- 合规的数据采集任务中，需要比默认 headless browser 更稳定的浏览器特征。

### 不适合/风险

此项目明确围绕反检测、验证码和反爬环境。技术能力不代表有权绕过目标站点的访问控制。不得用于规避付费墙、绕过 CAPTCHA、未授权访问、批量注册、欺诈、账户接管或违反站点条款的采集。代理、浏览器 profile、cookie 和 license key 都是敏感资产；不可提交到 Git、聊天记录或日志。

### 采用建议

1. 先使用普通 Playwright；只有在明确、合法的测试/业务理由下才评估 CloakBrowser。
2. 一律将账号权限限制到专用测试账户，且对付款、发布、删除、数据导出等动作增加人工确认。
3. 固定版本并做供应链审查：它会下载二进制，生产中应记录下载来源、版本、哈希与更新策略。
4. 将代理和授权密钥放环境变量/Secret；设置速率限制、失败上限和审计日志。

## Page Agent — 直接嵌入网页的 GUI Agent

| 字段 | 信息 |
| --- | --- |
| 上游 | [alibaba/page-agent](https://github.com/alibaba/page-agent) |
| 文档/演示 | [alibaba.github.io/page-agent](https://alibaba.github.io/page-agent/) |
| 许可证 | MIT |
| 技术形态 | TypeScript / npm；可选 Chrome Extension 与 MCP Server（Beta） |
| 收录时星标 | 28.0k |
| 上游最近推送 | 2026-07-23 |

### 是什么

Page Agent 的定位是“生活在网页里的 GUI Agent”：在自己的网站中加载一段 JavaScript，给当前页面提供自然语言操作能力。它以文本化 DOM 处理为主，不依赖截图或多模态模型；可以对接自带或自部署的 LLM。可选浏览器扩展用于多页任务，MCP Server 允许外部 Agent 客户端控制该能力。

### 典型场景

- SaaS/ERP/CRM 后台的内嵌 AI Copilot：用户说一句话完成多步表单填写、导航和筛选。
- 无障碍和语音操作：将自然语言映射为页面已有控件的可理解动作。
- 给内部 Web 应用快速加 Agent 原型，不改造服务端工作流。

### 最小接入

可从 npm 安装 `page-agent`，用 `new PageAgent({ model, baseURL, apiKey, language })` 创建实例，再调用 `execute()` 执行任务。上游也有用于技术评估的 demo CDN，但其免费测试 LLM API 不应被当作生产数据通道。

### 工程注意事项

- Agent 可操作的 DOM 范围就是权限范围；敏感按钮应显式标记、拦截或要求二次确认。
- 默认将“读取”和“写入/提交/删除”分成不同工具策略；让 LLM 先生成计划，再由可信策略层授权动作。
- LLM API Key 应在后端/受控代理保存；直接放到浏览器前端会泄露。
- Page Agent 适合**增强自家页面**，不是服务端大规模网页爬虫或未授权网站自动化方案。

## Agent Reach — 多渠道网络调研能力层

| 字段 | 信息 |
| --- | --- |
| 上游 | [Panniantong/Agent-Reach](https://github.com/Panniantong/Agent-Reach) |
| 许可证 | MIT |
| 实现 | Python CLI、SKILL.md、MCP/上游 CLI 集成 |
| 收录时星标 | 61,419 |
| 上游最近推送 | 2026-07-25 |

**定位**：不是又一个单一爬虫，而是给 Claude Code、Codex、Cursor、OpenClaw 等 Agent 提供“网络信息获取能力选型、安装、体检与路由”的能力层。它根据渠道提供首选/备用后端，例如网页阅读、GitHub、YouTube 字幕、B站、RSS、语义搜索，以及需要用户登录态的 X、Reddit、Facebook、Instagram、小红书、LinkedIn 等。

**工作方式**：安装后用 `agent-reach doctor` 真实检查各渠道的可用后端；Skill 让 Agent 知道何时使用 Jina Reader、`gh`、`yt-dlp`、RSS、Exa/MCP 或已授权的浏览器会话。它更像“受维护的工具路由表”，底层读取实际仍由上游工具执行。

**适合**：经常让 Agent 做跨网页、视频、代码仓库、RSS、社媒和公开资料调研，同时希望减少“工具过期/换代后重新踩坑”的个人开发环境。

**安装前的安全审查**：

- 上游安装流程可能执行 `pip install`、系统包/Node/gh CLI 安装、MCP 配置和 Skill 文件写入；先用 `--dry-run` 或 `--safe` 审阅动作，再在测试环境验证。
- 不要让它在生产主机或高权限 Agent 中无审查地自动安装依赖；尤其不要自动开放 `exec` 权限或修改全局 Agent 配置。
- Cookie、Token 和浏览器登录态等同账户权限。上游建议其配置保存在本机；仍应使用专用低权限账号、文件权限 `600`、密钥轮换和最小化渠道启用。
- 有登录态的社媒自动化可能违反平台规则或触发封号。只用专用账号，遵守条款，禁止抓取私密数据或绕过访问控制。
- 它会更新“当前最佳后端”的判断；每次更新前应审阅 release、变更的上游工具和新增加的权限需求。

**推荐采用路径**：先在开发机启用无凭据的网页/GitHub/YouTube/RSS 渠道，跑 `doctor`；随后按需、逐项启用需要登录态的渠道。不要将其与拥有主机文件读写、Docker socket 或生产凭据的 Agent 混用。

## scroll-world — 滚动驱动的 3D 场景飞行落地页 Skill

| 字段 | 信息 |
| --- | --- |
| 上游 | [oso95/scroll-world](https://github.com/oso95/scroll-world) |
| 许可证 | MIT |
| Agent 接入 | Claude Code Plugin；`npx skills add oso95/scroll-world -a codex`；兼容 `SKILL.md` Agent |
| 收录时星标 | 5.5k |
| 上游最近推送 | 2026-07-16 |

### 是什么

这是一个给 Codex、Claude Code 等 Agent 使用的 Skill，用来制作“随滚动穿越连续 3D 世界”的品牌落地页：镜头从场景外进入内部，再无切镜地连接下一个场景。它提供需求访谈流程、提示词模板、图像/视频生成流水线、场景连接帧规则和框架无关的原生 JS scroll-scrub 引擎。

### 工作流与依赖

- 先收集品牌、行业、场景顺序、艺术方向、移动端需求与预算。
- 用 Higgsfield（或有 Codex CLI 时可用其 `image_gen`）生成场景静帧；再用图生视频生成镜头与连接片段。
- 从相邻视频真实帧制作 connector，以降低接缝跳变；移动端可单独生成 9:16 链路，而不是裁切桌面视频。
- 依赖 Higgsfield CLI 与额度、`ffmpeg/ffprobe`、Python/Pillow；生成成本由场景数量和移动端链路决定。

### 采用建议

- 适合高视觉预算的品牌页、发布页和展示型营销页面；不适合低带宽、强 SEO 文本优先或频繁迭代的业务后台。
- 生成前让 Agent 输出镜头表、预估图/视频生成数和预算，获得确认后再花费 credits。
- 将 MP4/WebP 视为项目大资产：接入 CDN、懒加载、预加载关键段、提供 poster 和 `prefers-reduced-motion` 降级。
- 确保生成素材、品牌元素、人物肖像和音乐具备适当授权。
