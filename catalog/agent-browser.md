# Agent 浏览器

## ego lite — 供人和 Agent 协作的浏览器

| 字段 | 信息 |
| --- | --- |
| 上游 | [citrolabs/ego-lite](https://github.com/citrolabs/ego-lite) |
| 官网/文档 | [lite.ego.app](https://lite.ego.app/document/) |
| 许可证 | MIT |
| 平台 | macOS（收录时；Windows/Linux 在上游路线图中） |
| Agent 接入 | `ego-browser` skill；可通过 `npx skills add citrolabs/ego-lite` 添加技能 |
| 收录时星标 | 5.4k |
| 上游最近推送 | 2026-07-27 |

### 是什么

ego lite 是一个面向 AI Agent 浏览器自动化的 macOS 浏览器。它的核心思路不是让 Agent 抢占用户日常浏览标签，而是在独立 **Spaces** 中执行 Agent 任务，同时允许 Agent 复用用户已授权的浏览器登录态、Cookie、扩展和书签（是否迁移由用户在首次启动时确认）。

### 适合什么

- 需要真实登录态的网页操作，例如后台系统、研究网站或社媒工作流。
- 希望 Codex / Claude Code 等 Agent 同时跑多项浏览器任务，而不打扰手工浏览。
- 要求用户保持对浏览器资料和任务可见性的本机工作流。

### 接入方式

上游提供 macOS 应用安装包；也可仅安装技能：

```bash
npx skills add citrolabs/ego-lite
```

然后由支持对应 skill 的 Agent 使用 `ego-browser` 执行自然语言浏览器任务。上游文档说明首次使用会引导安装应用。

### 风险边界

- 登录态等同于真实账户权限。只把浏览器访问授权给可信 Agent，并在执行购买、发布、删除、传输数据等操作前设置明确确认规则。
- 不要把包含 Cookie、密码、会话令牌的浏览器配置上传到仓库或发给第三方。
- 自动化行为仍需符合目标网站条款和账户规则。
