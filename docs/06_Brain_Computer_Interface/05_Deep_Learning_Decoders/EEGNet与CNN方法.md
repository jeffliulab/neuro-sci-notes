# EEGNet 与 CNN 方法

**EEGNet（Lawhern et al. 2018, J Neural Eng）** 是 EEG BCI 领域最具代表性的深度学习架构。它把 CNN 引入 EEG，用**少量参数 + 精心结构设计**在多个 BCI 范式上达到 SOTA，成为**非侵入式 BCI 深度学习的基线**。

## 一、为什么 EEG 需要特殊 CNN

直接把 Computer Vision 的 ResNet/VGG 搬到 EEG 不行，原因：

1. EEG 是 **(channels, time)** 的 2D 信号，通道维度不具备图像的空间结构
2. 电极数少（8–64），数据维度 × 样本量 ≤ 常规 CV 的 1/1000
3. 需求：可解释、低延迟、小样本

EEGNet 用三个设计回应这些挑战。

## 二、EEGNet 架构

```
Input: (channels C, time T) — e.g. (64, 256)

① Temporal Conv 1D (depthwise on time axis):
   F1 filters, kernel = (1, 64)      # 学习频率选择器
   → (F1, C, T)

② Depthwise Conv (spatial):
   D=2 filters per F1, kernel = (C, 1)  # 学习空间滤波器（每个通道独立）
   → (F1*D, 1, T)

③ Separable Conv:
   kernel = (1, 16)                  # 时间尺度特征
   → (F2, 1, T/4)

④ Global average + Dense softmax
```

核心思想：
- **时间卷积学频带**
- **深度空间卷积学类 CSP 的空间模式**
- **可分离卷积降低参数**

总参数 **~2000**——比 ResNet 少 10000 倍。

## 三、关键设计选择

### Depthwise Separable

分离时间和空间——**参数减少 10×**，避免过拟合。

### Small receptive field

典型感受野 0.25–1 s，匹配 EEG 事件时标。

### BatchNorm + ELU

相对于 ReLU，ELU 在小数据 EEG 上更稳定。

### 正则化

- Dropout 0.5
- L2 regularization
- 数据增强：时间移位、噪声注入

## 四、在 BCI 任务上的表现

EEGNet 原始论文覆盖四个任务：

| 任务 | 数据集 | EEGNet | 基线 |
| --- | --- | --- | --- |
| P300 | BCI Competition III | **89.5%** | xDAWN+SWLDA 88% |
| ERP | MRCP | **91%** | FBCSP 89% |
| 运动想象 | BCI IV-2a | **69.9%** | FBCSP 67% |
| 错误感知 | Chavarriaga 2014 | **79%** | CCA 77% |

**在所有任务上与或超过传统方法**——首次证明通用 EEG CNN 可行。

## 五、EEGNet 的变种

### DeepConvNet / ShallowConvNet

**Schirrmeister et al. 2017 Hum Brain Mapp** 较早的 EEG CNN：
- DeepConvNet：较深 (4 conv blocks)
- ShallowConvNet：类 FBCSP 模拟（log + mean pooling）

### EEG-TCNet

**Ingolfsson 2020** 加入 TCN（时间卷积网络）处理长依赖。

### EEGSym

**Pérez-Velasco 2022** 利用大脑左右对称先验，性能再提升。

### BENDR（Transformer）

**Kostas 2021** 用对比预训练 + Transformer——进入基础模型时代。

## 六、侵入式 CNN：不同哲学

侵入式 BCI（spike + LFP）的 CNN 设计与 EEGNet 不同：

### QRNN / WaveNet 式

**Willett 2021 手写 BCI** 用 RNN + CNN 混合：
- 1D conv 在时间轴
- GRU 做序列建模
- CTC 输出字符

### LFP-CNN

**Eden-lab** 等用 1D CNN 直接从 LFP 原信号学特征，替代手工频带功率。

### Spike Tokenization

**NDT3、POYO** 把 spike 表示为离散 token 再用 Transformer（详见 [NDT系列与Transformer](NDT系列与Transformer.md)）——超越纯 CNN 范式。

## 七、EEGNet 的工程影响

### 开源与社区

- **Braindecode（Python）**：PyTorch EEGNet 参考实现
- **TorchEEG**：更现代的 EEG 深度学习框架
- Kaggle EEG 比赛中 EEGNet 是标配 baseline

### 教学基线

几乎所有 EEG 深度学习论文都把 EEGNet 作为基线对比。这让 EEGNet 成为"最后的经典 CNN"——就像 ResNet 之于 CV。

## 八、与 Transformer / Foundation Model 的比较

2023 后 EEG 基础模型兴起（BENDR、EEGPT、LaBraM），在多个数据集上预训练后表现超过 EEGNet。但：

- EEGNet 仍是**单 session、少数据**场景的最佳选择
- EEGNet 训练一个模型几分钟，Transformer 基础模型需 GPU 数天
- EEGNet 参数少易部署到嵌入式设备（消费级 BCI）

**分层使用**：嵌入式 / 消费级 / 实时延迟敏感 用 EEGNet；研究 / 大数据 / 跨被试迁移 用 Transformer。

## 九、CNN 的可解释性

EEGNet 的 depthwise 空间滤波器可视化后近似 CSP 滤波器——**深度网络自发学到了神经科学家手工设计的特征**。

**Grad-CAM / Integrated Gradients** 可定位哪个时间窗、哪个频带对分类最重要。这让 EEGNet 在临床场景（需要可解释）仍被保留。

## 十、逻辑链

1. **EEG BCI 的小数据高维特性**要求特殊 CNN 设计，不能直接套 CV 架构。
2. **EEGNet 用时间-空间可分离卷积**实现参数极简化，~2000 参数达到 SOTA。
3. **EEGNet 是 EEG 深度学习的基线**——所有后续 EEG 论文必须与之比较。
4. **侵入式 BCI 的 CNN 设计不同**：CTC + 1D conv 用于手写、Transformer-based 用于语音。
5. **Transformer 基础模型超过 EEGNet**，但 EEGNet 仍在资源受限场景占优。

## 参考文献

- Lawhern et al. (2018). *EEGNet: a compact convolutional neural network for EEG-based brain-computer interfaces.* J Neural Eng. https://iopscience.iop.org/article/10.1088/1741-2552/aace8c
- Schirrmeister et al. (2017). *Deep learning with convolutional neural networks for EEG decoding and visualization.* Hum Brain Mapp.
- Willett et al. (2021). *High-performance brain-to-text communication via handwriting.* Nature. — RNN+CNN 手写
- Kostas et al. (2021). *BENDR: using transformers and a contrastive self-supervised learning task to learn from massive amounts of EEG data.* Front Hum Neurosci.
- Braindecode: https://braindecode.org/
