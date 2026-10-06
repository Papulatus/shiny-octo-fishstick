# 视觉 AI、计算机视觉与办公自动化

## Qwen Image Multiple Angles 3D Camera — 多视角/旋转相机图像生成 Space

| 字段 | 信息 |
| --- | --- |
| 上游 | [Hugging Face Space](https://huggingface.co/spaces/multimodalart/qwen-image-multiple-angles-3d-camera) |
| 运行地址 | [Space App](https://multimodalart-qwen-image-multiple-angles-3d-camera.hf.space) |
| 作者 | `multimodalart` |
| 技术形态 | Gradio；运行时 Zero A10G（收录时）；公开 Space |
| 模型依赖 | `Qwen/Qwen-Image-Edit-2511`、多角度 LoRA、Lightning 组件（以 Space 卡片为准） |
| 收录时 likes | 2.6k |
| 最近更新 | 2026-01-08 |

### 是什么

这是一个公开的 Hugging Face 演示 Space，用 Qwen 图像编辑及多角度相关组件，从输入图像生成多角度、围绕主体旋转/3D 相机感的视觉结果。它是**生成式视觉演示与工作流参考**，不是精确几何重建、可量测 CAD 或真正 NeRF/3D mesh 输出工具。

### 适合什么

- 电商、角色、产品概念图的多视角展示草图。
- 为落地页、视频分镜、概念设计快速产生“绕拍”视觉素材。
- 研究 Qwen Image 编辑、多角度 LoRA 和 Gradio Space 的组合方式。

### 使用建议与限制

- 先用非敏感、拥有版权的输入图评估一致性；不同角度可能出现纹理、文字、遮挡和几何不一致。
- 不要把输出当成产品尺寸、医学/法证图像或真实世界证据。
- Space 运行状态和硬件会变化，可能排队/休眠；需要稳定生产能力时应审查其代码与模型许可，并自行部署可复现版本。
- 对人物图像、品牌资产和受版权保护图片，应获得适当授权并遵循模型/Space 条款。

## Supervision — 模型无关的计算机视觉工具箱

| 字段 | 信息 |
| --- | --- |
| 上游 | [roboflow/supervision](https://github.com/roboflow/supervision) |
| 文档 | [supervision.roboflow.com](https://supervision.roboflow.com/) |
| 许可证 | MIT |
| 语言 | Python >=3.10 |
| 收录时星标 | 48.4k |
| 上游最近推送 | 2026-07-27 |

### 是什么

Supervision 是 CV 应用的中间层/工具箱，不是检测模型本身。它统一不同模型输出为 `sv.Detections`，提供检测、分割、分类、跟踪、区域统计、视频处理、数据集读写/转换/拆分，以及高度可配置的可视化 annotator。可连接 Ultralytics、Transformers、MMDetection、Roboflow Inference 等生态。

### 典型用途

- 在 YOLO、RF-DETR、Transformers 等模型之后统一后处理和标注渲染。
- 视频目标跟踪、进出区域计数、热区/线计数、速度或事件分析。
- COCO、YOLO、Pascal VOC 等数据集的加载、转换、切分与检查。
- 快速做视觉模型评估 notebook 或可视化 demo。

### 最小开始

```bash
pip install supervision
```

模型推理得到检测结果后，将其转为 `sv.Detections`，再使用 `BoxAnnotator` 等渲染到 OpenCV/Pillow 图像。建议将模型推理、数据版本、阈值/NMS 参数与 Supervision 后处理配置一并记录，才能复现结果。

### 注意

- 模型输出的偏差、数据集偏差和阈值设置不会因统一工具箱而消失；必须分别评估类别、场景和设备。
- 涉及人脸、人员轨迹、车牌和公共场所视频时，应评估当地隐私、告知、保存期限与访问控制。
- 商业云推理/Roboflow API 和纯本地开源工具的凭据、成本、数据上传边界不同，部署前需明确。

## Larkboat（轻舟办公）— AI 办公与文档/数据处理平台

| 字段 | 信息 |
| --- | --- |
| 上游/官网 | [larkboat.com](https://larkboat.com/) |
| 定位 | AI 办公与数据处理服务 |
| 页面宣称能力 | Excel 数据处理、PDF 文档处理、PPT 智能生成、思维导图、AI 文档生成 |

### 适合什么

面向非代码或轻代码办公场景，适合把表格清洗、文档转换、演示文稿初稿、思维导图和文档生成放入同一工具链。适合先用真实但脱敏的小样本验证交付质量与编辑成本。

### 采用前检查

这是第三方在线服务而非可审计的开源仓库。上传前必须确认：数据存储地点、是否用于模型训练、数据删除机制、导出格式、价格/额度、团队权限、企业合规与隐私政策。不要上传客户数据、身份证件、商业合同、未公开财务数据或 API Key，除非已经完成供应商安全评审并签署适当协议。

## patent-disclosure-skill（中国专利.skill）— 专利交底、申请底稿与私有知识库技能套件

| 字段 | 信息 |
| --- | --- |
| 官方上游 | [handsomestWei/patent-disclosure-skill](https://github.com/handsomestWei/patent-disclosure-skill)；作者维护的项目，不是国家知识产权局官方工具 |
| 安装与入口 | [INSTALL.md](https://github.com/handsomestWei/patent-disclosure-skill/blob/main/INSTALL.md)、[SKILL.md](https://github.com/handsomestWei/patent-disclosure-skill/blob/main/SKILL.md) |
| 分类 | 视觉与办公自动化 / Agent Skills / 专利技术文档与知识管理 |
| 许可证 | MIT；已核对根目录 LICENSE，版权 2026 handsomestWei；复用需保留许可及版权声明 |
| 技术形态 | AgentSkills 布局、Markdown 提示词、Python 工具、Playwright；可选 Obsidian、向量检索、CadQuery |
| 收录快照 | 2026-10-07 CST；11,038 stars、1,168 forks；未归档，数字不是质量或法律有效性保证 |
| 维护快照 | main 最新提交 `5073d3d837a5d139ef2bc763c9dd46bb9605df84`，2026-09-30；仓库最近推送 2026-10-06；当前未发布 GitHub Release |

### 是什么

将研发材料、代码、产品图、公开专利 PDF 和审查意见组织为可交付专利文档的技能套件。覆盖中国发明、实用新型与外观设计工作流，定位为研发与专利代理协作助手，而不是代理机构、官方申请系统或自动授权工具。本库仅收录说明和链接，不镜像源代码，也未安装或运行该工具。

### 核心能力

- **交底书与布局**：梳理技术事实、挖掘候选保护点、轻量查新、Markdown/可编辑 Word 成文、框图与版本追溯；支持保护型 1+N 布局辅助。
- **申请与案卷**：生成权利要求、说明书、摘要、附图底稿；材料检查、支持性检查与问题清单；案卷工作流协调交底和申请文件迭代。
- **读专利与地图**：公开专利解读、术语和权利要求笔记、Obsidian Canvas/关系图、语义地形与引证网络等可视化。
- **检索与审查辅助**：国知局公布公告站著录检索、政策简报、审查答复草稿、脱敏案例检索、Excel 权利要求对照表。检索结果只是所用来源与查询条件下的证据，不是全面查新证明。
- **图与 CAD**：产品/结构辅助线稿、Mermaid PNG；可选 STEP 多视角投影。生成图需逐项核对结构、件号和真实技术事实，不能用推测图补齐缺失事实。

### 适合什么

研发团队整理技术贡献和交底材料、专利代理师审核申请初稿、研究者建立私有专利知识库，以及基于公开文件制作有引用的技术路线和权利要求对照。正式申请、审查答复、侵权或自由实施判断必须由具备相应资格和授权的专业人员审核。

### 推荐接入方式

1. 先阅读上游根 SKILL.md 与 INSTALL.md，固定审核过的提交。Claude Code/Cursor 可按官方安装说明将完整仓库置于技能目录，不能只复制根 SKILL.md 丢掉子技能与工具。
2. Hermes 用户需另行验证技能发现、相对路径和 Python 环境兼容性；不能把支持 AgentSkills 等同于已经验证 Hermes 一键安装可用。
3. 使用独立 Python 环境安装根 requirements.txt（python-docx、latex2mathml、PyYAML、Playwright、mammoth、python-pptx），先用虚构或已公开案例测试 Markdown/Word 交付与浏览器探测。
4. 默认可复用本机 Chrome/Edge；PDF、CAD、地图与审查案例向量库按需安装各子模块依赖。CadQuery 使用独立 Python 3.10–3.12 环境，不污染 Agent 主环境。
5. 指定受控案件输出目录与 Obsidian 库。审查案例和地图模型默认可写入系统 Documents 下的 patent-disclosure-skill 目录，采用前核对配置覆盖、权限和保留策略。

### 安全、隐私、供应链与法律边界

- **未公开技术是高敏感材料**：先核对雇主/客户授权、保密义务、发明人及申请人归属；尽量脱敏。云端 LLM、图像、embedding 与搜索可能向外传输内容，先审查服务条款、训练使用、地域和删除政策；本地知识库不等于全链路离线。
- **最小文件与网络权限**：只开放案件工作目录和指定知识库，避免整盘扫描；Secret 不写文档或仓库，第三方插件、浏览器登录态和模型依赖分别审核。
- **生成内容必须复核**：不得虚构实验数据、性能优势、附图结构或引证。核对新颖性、创造性、充分公开、支持性、清楚性和现行书式；政策与期限回到国家知识产权局官方来源确认。
- **检索合规与完整性**：遵守站点条款、速率和验证码控制，不绕过访问限制。上游明确只有 `complete: true` 才表示检索翻到末页，退出码 3 为部分结果；即使完成分页也不保证全球检索穷尽。
- **人工审批**：申请文件、审查答复、案件入库和敏感材料外发均需人工确认；本次收录未验证或授权自动提交申请、支付费用或处理法律期限。
- **许可分层**：MIT 适用于仓库代码，不自动覆盖专利文件、模型、字体、浏览器、CAD 依赖或第三方服务的数据许可；复制源码须保留声明。持续维护与星标不能替代供应链安全审查。

