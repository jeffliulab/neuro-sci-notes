# 开源工具：MNE、EEGLAB、CEBRA

**BCI 研究的开源生态**日趋成熟。从**信号处理**（MNE-Python、EEGLAB）到**深度学习解码器**（Braindecode、NDT、POYO）再到**对比学习框架**（CEBRA），开源工具让任何研究者可在**24 小时内**复现前沿论文。

## 一、信号处理

### 1. MNE-Python

**最主流**开源 EEG/MEG 处理包。

#### 核心

- Python + NumPy 基础
- 整个 MEG/EEG 工作流
- **活跃开发 15+ 年**

#### 功能

- 滤波、去噪
- ICA / artifact 去除
- Source localization
- 统计分析
- **KaTeX** 文档丰富

#### 安装

```bash
pip install mne
```

#### 学习资源

- mne.tools 官网
- 50+ tutorial
- Harvard 神经影像课程使用

### 2. EEGLAB

**MATLAB** 生态的 EEG 标准。

#### 核心

- MATLAB GUI 驱动
- **学术界广泛**使用
- 几十个 plugins

#### 优势

- 可视化**强**
- 标准算法**完备**
- 老派神经科学家首选

#### 劣势

- MATLAB license
- Python 整合弱
- **迁移到 MNE** 趋势

### 3. Brainstorm

- MATLAB/Python 混合
- MEG 强
- McGill + USC 开发

### 4. FieldTrip

- MATLAB
- 荷兰 Donders 研究所
- 主攻 MEG

### 5. BCILAB

- EEGLAB 的 BCI 扩展
- 实时 BCI 支持

## 二、Spike 处理

### 1. Kilosort

**Neuropixels 时代**标准。

#### 开发

- **Pachitariu, Stringer, Carandini** (UCL)
- Kilosort 4 最新版

#### 功能

- GPU 加速 spike sorting
- 模板匹配
- 自动化

#### 使用

- 学术研究 90%+
- 开源 + 文档

### 2. Wave_Clus

- 经典 spike sorting
- Scala 实验室

### 3. MountainSort

- 自动化
- 大数据

### 4. SpikeInterface

- **统一接口**多个 sorter
- Python，BrainBox 等整合

## 三、深度学习解码器

### 1. Braindecode

**EEG 深度学习标准库**。

#### 核心

- PyTorch 基础
- EEGNet、DeepConvNet、Shallow
- 预处理 + 训练 + 评估

#### 使用

```python
from braindecode.models import EEGNetv4
model = EEGNetv4(n_chans=22, n_outputs=4, n_times=1125)
```

### 2. NDT（Neural Data Transformer）

**NDT1/2/3** 开源于 GitHub。

#### 使用

```python
from ndt3 import NDT3
model = NDT3.from_pretrained("snel-repo/ndt3-base")
```

### 3. POYO

**POYO（Azabou 2023）** 基础模型。

- 多被试预训练
- Fine-tune on new subject
- GitHub: https://github.com/poyo-brain

### 4. CEBRA

见 [CEBRA 与对比学习](../05_Deep_Learning_Decoders/CEBRA与对比学习.md)。

#### 使用

```python
import cebra
cebra_model = cebra.CEBRA(model_architecture='offset10-model', 
                          batch_size=512,
                          learning_rate=3e-4,
                          output_dimension=8,
                          max_iterations=20000)
cebra_model.fit(neural_data, behavior)
embeddings = cebra_model.transform(neural_data)
```

- MPI-IS Tübingen 开发
- **Nature 2023 开源**
- 文档 + tutorial 完备

## 四、脑-图像重建

### 1. MindEye / MindEye2

- Ryan Scotti et al.
- **MedARC** 团队开源
- 预训练权重 HuggingFace

### 2. Brain-Diffuser

- Ozcelik 2023 开源
- fMRI → SD 通路

### 3. NeuroImagen

- 学术组件
- 多模型比较

## 五、脑-语言

### 1. Willett 2023 Code

- Stanford NPL 开源
- PyTorch RNN + CTC
- GitHub

### 2. DeWave

- EEG → 文本
- 非侵入 baseline

### 3. EEGPT / LaBraM

- EEG 基础模型
- 类 GPT 预训练
- 中国实验室开源

## 六、数据存储 + 可视化

### 1. NWB（Neurodata Without Borders）

- HDF5 基础
- 跨实验室数据标准
- PyNWB 接口

### 2. DANDI

- NWB 数据仓库
- 100+ 数据集

### 3. Neo

- Python 电生理 I/O
- 多格式支持

### 4. Plotly / Bokeh

