# 设计资源

这组资源更适合作为产品设计、前端实现、原型搭建和 AI 生成 UI 时的“受约束参考”。优先确定一套主图标体系，避免在同一界面混用不同笔画、圆角与视角风格。

## 图标库

| 资源 | 上游/官网 | 定位 | 许可证 | 何时优先选 |
| --- | --- | --- | --- | --- |
| **Lucide** | [官网](https://lucide.dev/) · [GitHub](https://github.com/lucide-icons/lucide) | 社区维护的一致性线性图标，Feather Icons 分支，提供多框架包 | ISC | React/Vue/Svelte/原生前端都需要成熟生态和常用图标时 |
| **Phosphor Icons** | [官网](https://phosphoricons.com/) · [GitHub](https://github.com/phosphor-icons/core) | 同一图标提供多种 weight（thin 到 fill/duotone） | MIT | 需要用字重/填充态表达交互层级、状态变化时 |
| **Tabler Icons** | [官网](https://tabler.io/icons) · [GitHub](https://github.com/tabler/tabler-icons) | 24×24、2px stroke 的大规模 SVG 图标集（收录时 6,000+） | MIT | 需要覆盖面广、统一 24px 线框图标的后台或产品界面时 |
| **Radix Icons** | [官网](https://www.radix-ui.com/icons) · [GitHub](https://github.com/radix-ui/icons) | WorkOS 设计的精确 15×15 图标 | MIT | 紧凑控制面板、菜单、编辑器等小尺寸 UI，需要精细栅格时 |

### 选型速记

- **默认产品图标**：Lucide 或 Tabler 二选一，保持全局一致。
- **视觉层级/状态丰富**：Phosphor 的多 weight 更合适。
- **超紧凑桌面式 UI**：Radix Icons 优先。
- 采用前检查框架包、tree-shaking、可访问文本（`aria-label` / tooltip）和品牌图标授权。

## UI 术语与 Agent Prompt 辅助

### NameThatUI — UI 元素视觉词典与精确命名助手

| 字段 | 信息 |
| --- | --- |
| 官网 | [namethatui.com](https://namethatui.com/) |
| 作者/动态 | [@argofowl](https://x.com/argofowl) |
| 形态 | 在线 UI 术语词典与搜索工具；不是可确认的开源代码仓库 |
| 收录时内容 | 71 个 UI 元素词条，覆盖 macOS 与 Web |

**用途**：当你、设计师或 Agent 知道“长什么样”却不知道专业名称时，用自然语言模糊描述检索对应 UI 元素。它会给出标准术语、相关平台/API symbol，以及一段可直接粘贴给 Coding Agent 的精确实现提示。

例如可以用“菜单栏图标后面的浅色胶囊”“拖动时抓住的一排点”“页面加载时的骨架”等口语化描述，定位到 `Menu Bar Extra`、`Resize Handle`、`Skeleton` 等更准确的术语。收录时其词条涵盖：Popover/Dropdown/Tooltip 的区别、Modal/Drawer/Sheet、Combobox、Command Palette、Bento Grid、Masonry、Focus Ring、Truncation、Progress indicators 等。

**适合什么**：

- 给 Agent 写 UI 改造需求前，先把含混的视觉描述转成组件/交互术语。
- 设计评审、Bug 报告、设计系统文档中统一命名，减少“那个三个点”“那个浮层”式沟通歧义。
- 学习 macOS AppKit 和 Web UI 的常用组件名、差异及语义边界。

**推荐工作流**：

1. 在 NameThatUI 搜索或浏览，确认该元素的标准名称与相近概念区别。
2. 将其生成/提供的术语和 prompt 作为需求输入，再补充项目约束：技术栈、设计 token、暗色模式、无障碍、响应式规则。
3. 要求 Agent 先复用现有组件库；不要因知道一个术语就随意引入新的 UI 依赖。
4. 最后人工核验键盘操作、焦点顺序、ARIA 语义、移动端和空状态。

**注意**：该网站是知识与命名参考，不是某个组件的授权来源。具体组件代码、设计资产、框架 API 和商标使用仍需回到各自上游的许可证与文档核验。

## UI 组件与灵感

### Uiverse Galaxy

| 字段 | 信息 |
| --- | --- |
| 上游 | [uiverse-io/galaxy](https://github.com/uiverse-io/galaxy) |
| 浏览入口 | [Uiverse.io](https://uiverse.io/) |
| 内容 | 3,000+ 社区 CSS/Tailwind UI 元素归档 |
| 许可证 | MIT |
| 收录时星标 | 11.7k |
| 上游最近推送 | 2024-09-02 |

**用途**：按钮、卡片、加载动画、输入框等可直接参考或改造的社区组件。上游建议在网站中浏览和互动预览；仓库是平台自动归档，不接收直接 PR。

**建议**：把它用于灵感、局部组件和快速原型，而非未经审查整段复制。接入生产前检查响应式布局、无障碍、动画性能、暗色模式、依赖体积和项目设计 token 的一致性。

## AI 生成 UI 的提示规则

向 Agent 指定资源时，可附加以下约束：

```text
Use Lucide icons only. Do not mix icon libraries.
Keep icons decorative unless an accessible label is provided.
Match the existing spacing, radius, color tokens, and dark-mode behavior.
Use Uiverse only as inspiration; do not copy unreviewed component code verbatim.
```
