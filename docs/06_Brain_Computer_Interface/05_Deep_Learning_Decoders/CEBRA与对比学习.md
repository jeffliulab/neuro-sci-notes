# CEBRA 与对比学习

**CEBRA（Consistent EmBeddings of high-dimensional Recordings using Auxiliary variables）** 由 Steffen Schneider、Jin Hwa Lee 和 Mackenzie Mathis 在 2023 年 Nature 上提出，是**对比学习方法在神经解码上的代表作**。它用**行为约束 + 对比目标**学习"与行为对齐的神经潜空间"。

## 一、核心问题

LFADS、NDT 等方法学的潜空间是**数据驱动的**——结构好但不一定"和行为对齐"。

例如：LFADS 的潜轨迹可能**同时编码运动和感觉反馈**，解码运动时被感觉噪声污染。

CEBRA 的问题：**如何让神经潜空间的几何结构与行为变量（运动方向、奖励、视觉刺激）显式对齐？**

## 二、CEBRA 的对比学习目标

给一组 (neural, behavior) 数据 $(x_t, y_t)$，CEBRA 训练编码器 $f_\theta: x_t \mapsto z_t$ 满足：

- **行为相似 → 潜空间相近**
- **行为不同 → 潜空间相远**

### InfoNCE 目标

$$\mathcal{L} = -\log \frac{\exp(z_t \cdot z_t^+ / \tau)}{\exp(z_t \cdot z_t^+ / \tau) + \sum_{i} \exp(z_t \cdot z_i^- / \tau)}$$

其中：
- $z_t^+$ 是"行为相似"的正样本（例如同一运动方向的邻近时间）
- $z_i^-$ 是负样本
- $\tau$ 是温度

### 三种训练模式

1. **Discrete**：行为是离散类别（如方向 8 bins）——正样本 = 同类
2. **Time-continuous**：行为连续时序——正样本 = 时间邻近
3. **Mixed**：行为 + 时间双约束

## 三、CEBRA 的独特性

### 与 LFADS 比较

| | LFADS | CEBRA |
| --- | --- | --- |
| 目标 | 重构神经活动 | 对齐行为 |
| 行为变量 | 不用 | 核心输入 |
| 潜空间结构 | 动力学驱动 | 行为驱动 |
| 解码 | 后 hoc 线性 | 直接用潜空间 |

### 与 t-SNE / UMAP 比较

CEBRA 是**参数化非线性降维**——训练后可以对新数据编码，而 t-SNE/UMAP 不行。这让它适合 **在线 BCI 应用**。

## 四、里程碑实验

**Schneider et al. 2023 Nature**：

### 视觉皮层场景重建

- 鼠 V1 spike → CEBRA → 潜空间
- 潜空间直接解码看过的**自然视频帧**（线性解码器）
- 准确率超过 t-SNE + KNN 显著

### 运动皮层跨被试

- 多个猴子的 M1 数据
- CEBRA 统一训练
- 一个被试学到的解码器可迁移到另一被试

### 海马位置编码

- 鼠海马 spike
- CEBRA 恢复**已知位置-细胞结构**
- 与神经科学共识一致

## 五、数学性质

### 保持拓扑

CEBRA 潜空间的**度量结构与行为空间近似同构**——这是可解释 BCI 的强性质。

### 小样本鲁棒性

因为有行为监督，CEBRA 在 **100–1000 试次** 水平上也能工作，而纯重构模型（LFADS）需要更多数据。

### 跨被试一致

用同样行为变量训练的不同被试 CEBRA 潜空间**几何近似**——让跨被试迁移自然发生。

## 六、在 BCI 上的应用场景

### 场景 1：实时解码

训练 CEBRA 后，把 $z_t = f_\theta(x_t)$ 作为特征送入线性解码器（速度/位置）——性能常优于卡尔曼、LFADS。

### 场景 2：跨 session 对齐

不同 session 电极通道可能不同，CEBRA 让**潜空间作为统一表征**，解码器在潜空间训练后无需重新校准。

### 场景 3：可解释可视化

CEBRA 3D 潜空间可直接可视化运动轨迹——医生/工程师能肉眼看到"神经状态走向目标"。

### 场景 4：行为标签缺失补全

**CEBRA-Behavior + CEBRA-Time** 联合训练：少量标注 + 大量无标注数据。

## 七、与 LLM 时代的连接

CEBRA 的"对比学习 + 模态对齐"精神与 **CLIP** 一致：
- CLIP：图像 ↔ 文本
- CEBRA：神经 ↔ 行为

这让 **"神经 embedding 能像 CLIP embedding 一样被 LLM 使用"** 成为可能：
- 把神经 embedding 作为 LLM 的软提示（soft prompt）
- 在潜空间做**神经-语言-图像**多模态对齐

2024 后的 **neural-to-language** 工作（如 MindEye2）正走这条路。

## 八、CEBRA 的实现与工具

- **[cebra.ai](https://cebra.ai/)**：PyTorch 官方库
- **一键 pip install**：接口类 scikit-learn
- **GPU 加速**：训练一个模型 5–30 分钟
- **多种模型**：cebra-time、cebra-behavior、cebra-hybrid

## 九、局限与批评

1. **需要行为标签**：纯无监督场景不适用
2. **对标签噪声敏感**：行为标注不准会污染潜空间
3. **假设行为与神经同步**：hemodynamic 延迟等会破坏对齐
4. **不建模动力学**：不像 LFADS 有显式的演化结构

应对：**CEBRA + LFADS 混合架构**——先 LFADS 学动力学潜空间，再 CEBRA 对齐行为。

## 十、逻辑链

1. **数据驱动的潜空间（LFADS）不一定和行为对齐**。
2. **CEBRA 用对比学习 + 行为约束**显式构造行为对齐的潜空间。
3. **InfoNCE 目标**让相似行为的神经活动在潜空间靠近。
4. **CEBRA 在跨被试、跨任务迁移上表现突出**，是神经"CLIP"。
5. **CEBRA + LLM** 的多模态对齐路线是 2024 后神经解码的新前沿。

## 参考文献

- Schneider, Lee & Mathis (2023). *Learnable latent embeddings for joint behavioural and neural analysis.* Nature. https://www.nature.com/articles/s41586-023-06031-6
- Chen et al. (2020). *A simple framework for contrastive learning of visual representations.* ICML. — SimCLR
- Radford et al. (2021). *Learning transferable visual models from natural language supervision.* ICML. — CLIP
- Oord et al. (2018). *Representation learning with contrastive predictive coding.* arXiv. — InfoNCE
- CEBRA: https://cebra.ai/
