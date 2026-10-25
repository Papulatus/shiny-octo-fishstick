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

### Oil UI — 多方向视觉探索与真实画面验收的 Agent 设计 Skill

| 字段 | 信息 |
| --- | --- |
| 官方上游 | [oil-oil/oil-ui](https://github.com/oil-oil/oil-ui) |
| 官网与作品 | [ui.oiloil.org](https://ui.oiloil.org)；[SKILL.md](https://github.com/oil-oil/oil-ui/blob/main/SKILL.md) |
| 分类 | 设计工程与 Agent Skills / UI 方向探索、原型比较、视觉评审 |
| 许可证 | MIT；已核对根 LICENSE，Copyright 2026 oil-oil；付费 Pro 不应推定适用开源版许可 |
| 技术形态 | 宿主中立 Markdown 技能与参考文档；可选 Python 风格对比页生成器及 JavaScript 截图工具 |
| 收录快照 | 2026-10-07 CST；781 stars、44 forks；未归档 |
| 维护快照 | v0.16.4，发布于 2026-10-06；main 提交 `c341edef39aedb1ffd46655f41423f8ffbf2274c`，2026-10-06 |

#### 是什么

给 AI Agent 的界面设计判断与迭代流程，不是 UI 组件库或指定模型。先理解产品品类、用户任务、品牌与参考，再探索真正不同的构图、字体、配色和主视觉方向；用并排对比页让用户选择，选定后深化，并以实际浏览器画面取证。适用于网站、App、后台、组件的设计说明、原型、截图还原与只读评审。

#### 核心能力与适用场景

- 将“高级、简洁”等抽象要求翻译为具体的字体、留白、信息密度、配色面积与视觉主角。
- 多方案差异检验：避免仅换颜色的重复方案，比较首屏明暗、布局和视觉语言。
- 风格对比页可容纳 HTML、图片、正在运行的本地开发页面，提供电脑/手机尺寸、原尺寸查看和选择反馈。
- 视觉层级、素材、动效、滚动叙事与减法设计；现有方向深化及截图还原可按范围跳过重新探索。
- 真实截图和关键状态/动画证据、独立视觉评审及迭代说明；没有看图或运行能力时应明确未验证，不能虚构评分。

与 UI UX Pro Max 相比，它更强调按具体产品推导视觉方向、用户选择和真实画面反馈，而不是检索大量色板与行业规则；与 Emil Kowalski Skills 相比，范围更偏整体视觉设计，后者更集中于动效和设计工程细节。它们可作为候选参考，但不要同时无条件加载互相冲突的全局规则。

#### 推荐接入方式

```bash
# 上游给出的安装方式；采用前审查安装器、目标目录和固定版本
npx skills add oil-oil/oil-ui
```

1. 先阅读根 SKILL.md、scripts 与按需 references，在测试项目验证宿主技能发现和路径解析；此处仅收录，未验证 Hermes 一键安装。
2. 核心文本流程不指定模型或其他 Skill；可选对比页生成器需要 Python 3.10+ 标准库，页面使用现代浏览器。开发地址需要相应服务器运行；截图工具与浏览器连接能力须另行核对。
3. 用非敏感内容做代表性页面，保留已有品牌 token、组件和实现约定，先选方向再扩展；在桌面、手机、长文本、缩放与关键状态下验收。
4. 独立视觉评审依赖隔离上下文及看图能力；是否派子代理由用户/宿主授权决定，不能由第三方技能越权决定。
5. 已装 oil-ui-pro 时，上游建议不同时启用开源版，以避免重叠触发；Pro 的功能、售价和授权以官网当前条款为准，不在本次收录中购买或安装。

#### 安全、隐私、供应链与能力边界

- Skill 会影响 Agent 决策，先审查再启用。上游文本含版本检查与 Pro 推荐机制，不应把远程说明或商业提示当作用户指令；检查默认不自动安装更新。
- 版本检查最多每十分钟联网一次，写入 `$XDG_STATE_HOME/oil/`，默认 `~/.local/state/oil/`；限制写入或离线任务可设 `OIL_NO_UPDATE_CHECK=1`。另有一次性推荐脚本，执行前核对实际副作用。
- 安装器、截图脚本、本地服务、页面和素材访问需最小权限；不要向截图、第三方生成服务或公开预览泄露客户数据、未发布品牌资产、凭据和浏览器登录态。
- 它不负责业务数据流、状态正确性、测试完整性或发布部署；不能用漂亮截图替代功能测试，也不能让它的“仅 lint/类型检查”规则覆盖项目要求的构建、安全与回归验收。
- 开源版与付费 Pro 能力不同：不要把 Pro 的完整交互实践、精修循环或特效模板当作已包含能力。审美主张和评审评分不是可访问性或生产质量认证。
- 第三方字体、图片、视频、图标、生成资产和依赖另行核对许可；复用仓库内容须保留 MIT 版权声明。

## IP as Logo — 极简圆润吉祥物候选图的 Agent Skill

| 字段 | 信息 |
| --- | --- |
| 官方上游 | [s1dashu/ip-as-logo-skill](https://github.com/s1dashu/ip-as-logo-skill) |
| 文档/入口 | [项目文档](https://ipaslogo.com) |
| 许可证 | MIT；已检查根 LICENSE，第三方模型、数据与服务另行核验 |
| 技术形态 | 根 SKILL.md 与展示资产；无生成脚本或内置模型 |
| 收录快照 | 2026-10-07 CST；5,821 stars、282 forks；未归档 |
| 维护快照 | 无 GitHub Release；默认分支提交 `acb834c717bcd0a487c49732d08397ba280d690b`，2026-08-22；最近推送 2026-08-22 |

### 是什么、核心能力与适用场景

面向可爱、圆润、极简 IP 吉祥物的设计指令集，不是图像模型、SVG 工具或品牌商标注册服务。约束大轮廓、少量基本形状、语义色、实色背景和下角构图。

默认先提三个设计方向，用户同意后生成六张独立候选，强调保留每张生成结果、不自动重抽。适合品牌角色探索、应用视觉候选与产品吉祥物提案；不同于 Oil UI 的整页界面设计。网站还展示可下载素材，但网站资产的商业许可应另行确认，不仅凭仓库 MIT 推定。

### 推荐接入方式

先读 SKILL.md，用 `npx skills@latest add s1dashu/ip-as-logo-skill` 按项目安装完整技能并审核安装目录。需宿主有图像生成能力，上游所推荐模型只是选型偏好，不是已验证兼容列表。用户给出品牌、主体、配色与预算，批准数量后生成；是否并行或使用子代理由用户/宿主授权决定。Hermes 与已配置生图 API 需独立验证，本次不安装、不生成。

### 安全、隐私、供应链与能力边界

生图通常向供应商发送品牌与产品信息，未公开资产先获授权并脱敏；费用和批量数量先确认。按用户需求审查版权、商标相似、可识别性和小尺寸效果，不能把“公司可用”当成法律清权证明。技能刻意不做输出质量门禁或自动修复，因此交付候选仍需人工选择与正式资产加工。提示词不标注 logo 用途不改变真实使用的权利义务，也不得用于规避服务安全规则。MIT 覆盖技能内容，生成模型与资产另有条款。