- 交互可视化
- 脑图投影

### 5. Nilearn

- 脑影像 ML
- fMRI 分析

## 七、实时 BCI 系统

### 1. BCI2000

- **经典**实时 BCI 平台
- Wadsworth 中心
- C++ + 模块化

### 2. OpenBCI GUI

- 开源硬件 + 软件
- 消费 EEG

### 3. LSL（Lab Streaming Layer）

- 实时数据流协议
- 跨设备同步

### 4. OpenViBE

- 图形化 BCI 编程
- EU 项目

## 八、机器学习框架

### 通用

- **PyTorch**：BCI 深度学习标准
- **TensorFlow**：EEGNet 等原生
- **JAX**：高性能研究

### 专用

- **scikit-learn**：经典 ML
- **Riemannian**：pyRiemann
- **CSP**：MNE + scikit

## 九、云端协作

### 1. HuggingFace

- 预训练模型共享
- CEBRA、MindEye、NDT 等都在

### 2. Weights & Biases

- 实验跟踪
- BCI 研究广用

### 3. Neptune

- 另一实验跟踪
- 类似 W&B

### 4. Colab / Kaggle

- 免费 GPU
- BCI 入门

## 十、教程与资源

### 书籍

- **"Brain Signal Analysis" (Sanei & Chambers)**
- **"Neural Engineering" (He)**
- **"Neurotechnology and Brain Machine Interface" (Kao & others)**

### 课程

- **NMA（Neuromatch Academy）**：免费线上，多国
- **Stanford CME 290**：BCI 课程
- **Coursera / edX** 多个课程

### 文档

- MNE tutorials
- CEBRA paper + GitHub
- ML for BCI review

## 十一、完整 pipeline 示例

### EEG 分类任务

```python
import mne
from braindecode.models import EEGNetv4
import torch

# 1. 加载 + 预处理
raw = mne.io.read_raw_edf("data.edf")
raw.filter(1, 40)
epochs = mne.Epochs(raw, events, tmin=0, tmax=2)

# 2. ICA 去 artifact
ica = mne.preprocessing.ICA(n_components=20)
ica.fit(raw)
ica.apply(raw)

# 3. 提取数据
X = epochs.get_data()
y = epochs.events[:, -1]

# 4. 训练 EEGNet
model = EEGNetv4(n_chans=22, n_outputs=4, n_times=501)
# ... 训练代码
```

**15 行代码** 完成端到端。

## 十二、评估与对比

### 标准数据集

- **PhysioNet BCI IV 2a**（EEG 运动想象）
- **CHB-MIT**（EEG 癫痫）
- **NSD**（fMRI 图像）
- **NLB / FALCON**

### 标准指标

- Classification accuracy
- R² for continuous
- WER for speech
- Kappa

## 十三、开源文化

### BCI 特殊性

- 硬件高门槛
- 数据隐私
- 但**算法可开源**

### 社区

- **BCI Society** 年会
- **NeurIPS / NAT NEURO** 论文
- Twitter / 微博 BCI 账号

### 贡献

- 学生 → 实习 → 工作
- 开源是**进入门票**

## 十四、未来

### 1. BCI × LLM 整合

- 开源 LLM + 开源 BCI
- **神经增强输入**研究

### 2. 实时 AI

- Edge LLM
- ONNX 加速

### 3. 联邦学习

- 跨医院
- HIPAA 合规

### 4. 无代码

- 点击即用
- 民主化研究

## 十五、逻辑链

1. **MNE + EEGLAB** 是信号处理基础工具。
2. **Kilosort** 是 spike sorting 标准。
3. **Braindecode + NDT + POYO + CEBRA** 是深度学习解码器生态。
4. **MindEye + DeWave** 是脑-图像/语言具体应用。
5. **NWB + DANDI** 是数据标准 + 仓库。
6. **BCI2000 + LSL + OpenBCI** 是实时系统。
7. **开源文化 + 教程 + 社区** 让 BCI 进入门槛前所未有低。

## 参考文献

- Gramfort et al. (2013). *MEG and EEG data analysis with MNE-Python.* Front Neurosci.
- Delorme & Makeig (2004). *EEGLAB: an open source toolbox for analysis of single-trial EEG dynamics.* J Neurosci Methods.
- Pachitariu et al. (2024). *Kilosort 4.* Nat Methods.
- Schirrmeister et al. (2017). *Deep learning with convolutional neural networks for EEG decoding and visualization.* Hum Brain Mapp.
- Schneider et al. (2023). *Learnable latent embeddings for joint behavioral and neural analysis.* Nature.
