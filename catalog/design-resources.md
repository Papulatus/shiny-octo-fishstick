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

## AI 设计系统与 Agent Skill

### UI UX Pro Max — 面向 Coding Agent 的设计智能 Skill

| 字段 | 信息 |
| --- | --- |
| 上游 | [nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) |
| 官网 | [uupm.cc](https://www.uupm.cc/) |
| 许可证 | MIT |
| 形式 | Python/CLI + Agent Skill；支持 Claude、Codex、Cursor 等生态 |
| 收录时星标 | 111,040 |
| 上游最近推送 | 2026-07-28 |

**定位**：给 Coding Agent 的 UI/UX 设计决策资料库与生成器，而不是单一组件库。它将产品类型、页面模式、UI 风格、色板、字体配对、图表、技术栈、无障碍/交互规则和反模式组合为可搜索/可推理的设计系统建议。

**收录时能力快照**：README 描述 161 条行业推理规则、84 种 UI 风格、192 套色板、74 组字体配对、25 类图表建议、22 种技术栈和 98 条 UX 指南。v2 的 Design System Generator 会针对产品需求输出页面模式、视觉方向、色彩、排版、动效、反模式与交付检查清单。

**适合什么**：

- 让 Codex/Claude Code 在实现前生成具有一致性的设计 brief，而不是直接产出千篇一律的“AI 紫渐变”页面。
- 为金融 Dashboard、SaaS、营销页、移动端或多框架项目定义明确的 UI 约束。
- 在代码评审阶段检查对比度、焦点可见性、`prefers-reduced-motion`、响应式断点、图标策略和交互状态。

**推荐工作流**：

1. 输入业务类型、目标用户、平台、技术栈、品牌资产、内容密度、无障碍等级和关键转化目标。
2. 将生成结果视为初稿，保留项目已有 design token、组件库和品牌规范的优先级。
3. 要求 Agent 先输出页面信息架构、token、组件状态和验收清单，再写组件代码。
4. 用真实 375/768/1024/1440 宽度、键盘导航、暗色模式、长文本和空/错/加载状态验收。
5. 对金融、医疗、政务等高可信场景，优先清晰度、数据来源与可访问性，不要为“酷炫风格”牺牲可读性。

**注意**：大量风格/色板和推理规则可帮助发散，但不能替代实际用户研究、品牌判断或可用性测试。安装 Skill/CLI 前应审查脚本的写入位置、依赖与更新行为；不要把它自动生成的设计决策当作无条件规范。

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

### Emil Kowalski Skills — 面向设计师与工程师的 UI 动效与设计工程 Skill 集

| 字段 | 信息 |
| --- | --- |
| 官方上游 | [emilkowalski/skills](https://github.com/emilkowalski/skills) |
| 作者/主页 | [Emil Kowalski](https://emilkowal.ski/) · [Skills 介绍](https://emilkowal.ski/skill) |
| Skill 安装入口 | [skills.sh](https://skills.sh/emilkowalski/skills) · `npx skills@latest add emilkowalski/skills` |
| 许可证 | MIT（仓库 `LICENSE`；其中引用的设计理念、Apple/平台资料和第三方 UI 库仍需分别遵守各自条款） |
| 技术形态 | Markdown/SKILL.md；面向 Claude Code、Cursor、Codex 等 Coding Agent 的设计与动效指导 |
| 收录快照 | 2026-08-09：27,322 stars、1,497 forks；未归档；最近提交 2026-08-05（新增 `animate` Skill） |

### 是什么

这是 Emil Kowalski 为设计师和工程师整理的一组前端设计工程 Skills。它把 UI 动效、交互细节和设计决策中的经验固化为 Agent 可读取的操作规则，目标是让 Coding Agent 不只“把组件做出来”，还会考虑动效是否必要、动效目的、属性选择、曲线/时长、可中断性、空间来源、性能和 reduced motion。

它不是组件库、CSS 代码片段集合或视觉素材库，也不会替代真实设备测试和设计评审。更准确地说，它是一个**设计判断与实现审查层**，可以与现有 React/Vue/Svelte 组件库、CSS 体系、Framer Motion/WAAPI 等实现工具配合使用。

### 包含的 Skills

- **`emil-design-eng`**：总的设计工程理念，覆盖 UI polish、组件反馈、Popover/Tooltip/Toast、动效属性、弹簧、手势、性能、无障碍和审查清单。
- **`animate`**：从零构建动效，依次判断“该不该动”、动效目的、实现工具、属性、曲线/时长、打断与退出方式、reduced-motion 和 hover 条件。
- **`review-animations`**：以较高标准审查已有动效；会重点发现 `transition: all`、`scale(0)`、交互使用 `ease-in`、布局属性动画、过长时长、错误 transform-origin、缺少 reduced-motion 等问题。
- **`improve-animations` / `find-animation-opportunities`**：对整个代码库做只读动效审计、发现适合动效的地方，并输出按优先级排序的实施计划。
- **`apple-design`**：将 Apple 的反馈、直接操作、手势跟手、弹簧、材料层次、排版和可访问性原则转译为 Web UI 指导。
- **`animation-vocabulary`**：把“那个弹出来的效果”等模糊描述转换成更精确的动效术语，方便需求和 Agent prompt 沟通。
- **`pick-ui-library`**：从一套有观点的库清单中选择 Toast、Popover、拖拽、虚拟列表、状态管理、动效等依赖；仅在明确调用时触发。
- **`prototype`**：制作多个真正不同的 UI 变体，通过交互式 picker 进行比较；只在明确调用时触发，不会自动进入生产代码。

### 适合什么

- 让 Codex、Claude Code 或 Cursor 在实现 UI 前先做动效/交互决策，而不是默认给所有元素加动画。
- 审查已有 Web UI 的动效质量，统一曲线、时长、transform-origin、进入/退出路径与中断行为。
- 改造 Toast、Popover、Tooltip、Dropdown、Sheet、Modal、Tab、列表进入和拖拽反馈等高频组件。
- 为设计系统补充 `prefers-reduced-motion`、触摸设备 hover、键盘操作、焦点反馈和真实设备验收规则。
- 在项目早期通过 `prototype` 做多方向原型比较；选定后再将实现提升到生产组件。

### 推荐接入方式

```bash
# 先审查安装内容，再按需安装整个仓库的 Skills
npx skills@latest add emilkowalski/skills

# 或从上游直接阅读某个 SKILL.md，选择性纳入 Agent 工作流
```

建议按任务选择，而不是默认把全部规则注入所有项目：

1. 新增动效：先使用 `animate`，确定目的、工具、属性、曲线、时长和打断方式。
2. 检查已有动效：使用 `review-animations`，先看 Findings 与 Block/Approve 判断，再决定是否修改。
3. 全库治理：使用 `improve-animations` 或 `find-animation-opportunities`；它们是只读建议，不会替你改源码。
4. 原型探索：使用 `prototype` 建立隔离路由或独立 HTML picker，不要让实验代码直接污染生产组件。
5. 最终验收：在 DevTools 中慢放/逐帧检查，测试真实触摸设备、键盘焦点、暗色模式、长文本、低性能设备和 `prefers-reduced-motion: reduce`。

### 设计与工程边界

- **动效不是越多越好**：应服务反馈、空间一致性、状态说明、避免突兀变化或少量首次体验上的 delight；键盘快捷键和高频操作不应被多余动画拖慢。
- **性能优先**：默认优先 `transform` 与 `opacity`，谨慎使用 `clip-path`；避免动画化 width/height/margin/padding/top/left 等会触发布局的属性。不能机械套用规则，Accordion 等场景仍需根据实际体验评估。
- **真实交互必须可中断**：手势/拖拽应从当前 presentation value 继续，保持速度交接；不要在用户反向操作时从逻辑目标值跳回。
- **可访问性必须保留**：实现 movement 时提供 `prefers-reduced-motion` 降级；不要把 hover-only 反馈当作触摸或键盘用户唯一入口。
- **依赖建议不是强制规范**：`pick-ui-library` 的库清单体现作者偏好，不等同于项目必须安装的依赖。先检查现有设计系统、bundle 体积、许可证、维护状态和无障碍能力。
- **Apple Design 是转译参考**：它提供 Web 交互启发，不是 Apple 官方 Web 组件库、品牌授权或平台合规证明。

### 安全与维护注意

1. 安装前检查 `SKILL.md` 和安装器/CLI 的实际写入路径、依赖与更新行为；Skill 本质上是会影响 Agent 决策的指令内容，不能盲目从任意 fork 安装。
2. 将其按项目/平台局部启用，避免全局设计规则覆盖既有品牌规范、design token 或业务无障碍标准。
3. 任何“视觉更好”的建议都需要真实浏览器、真实设备和用户场景验证；Agent 无法仅凭源码保证动效的节奏、触感、性能和可用性。
4. 引用的第三方库、WWDC/Apple 资料、字体、图标与视觉资产须回到各自官方许可核验；MIT 只覆盖该仓库自身可授权内容。

