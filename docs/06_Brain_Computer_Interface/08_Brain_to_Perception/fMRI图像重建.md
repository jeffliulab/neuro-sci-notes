# fMRI 图像重建

**fMRI 图像重建** 是 2022–2025 BCI × 生成式 AI 最惊艳的方向——**从大脑活动生成用户看到的图像**。MindEye、MindEye2、Takagi-Nishimoto 等工作在一年内把重建质量从"模糊类别"提升到"近照片级"。

## 一、核心原理

### fMRI 看视觉的什么

- **V1/V2/V3**：视网膜地形（retinotopy）——早期视觉特征
- **V4**：颜色、形状
- **IT（颞叶下）**：物体识别、语义
- **fusiform, PPA**：面孔、场景

fMRI 多体素模式分析（MVPA）能从**整个视觉皮层的 BOLD 模式**解码图像信息。

### 生成式 AI 的角色

**Stable Diffusion、CLIP** 等生成模型提供了**强先验**——即便 fMRI 信号噪声大，只要能指向正确语义，生成模型就能补出细节。

## 二、Takagi-Nishimoto 2023 CVPR

**Takagi & Nishimoto (2023)** 是现代 fMRI 图像重建的开山之作：

### 数据

- NSD（Natural Scenes Dataset）
- 8 被试、每被试 ~10000 张 COCO 图像 × fMRI

### 方法

```
fMRI voxels 
  ├→ 线性映射 → CLIP text embedding
  └→ 线性映射 → CLIP image embedding
         ↓
    Stable Diffusion
         ↓
    重建图像
```

- 用 **Ridge regression** 把 fMRI → CLIP embedding
- Stable Diffusion 从 CLIP embedding 生成图像

### 结果

- 重建出**高质量自然图像**
- 部分细节（颜色、物体类别）与真实一致
- 某些重建**视觉上难以区分**原图

## 三、MindEye 2023 NeurIPS

**Scotti et al. (MindEye)** 在 Takagi-Nishimoto 基础上大幅提升：

### 关键创新

1. **MLP + Diffusion prior**：更强的 fMRI → CLIP 映射
2. **多模态对齐**：同时映射到 image embedding 和 text embedding
3. **推理时优化**：迭代精调图像

### 性能

- **跨被试训练** + 被试特定微调
- **比 Takagi 2× 重建质量**（LPIPS、CLIP-sim 等指标）

## 四、MindEye2 2024

**Scotti et al. (MindEye2, 2024)** 的里程碑：

### 规模

- 1 小时 fMRI 即可训练
- 开源 + Hugging Face 模型

### 架构

```
fMRI 
  ↓
Pretrained fMRI encoder (跨被试)
  ↓
Diffusion prior
  ↓
CLIP embedding
  ↓
Stable Diffusion XL
  ↓
高质量图像
```

### 性能

- **Human-level image identification**：
- 从 300 张候选中识别重建 → 原图，准确率 93%
- 细节质量接近原图

### 意义

**"我们现在能从 1 小时 fMRI 重建出接近照片质量的图像"**——这是 brain decoding 十年最大跃进。

## 五、技术栈细节

### CLIP as bridge

CLIP 是 fMRI 图像重建的关键组件：

- CLIP image encoder 把图像映到 512/768 维
- fMRI 学习映射到**同一空间**
- Stable Diffusion/SDXL 接受 CLIP embedding 作为条件

这让 fMRI 解码成为**标准 CLIP-guided generation**。

### Diffusion prior

```
fMRI → Diffusion prior network → CLIP embedding
```

相比纯 MLP，diffusion prior 能建模 CLIP embedding 的分布 → 更真实重建。

### 训练数据

**NSD（Natural Scenes Dataset）** 是这一领域的 ImageNet：
- 8 个被试
- 每被试 ~10,000 张 COCO 图像
- 每图 3 次呈现
- 7T fMRI 高质量

没有 NSD，很多工作不可能。

## 六、局限性

### 1. 被试特定

