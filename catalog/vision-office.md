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
