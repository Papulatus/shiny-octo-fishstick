# 网页采集

### MediaCrawler — 多平台自媒体公开内容采集工具

| 字段 | 信息 |
| --- | --- |
| 上游 | [NanmiCoder/MediaCrawler](https://github.com/NanmiCoder/MediaCrawler) |
| 文档/主页 | [nanmicoder.github.io/MediaCrawler](https://nanmicoder.github.io/MediaCrawler/) |
| 许可证 | GitHub API 未声明 SPDX；使用前须阅读仓库 [`LICENSE`](https://github.com/NanmiCoder/MediaCrawler/blob/main/LICENSE) 与免责声明 |
| 实现 | Python、Playwright、Node.js；可选 WebUI |
| 收录时星标 | 58,800 |
| 上游最近推送 | 2026-07-24 |

**定位**：用于采集小红书、抖音、快手、B站、微博、贴吧、知乎等平台**公开内容**的浏览器自动化项目。README 描述支持关键词搜索、指定帖子、评论/二级评论、作者主页、登录态缓存、代理和词云；同时提供命令行和可视 WebUI。

**工作原理**：默认 CDP 模式连接用户已经打开的 Chrome，复用该浏览器已有会话以完成页面操作；也可使用标准 Playwright。它不是官方数据 API，也不是通用的“无风险反爬绕过器”。

**适合**：小规模、可审计的公开舆情/内容研究；在有明确授权的测试账号和目标范围内学习浏览器自动化架构。数据采集前应先确认平台条款、法律、版权、个人信息规则和本地监管要求。

**安全与合规边界**：

- 只用专用、低权限研究账号；Chrome 登录态/Cookie 等同于账户权限，绝不提交、共享或交给不可信 Agent。
- 不要采集私密内容、绕过付费/访问控制、批量骚扰、账户注册或大规模下载；不要将“技术上可行”理解为“获授权”。
- 先设置关键词/帖子上限、并发、速率、失败重试和数据保留期；原始内容可能包含个人数据，需最小化保存和访问控制。
- 默认不要把 CDP `9222` 或 WebUI/API 端口暴露到局域网/公网；如需远程访问，使用认证与明确网络边界。
- 上游 README 明确声明仅供学习/参考并禁止非法、侵权或大规模滥用；应将其视为风险提示而非法律豁免。

### Scrapling — 自适应网页采集框架

| 字段 | 信息 |
| --- | --- |
| 上游 | [D4Vinci/Scrapling](https://github.com/D4Vinci/Scrapling) |
| 文档 | [scrapling.readthedocs.io](https://scrapling.readthedocs.io/en/latest/) |
| 许可证 | BSD-3-Clause |
| 技术形态 | Python；解析器、fetcher、动态浏览器 fetcher、爬虫框架、MCP server |
| Agent 资源 | [官方 agent-skill 目录](https://github.com/D4Vinci/Scrapling/tree/main/agent-skill) |
| 收录时星标 | 71.5k |
| 上游最近推送 | 2026-07-27 |

### 是什么

Scrapling 是从单请求提取到并发爬虫都覆盖的 Python 框架。其显著特点是自适应选择器：在页面结构变化后，可尝试重新定位元素；同时提供普通、异步、隐身和动态浏览器等 fetcher，以及代理轮换、暂停/恢复与实时统计等爬虫能力。

### 适合什么

- 电商、资讯、研究资料等公开网页的结构化提取。
- 页面经常改版、固定 CSS/XPath 容易失效的采集任务。
- 需要从轻量请求逐步升级到 Playwright/动态网页或并发爬虫的 Python 项目。
- 给支持 MCP/Agent Skill 的 Agent 提供受控网页采集能力。

### 最小示意

```python
from scrapling.fetchers import StealthyFetcher

page = StealthyFetcher.fetch("https://example.com", headless=True, network_idle=True)
items = page.css(".product", auto_save=True)
# 页面后来改版时，可尝试 adaptive=True 重新定位已保存的元素。
```

### 使用建议

1. 优先使用稳定、公开的 API 或导出接口；网页采集应是替代方案，而不是默认方案。
2. 按站点设置速率限制、重试上限、缓存、User-Agent 及可追踪日志，避免对目标站造成压力。
3. 把代理、Cookie、账号凭证放入环境变量/Secret，不写入代码或本目录。
4. 从小规模、可审计的单站点任务开始，再考虑并发、动态浏览器和代理池。

### 合规与反爬提醒

“能绕过反自动化”不等于“可以绕过”。必须遵守适用法律、网站条款、robots、版权和数据保护要求；不得将该工具用于未授权访问、规避付费墙、规避访问控制或大规模滥用。
