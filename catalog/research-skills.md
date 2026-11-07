# 调研与研究 Skills

> 本页收录「把外部信息变成可交付成果」这一层的能力：调研方法论、文献工作流、报告生成、数据核对。与其他分类的区别是——这里收的是 **Skill（方法/工作流）**，不是数据源或框架本体。

## Deep Research Anything（本库维护的原创 Skill）

| 字段 | 信息 |
| --- | --- |
| 上游 | [Papulatus/deep-research-anything](https://github.com/Papulatus/deep-research-anything)（本库维护） |
| 许可证 | MIT |
| 形态 | 纯 Skill（`SKILL.md` + 6 个 `references/` 模块），无运行时、无内置服务、无第三方代码 |
| 方法来源 | 合并自 [bytedance/deer-flow](https://github.com/bytedance/deer-flow)（MIT）的六个研究类技能，已去重重构并补齐数据源契约 |
| 文档语言 | 中英双语（`README.md` / `README.zh-CN.md`） |
| 收录时星标 | 新仓库（自有） |

**定位**：一个技能覆盖六类调研任务——网络调研、论文精读、系统性文献综述、咨询级报告、表格与 SQL 分析、开源仓库尽调。六者共享同一套五阶段流程（定界 → 取证 → 校验 → 综合 → 交付）、同一道质量闸门（结论可追溯、不编造数据）和同一个数据源接入契约。

**为什么合并**：真实任务很少是"纯"的（调研完要出报告、读完论文要写综述、自己的数据要支撑结论）。分开维护六套技能会导致质量要求各写一遍且漂移、引用与数据纪律不一致、私有数据无处安放。合并后 `SKILL.md` 只做路由与公共纪律，六套方法论按需加载。

**核心能力**

- **路由表**：按任务信号自动选模式，支持组合（调研+报告、综述+精读、数据+报告、仓库+生态口碑）。
- **五条硬纪律**：任何无法溯源的数字写 `Data not available`；至少 3–5 个角度且正反面覆盖；时效按问题精度对齐；结论挂来源与日期；跨源指标先声明口径。
- **数据源契约**（`references/data-sources.md`）：支持 SQL / HTTP API / 文件 / MCP / 脚本五种形态，适配器只需实现 `describe()`、`query()`、`provenance()`、`freshness()`；凭据只从环境变量读。
- **数据到手三条硬规则**：不插值补齐、不跨源混算、差异必须留痕（幅度 + 原因 + 适用边界）。
- **可直接用的模板**：论文评审（S/W/Q 结构）、文献综述（APA/IEEE/BibTeX）、咨询报告骨架（30+ 框架选 2–4 个）、仓库尽调（健康度清单 + 置信度分级）。

**接入方式**

```bash
# Hermes Agent
cp -R deep-research-anything ~/.hermes/skills/
# Claude Code / Codex / Cursor
cp -R deep-research-anything ~/.claude/skills/
# DeerFlow：放到 skills/public/ 下即可
```

**边界与注意事项**

- 只覆盖**公开信息**；内部专有数据必须自行按契约接入，本技能不携带任何数据源。
- 时间精度到"日"，不适用于秒级实时数据（行情、突发新闻请走 API）。
- 不作专业背书：医疗、法律、投资建议场景只产出证据与角度，不替代专业判断与合规审查。
- 不绕过付费墙/登录墙；读不到全文的来源会显式标注，不允许"假装读过"。
- 文献综述单次上限 50 篇；更大规模应按子主题拆分。
- 技能质量取决于模型自身的检索与判断能力，仍需人工复核关键结论与引用。

## DeerFlow 2.0 — 长任务 SuperAgent Harness（上游来源）

| 字段 | 信息 |
| --- | --- |
| 上游 | [bytedance/deer-flow](https://github.com/bytedance/deer-flow) |
| 许可证 | MIT |
| 形态 | 完整应用：Python 3.12+/LangGraph 后端 + Next.js 前端 + Gateway（默认 `:2026`）+ 沙箱 + 技能系统 + 插件 API |
| 收录时星标 | 85,114 |
| 上游最近推送 | 2026-11-07 |
| 开放 issue | 1,061 |
| 最新版本 | v2.2.0（2026-11-04） |

**定位**：字节开源的长任务智能体运行框架，编排子代理、记忆、沙箱与技能，跑分钟级到小时级任务。2026-02-28 发布 2.0 当天登上 GitHub Trending 第一。

**v1 与 v2 的关系（重要）**

- **v2 是从零重写**，与 v1 不共享代码；v1 冻结在 `main-1.x` 分支（无 tag/release，`git clone -b main-1.x`）。
- v1 是"社区驱动的 Deep Research 框架"，采用固定多角色研究流程；v2 是通用 harness + 可插拔技能 + 四档执行模式（`flash` / `standard` / `pro` 规划 / `ultra` 多子代理）。
- 想复刻 v1 那套固定研究流水线 → 用 `main-1.x`；想要"调研 + PPT/播客/视频/图表 + MCP + 私有知识库"→ 用 v2。

**内置研究类技能**（本库的 Deep Research Anything 即由这六个合并而来）

`deep-research`、`academic-paper-review`、`systematic-literature-review`、`consulting-analysis`、`data-analysis`、`github-deep-research`

**运行时依赖（最小集）**

| 项 | 必需性 | 说明 |
| --- | --- | --- |
| LLM 端点（OpenAI 兼容） | 必需 | 默认火山方舟 + `doubao`；可换 Ollama 或 Gemini AI Studio |
| 搜索 API key（Tavily / InfoQuest / Brave） | 必需 | 没有信息源，研究流程不成立 |
| Python 3.12+（uv） | 必需 | 后端 |
| Node.js 22+（pnpm） | 仅 Web UI | 纯 API 使用可省 |
| Docker | 可选 | 仓库自带 `Dockerfile` + `docker-compose.yml`（后端 `:8000`、前端 `:3000`） |
| Python REPL / MCP server / TTS / RAGFlow | 默认关闭 | 研究流程不需要 |

**工程与安全边界**

- 沙箱与产出物目录会承载用户文件，需限制网络出口与访问权限。
- 技能支持社区 `.skill` 归档导入，**导入前必须审查内容**（上游提供 `review_skill_package` 仅审查不激活）。
- 插件 API 可提供整页 UI 与后端操作，第三方插件等同代码执行，须审查后安装。
- 付费搜索/模型调用会产生实际成本，建议设预算与速率上限。
- 上游迭代极快（v2.0.0 → v2.2.0 三个月内），自建部署应锁定 tag，不要跟 `main`。
