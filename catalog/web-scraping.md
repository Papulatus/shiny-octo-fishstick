# 网页采集

## Scrapling — 自适应网页采集框架

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
