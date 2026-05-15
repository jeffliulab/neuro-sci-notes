# 开放神经科学 (Open Neuroscience)

> *开放科学运动重塑 neuroscience:开放数据(OpenNeuro、DANDI、Allen Brain)、开放工具(MNE、Suite2p、DeepLabCut)、开放标准(BIDS、NWB)、预注册、preprint(bioRxiv)。回应复制危机 + 大数据时代。AI 时代尤其关键:大模型需大规模标准化神经数据。*
>
> **难度**:Beginner
> **前置知识**:[Research Methods](Research_Methods.md)、[Statistics in Neuroscience](Statistics_in_Neuroscience.md)

---

## 1. 为何开放

- **复制危机**回应(透明 → 可验证)
- **大数据**:单 lab 无法采集全部
- **AI**:训练模型需大规模标准数据
- **效率**:不重复造轮子
- **公平**:资源不足者也可研究

---

## 2. 开放数据库

| 库 | 内容 |
|---|---|
| **OpenNeuro** | fMRI/EEG/MEG(BIDS 格式) |
| **DANDI** | 电生理 / 光学(NWB) |
| **Allen Brain Atlas** | 基因表达、connectivity、Ca imaging |
| **Human Connectome Project** | 健康 connectome |
| **UK Biobank** | 50万人影像 + 基因 + 健康 |
| **IBL** (Int'l Brain Lab) | 标准化决策任务 + Neuropixels |
| **DABI** | 侵入式人类数据 |
| **NeuroMorpho** | 神经元形态 |

---

## 3. 数据标准

- **BIDS** (Brain Imaging Data Structure):文件组织标准
- **NWB** (Neurodata Without Borders):电生理 / 光学统一格式
- 标准化 → 工具互操作 + 数据复用

---

## 4. 开放工具

| 工具 | 用途 |
|---|---|
| **MNE-Python** | EEG/MEG 分析 |
| **EEGLAB** | EEG(MATLAB) |
| **Suite2p / CaImAn** | 钙成像 pipeline |
| **DeepLabCut / SLEAP** | 行为追踪 |
| **SpikeInterface** | spike sorting 统一 |
| **fMRIPrep** | fMRI 预处理标准 |
| **Nilearn** | fMRI 机器学习 |
| **Brian2 / NEST** | SNN 仿真 |

---

## 5. 预注册 + Registered Reports

- **Preregistration**:分析前锁定假设 + 方法
- **Registered Reports**:方法先 peer review → 原则上接受(结果不影响发表)
- 解决 p-hacking + 发表偏倚

---

## 6. Preprint

- **bioRxiv**(2013 起)、**arXiv** q-bio
- 快速 + 免费传播
- COVID 加速接受
- 但需注意未 peer-review

---

## 7. PyTorch — 加载 NWB 数据示例

```python
# pip install pynwb
from pynwb import NWBHDF5IO

with NWBHDF5IO('session.nwb', 'r') as io:
    nwb = io.read()
    spikes = nwb.units['spike_times'][:]      # 标准化 spike data
    behavior = nwb.acquisition['position'].data[:]
# 同一代码可读任何 NWB 数据集 → 互操作性
```

---

## 8. 大科学项目

- **BRAIN Initiative** (US, 2013):tools + cell census
- **Human Brain Project** (EU, 2013-2023):simulation + infrastructure
- **China Brain Project**、**Japan Brain/MINDS**
- **MICrONS**:cortex connectome + 功能
- **Allen Institute**:开放数据典范

---

## 9. AI × Open Neuro

- 大规模标准数据 → 训 foundation model(neural data)
- POYO、Neuroformer 等"neural data Transformer"
- 需 NWB/BIDS 才能 scale
- 类似 NLP 需 Common Crawl

---

## 10. 挑战

- **隐私**:人类脑数据 = 敏感(见 [Neuroethics](Neuroscience_Ethics.md))
- **激励**:分享数据缺职业奖励
- **质量**:开放 ≠ 高质量(需 curation)
- **存储**:EM/connectome PB-EB 级

---

## 11. Common Pitfalls

### 11.1 开放数据 = 高质量

需 curation + 元数据;垃圾进垃圾出。

### 11.2 Preprint = peer-reviewed

未审稿;批判性阅读。

### 11.3 预注册扼杀探索

探索性分析仍允许,只需标注(非 confirmatory)。

### 11.4 标准格式可选

无 BIDS/NWB → 数据复用 + 工具互操作崩溃。

### 11.5 大数据自动 = 好科学

无良好问题 + 行为,数据量无用(见 [Behavioral Neuroscience](Behavioral_Neuroscience.md))。

---

## 12. Related Concepts

- **同节**:[Research Methods](Research_Methods.md)、[Statistics in Neuroscience](Statistics_in_Neuroscience.md)、[Neuroscience Ethics](Neuroscience_Ethics.md)
- **前沿**:[Calcium Imaging](../07_Neurotech_Frontiers/Calcium_Imaging.md)、[EEG](../07_Neurotech_Frontiers/EEG.md)
- **AI**: foundation models、data standardization

---

## References

1. **Gorgolewski, K. J. et al.** "The brain imaging data structure (BIDS)." *Sci Data*, 2016.
2. **Rübel, O. et al.** "The Neurodata Without Borders ecosystem for neurophysiological data science." *eLife*, 2022.
3. **International Brain Laboratory** "Standardized and reproducible measurement of decision-making in mice." *eLife*, 2021.
4. **Poldrack, R. A. & Gorgolewski, K. J.** "Making big data open: data sharing in neuroimaging." *Nat Neurosci*, 2014.