大部分方法需要**每被试 10 小时 +** 数据训练。跨被试 zero-shot 仍差。

MindEye2 把这降到 1 小时——仍远高于"即插即用"。

### 2. 只能重建见过的

fMRI 解码是**关联性**的——训练数据外的视觉概念难以解码。

### 3. 依赖自然图像先验

Stable Diffusion 的"自然图像先验"让重建看起来好——但可能**超过 fMRI 实际内容**。重建可能是 SD "脑补" 而非真实解码。

### 4. 延迟大

fMRI 慢（~1 s/scan），不能实时。

### 5. 语义 vs 视觉

fMRI 重建**高层语义（这是只狗）稳**，**低层视觉（毛色、姿态）弱**。

## 七、与 Tang 语义重建对比

**Tang 2023**（fMRI → 语言）用语言表达语义；**Takagi/MindEye**（fMRI → 图像）用图像表达视觉。

| | Tang | MindEye |
| --- | --- | --- |
| 输出 | 文本 | 图像 |
| 侧重 | 语义 | 视觉 |
| 脑区 | 分布式 | 视觉皮层 |
| 用途 | 听故事重建 | 看图片重建 |

未来可能**融合**——从 fMRI 重建**带描述的图像**。

## 八、脑-视频扩展

**MinD-Video（Chen 2023）** 把这一方法扩展到**视频**：

- fMRI 时间序列 → CLIP 序列 embedding
- 视频 diffusion 生成
- 重建"看过的视频"

详见 [脑-视频解码](脑-视频解码.md)。

## 九、应用前景

### 临床

- **视觉假体**：V1 皮层刺激让盲人"看到"——这是 **反向问题**（见 [视觉皮层假体](视觉皮层假体.md)）
- **失忆诊断**：比较健康人 vs 患者视觉重建质量

### 研究

- 神经科学：理解视觉皮层如何编码
- AI 研究：生成模型 × 生物视觉

### 消费（未来）

- 梦境记录（fMRI 需要便携化）
- 视觉辅助创作
- 情绪可视化

### 风险

- **隐私**：fMRI 可揭示想法
- **同意**：被动扫描是否合法？
- 见 [13 章伦理](../13_Ethics_Neurorights/index.md)

## 十、开源工具

- **[MindEye2](https://medarc-ai.github.io/mindeye2/)**：完整代码 + 预训练模型
- **NSD 数据集**：公开可下载
- **Stable Diffusion / SDXL**：Hugging Face

研究者可在 24 小时内复现论文结果——**低门槛促成快速进展**。

## 十一、逻辑链

1. **fMRI 具有高空间分辨率**，适合视觉皮层解码。
2. **CLIP + Stable Diffusion** 提供了从 fMRI → 图像的统一管道。
3. **Takagi 2023** 开启；**MindEye2 2024** 达到 human-level 识别。
4. **1 小时 fMRI 就能训练**——可扩展到更多被试。
5. **局限**：需被试特定训练、只能重建见过的、语义胜过细节。
6. **脑-视频扩展**已经开始；未来方向是融合文本 + 图像。

## 参考文献

- Takagi & Nishimoto (2023). *High-resolution image reconstruction with latent diffusion models from human brain activity.* CVPR. https://openaccess.thecvf.com/content/CVPR2023/html/Takagi_High-Resolution_Image_Reconstruction_With_Latent_Diffusion_Models_From_Human_Brain_CVPR_2023_paper.html
- Scotti et al. (2023). *Reconstructing the mind's eye: fMRI-to-image with contrastive learning and diffusion priors.* NeurIPS.
- Scotti et al. (2024). *MindEye2: Shared-subject models enable fMRI-to-image with 1 hour of data.* ICML. https://medarc-ai.github.io/mindeye2/
- Allen et al. (2022). *A massive 7T fMRI dataset to bridge cognitive neuroscience and artificial intelligence.* Nat Neurosci. — NSD
- Ozcelik & VanRullen (2023). *Brain-Diffuser: natural scene reconstruction from fMRI signals using generative latent diffusion.* Scientific Reports.
