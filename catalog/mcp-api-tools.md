# MCP、API 与 Agent 工具桥接

> **安全与边界**：本页项目把 MCP、OpenAPI 或 GraphQL 的远程能力暴露给命令行和 Agent。它们能缩短接入时间，但也会把远端 API、OAuth、stdio 子进程和高权限工具一并带入本机环境。接入前必须审查端点、工具名单、权限范围、数据去向与服务条款；优先只读、最小 scope、独立测试凭据和显式 allowlist。不要把 API Key、OAuth token、MCP URL 或本地敏感目录写入 Git、聊天、日志或共享脚本。

## mcp2cli — 将 MCP、OpenAPI 与 GraphQL 动态转换为 CLI

| 字段 | 信息 |
| --- | --- |
| 官方上游 | [knowsuchagency/mcp2cli](https://github.com/knowsuchagency/mcp2cli) |
| PyPI | [mcp2cli](https://pypi.org/project/mcp2cli/) |
| 许可证 | MIT |
| 技术形态 | Python >= 3.10；MCP HTTP/SSE、MCP stdio、OpenAPI、GraphQL、OAuth、CLI |
| 收录快照 | 2026-08-07：2,337 stars、171 forks；未归档 |
| 上游维护快照 | 默认分支 `main`；最近代码提交 2026-06-30（[`dd2a5a6`](https://github.com/knowsuchagency/mcp2cli/commit/dd2a5a6353060a7bf8d2599bf5d037d75b26a7ae)）；源码 `pyproject.toml` 版本 3.3.1；GitHub 未提供正式 Release |

### 是什么

mcp2cli 是一个运行时适配器：它不生成客户端代码，而是读取 MCP 服务、OpenAPI 规范或 GraphQL endpoint 的能力定义，并把发现到的工具/operation/query 转换为可调用的命令行子命令。它也提供 `--json` 机器可读输出、工具搜索与缓存、使用频率排序，以及将一组连接参数保存为具名“bake”配置的能力。

它的主要价值是减少 Agent 每轮都携带完整工具 schema 的开销，并让脚本/人类与 Agent 用同一 CLI 调用已存在的 API。它**不是** API 的权限系统、数据治理层或安全审计替代品；目标服务能做什么，mcp2cli 就可能暴露什么。

### 支持范围

- **MCP HTTP/SSE**：发现和调用远端 MCP 工具；可用 `--search`、`--list`、`--transport` 控制发现与传输。
- **MCP stdio**：以本地子进程方式启动 MCP server，例如 `npx ...` 或 `node server.js`；可通过 `--env` 将环境变量交给子进程。
- **OpenAPI 与 GraphQL**：从远端或本地规范/endpoint 动态生成 CLI 命令；GraphQL 可 introspection 并构造 query/mutation。
- **OAuth**：支持授权码 + PKCE 与 client-credentials；按上游 README，令牌会缓存到 `~/.cache/mcp2cli/oauth/`，因此需要保护好本机账户和该目录权限。
- **Bake 配置**：将连接、认证和过滤条件保存到 `~/.config/mcp2cli/baked.json`，并可创建 wrapper。它适合减少重复参数，但配置应按环境、账户和权限边界拆分，不能把高权限生产端点与开发端点混在同一 bake 中。

### 接入建议

```bash
# 临时运行或全局安装
uvx mcp2cli --help
uv tool install mcp2cli

# 先只发现工具，再决定是否调用
mcp2cli --mcp https://example.com/mcp --list --search "report"
mcp2cli --spec https://example.com/openapi.json --list
mcp2cli --graphql https://example.com/graphql --list

# 用最小白名单保存一个只读 OpenAPI 工具
mcp2cli bake create readonly-api --spec https://example.com/openapi.json \
  --include "list-*,get-*" --exclude "delete-*,update-*" --methods GET
mcp2cli @readonly-api --list --top 10 --compact
```

1. 在隔离目录、测试账号和无敏感数据端点上先运行 `--list` / `--search`，人工审查工具名称、描述、输入字段和写操作。
2. 对 OpenAPI 优先限制 `--methods GET`；对 MCP bake 使用 `--include "list-*,get-*"`，并明确排除 `delete-*`、`update-*`、`create-*`、支付/下单/凭据相关工具。过滤是便利与减暴露措施，不是对上游恶意行为的安全证明。
3. 使用 `--json` 将结果交给脚本或 Agent 时，仍要校验结构、分页、错误字段和数据来源；不要把 API 返回的文本当作可信指令执行。
4. `--mcp-stdio` 会启动本地命令，等价于允许该 server 在当前用户权限下运行。固定包版本/来源，审阅命令和 `--env` 值，避免把工作目录、SSH key、云凭据或完整环境变量暴露给未知 server。
5. 对 OAuth 使用最小 scope、独立 client/测试账户和可撤销 token；检查 `~/.cache/mcp2cli/oauth/` 及 bake 配置的文件权限与备份范围。

### 凭据、缓存与供应链边界

- 上游支持 `env:` / `file:` 前缀读取 `--auth-header`、OAuth client id/secret，避免把 Secret 直接写到命令行参数（会暴露在进程列表、shell history 和日志中）。环境变量和受限 Secret 文件仍可能被同用户进程、错误日志或 CI 配置泄露，需按环境隔离。
- API/MCP 工具列表与规范默认缓存在 `~/.cache/mcp2cli/`；本地 spec 不缓存。缓存可能过期或包含敏感 endpoint 元数据，使用共享机器/备份工具时应纳入清理和访问控制。
- GraphQL introspection、OpenAPI 下载和 MCP discovery 都是网络请求；仅连接受信任域名，并验证 TLS、DNS、重定向和服务所有权。不要从提示词、网页或第三方 README 中直接复制未知 endpoint 后使用生产凭据。
- `bake install` 会创建可执行 wrapper。将 wrapper 输出目录限定为受控路径，审阅生成文件，避免 PATH 劫持或与同名系统命令混淆。

### 适合什么 / 不适合什么

**适合**：已有、受信任的 MCP/API，需要以低 schema 开销供 Codex、Claude Code、Hermes 或脚本发现和调用；或者想把只读 OpenAPI/GraphQL 查询统一为可审计命令行接口的开发团队。

**不适合直接用于**：未经审计的 MCP 市场链接、需要细粒度授权策略/审批/审计网关的生产系统、自动下单/支付/删除数据的无人工流程，或需要把第三方 API 数据任意再分发的场景。此时应先建立服务端 RBAC、网络出站控制、审计、预算/速率限制和确定性批准闸门。

## AOCI-CODE — Git 版本化的代码与数据库结构认知索引

| 字段 | 信息 |
| --- | --- |
| 官方上游 | [aoci-spec/aoci-code](https://github.com/aoci-spec/aoci-code) |
| 文档 | [中文 README](https://github.com/aoci-spec/aoci-code/blob/main/README.zh-CN.md)、[安装与签名验证](https://github.com/aoci-spec/aoci-code/blob/main/docs/install.md)、[安全政策](https://github.com/aoci-spec/aoci-code/blob/main/SECURITY.md) |
| 分类 | MCP、API 与 Agent 工具桥接 / 仓库上下文、长期索引与治理 |
| 许可证 | **FSL-1.1-MIT，Fair Source / 源码可见，不是当前的 OSI 开源许可**；已核对 LICENSE，Copyright 2026 Liu JinShi |
| 技术形态 | Go CLI、stdio MCP、Git 可版本化纯文本索引与本地治理状态；当前 go.mod 为 Go 1.26.6 |
| 收录快照 | 2026-10-07 CST；1,206 stars、156 forks；未归档 |
| 维护快照 | v0.1.0-rc18，2026-10-05 发布，仍为候选版本；main 提交 `3604ce488e29bf95389c67360c60893274aea9ca`，2026-10-06 |

### 是什么

为 Coding Agent 提供持久化系统地图的本地优先工具：模型阅读真实源码，按文件记录职责、强关联、公共契约和维护约束，再将其作为 Git 可审查的索引保存。CLI/MCP 负责范围、漂移、证据绑定、校验、并发写入与恢复。它不是自动理解业务的 AST 引擎，也不是独立知识图谱、向量数据库或对代码正确性的证明。

FRAS 分别描述 F（职责）、R（关联）、A（契约）与 S（高价值约束）。核心资产包括 aoci.txt、aoci.meta.txt、aoci.code.txt，可选 aoci.database.txt；机器状态、草稿、ledger 和恢复证据通常在 .aoci/。

### 核心能力

- 跨对话、跨 Agent 复用版本化索引，变更后识别需要重新编写的条目，区别缺失、孤立、过时及范围未纳入等漂移。
- 通过校验、锁、CAS、原子更新、Baseline 和恢复流程治理索引写入；结构与治理通过不代表模型语义正确。
- stdio MCP 当前九个工具：读取规则/概览/条目/搜索，维护、更新/移除条目及 header/report 支持工具。不需要常驻后台服务，状态面板可按需启动。
- 可选 PostgreSQL、MySQL 及受限 openGauss 6.0.5 的系统目录结构采集，不读业务行，不执行 DDL/DML；数据库证据需明确授权与接受。
- Lineage、Relations、Impact、Snapshot、Evolution 为派生观察；影响分析只沿明确写入的关系，不等于精确调用图或新的事实来源。
- 支持宿主 Agent 编写、可选显式配置的 OpenAI 兼容端点及 deterministic-only 模式；确定性模式仍需人或模型提供新增语义条目。

### 适合什么

长期维护、多会话接力、系统交接、大型代码库导航，以及将数据库 schema 与代码约束一起提供给 Agent。适合与源码、LSP、搜索、测试和运行证据配合，不适合把整个索引当作永远准确的事实或代替安全审查。初始建索引需要逐文件阅读和模型成本；上游的规模和耗时例子不是本库实测，需先做代表性小项目评估上下文预算。

### 推荐接入方式

1. 先做许可证审查与 Git 备份，在一次性测试仓库试用候选版本。下载对应平台 Release，按官方安装文档验证签名与来源；校验和只证明字节一致，不独立证明发布者。
2. 将二进制置于稳定绝对路径。源码构建须核对当前 go.mod、make 与本地依赖，不盲用旧工具链。
3. 用显式目标仓库初始化、scan，审查生成/修改的 AGENTS.md、索引骨架、Git 边界及宿主 MCP 配置。然后确认宿主实际加载的二进制版本和仓库路径，再让 Agent 建索引。
4. Claude Code、Codex、Cursor、OpenCode 等支持情况以集成文档为准；Hermes 的 stdio MCP 连接仍需实测工具契约、结果尺寸和分块读取，模型名称本身不能证明兼容。
5. 建索引后运行 verify/check，人工抽查 FRAS 和原源码。上下文压缩后重新加载完整索引，不把旧摘要当成完整交付证明。
6. 数据库默认不开通；管理员提供最小只读系统目录权限，以环境变量注入 DSN，不在聊天、索引或仓库保存凭据。

### 安全、隐私、供应链与许可边界

- **许可限制显著**：FSL 允许内部使用、非商业教育/研究及合规专业服务，但限制提供替代或实质相同功能的竞争性商业产品/服务。每个版本在公开提供满两周年后另授 MIT；按 rc18 的发布快照，对应两周年为 2028-10-05，最终以版本实际提供日期及许可证核验为准，不是现在整个仓库已 MIT。
- **本地优先不等于全链路不外发**：默认工具不额外上传源码，但宿主模型本来就可能接收源码和索引；endpoint-native 模式还可调用显式远端。需审查供应商数据条款、企业源码与 schema 保密要求。
- **“只读”有层次**：不会写业务数据库不等于零文件写入；verify/check 等可能追加 ledger，verify 还可能写历史。严格零写审计应使用隔离副本，不能仅凭命令名判定。
- init 修改项目规则与宿主配置，MCP 可以修改索引和治理状态；限定项目路径、审查权限、避免用任意仓库规则扩大生产写入授权。面板只使用回环和受控访问。
- 数据库采集需显式授权、范围与 TLS；openGauss 支持严格限定版本、模式和表类型，不推定支持所有 Gauss 分支或分区/视图等。
- 索引可能包含业务秘密，Git 提交前审查文件范围、敏感路径、条目与证据；凭据和业务数据不进入认知资产。依赖、签名、NOTICE、PATENTS、TRADEMARKS 分别审核，不据 README 推断专利授权状态。
- 当前 RC 需验证升级/回滚、并发与恢复边界；绿色检查不替代语义抽查、功能测试或生产验收。本次仅收录，未安装、连接数据库或初始化用户项目。

