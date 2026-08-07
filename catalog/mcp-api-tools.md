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
