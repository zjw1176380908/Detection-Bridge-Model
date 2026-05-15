明白！这里是完整的 Markdown 源码。您可以直接点击代码块右上角的“复制”按钮，然后全选您 GitHub 编辑器里的内容，直接粘贴覆盖即可：

```markdown
# DBM-65k: A Unified Framework for Data-Centric Bridge Damage Identification 🌉

[![Hugging Face](https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Models-blue)](https://huggingface.co/DBM-65k) 
[![Dataset](https://img.shields.io/badge/Dataset-DBM--65k-green)]() 
[![Baseline](https://img.shields.io/badge/Baseline-YOLOv12-orange)]()

本项目开源了 **DBM (Detection Bridge Model) 系列模型**的预训练权重与评估代码。本工作基于构建的包含 65,000 多张真实无人机巡检图像的大规模基准数据集 **DBM-65k**，为真实复杂工程场景下的桥梁结构健康检测提供了坚实的数据与算法基础。

## 🌟 核心亮点 (Highlights)

* **“宏微观分层”超大规模数据集**：DBM-65k 包含超 65,000 张图像，确立了“宏观主体表观（混凝土与钢材）与微观通用构件（缆索与螺栓）”的分层感知体系。
* **统一多任务框架**：开发了 DBM 模型系列，以实现多任务综合评估，实现了从目标检测到高精度像素级分割的任务全覆盖。
* **攻克极端尺度差异**：提出了动态对齐融合模块（DAF）和动态小目标检测头（Dynamic P2 Head），有效解决无人机远近巡检带来的极端尺度差异问题。
* **微小病害精准感知**：设计了自适应尺度感知损失 (Adaptive Scale-aware Loss)，显著放大远距离微小病害（如螺栓松动、细微裂缝）的梯度响应。
* **跨任务知识迁移**：采用检测先验驱动跨任务迁移学习，实现了宏微观检割一体化，极大提升了复杂病害（如网状裂缝、不规则剥落）的边缘解析精度。

## 🧰 模型库与性能 (Model Zoo & Performance)

基于 YOLOv12 基线架构，我们针对桥梁多尺度损伤特性进行了深度定制优化，提供以下 5 个维度的专业预训练权重：

| 模型名称 (Model) | 任务类型 (Task) | 适用对象 (Target) | 涵盖类别 (Categories) | mAP@0.50 | 权重下载 (Weights) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **DBM-Con** | 目标检测 | 混凝土结构 | 裂缝、剥落、锈蚀、孔洞 | 0.5683 | [HuggingFace](https://huggingface.co/DBM-65k) |
| **DBM-Stl** | 目标检测 | 钢结构 | 涂层早期失效、涂层剥落、表面锈蚀 | 0.9301 | [HuggingFace](https://huggingface.co/DBM-65k) |
| **DBM-Comp** | 目标检测 | 构件 | 缆索破损、螺栓 (正常/缺失/锈蚀/松动) | 0.6794 | [HuggingFace](https://huggingface.co/DBM-65k) |
| **DBM-Conseg** | 图像分割 | 混凝土结构 | 裂缝、泛碱、剥落、露筋、孔洞 | 0.7381 (Mask) | [HuggingFace](https://huggingface.co/DBM-65k) |
| **DBM-Stlseg** | 图像分割 | 钢结构 | 锈蚀等级定级 (一般、差、严重) | 0.9283 (Mask) | [HuggingFace](https://huggingface.co/DBM-65k) |

> **注**：全部训练权重均托管于 Hugging Face。

## 🚀 快速上手 (Quick Start)

### 1. 环境准备

本项目基于 PyTorch 2.1 与 Ultralytics 8.1 系列框架开发。

```bash
git clone [https://github.com/zjw1176380908/Detection-Bridge-Model.git](https://github.com/zjw1176380908/Detection-Bridge-Model.git)
cd Detection-Bridge-Model
pip install torch torchvision ultralytics

```

### 2. 模型推理 (Inference)

下载对应权重后，您可以使用以下脚本对图像进行病害检测或分割（推荐输入分辨率设定为 640x640）：

```python
from ultralytics import YOLO

# 加载 DBM 混凝土检测模型
model = YOLO("weights/DBM_Con_best.pt")

# 对图像进行推理 (保留桥梁微细病害的高频纹理信息，分辨率设为 640x640)
results = model.predict(source="test_images/bridge_sample.jpg", imgsz=640, conf=0.25)

# 可视化并保存结果
for r in results:
    r.save(filename="output/result.jpg")

```

## 📊 数据集 (Dataset: DBM-65k)

DBM-65k 采用宏观—微观分层组织方式：

* **总图像量**: 65,846 张
* **总标注实例**: 297,812 个
* **采集场景**: 采用多高度、多角度以及动态飞行方式，涵盖多种桥梁结构形式（如箱梁桥、连续梁桥、钢桁架桥、钢箱梁桥等）及服役环境。

## 📑 引用 (Citation)

如果您在研究中使用了我们的模型或数据集，请引用我们的论文：

```bibtex
@article{zheng2026dbm65k,
  title={DBM-65k: A Large-Scale Multi-Scale Dataset and Unified Framework for Data-Centric Bridge Damage Identification},
  author={Zheng, Junwen and Feng, Hao and Zhang, Jinghuan and Zhang, Jian and Duan, Wenhui},
  journal={TBD},
  year={2026}
}

```

## 🙏 致谢 (Acknowledgements)

本研究得到了中国国家重点研发计划 (No. 2022YFC3801700)、中国国家自然科学基金 (No. 52378289)、东南大学先进海洋工程研究院科研基金 (No. KP202407) 以及苏州市科技计划-关键核心技术项目 (No. SYG2025117) 的资助。

```

```
