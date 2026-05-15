# EEG — 头皮脑电

> *Electroencephalography (EEG) 是最常用、最便宜的 brain imaging 技术。Berger 1924 首录 human EEG。非 invasive、ms 时间分辨率、cm 空间分辨率。临床用于 epilepsy diagnosis、sleep staging、coma assessment。Consumer EEG (Emotiv、Muse、Neurosky) 兴起。机器学习 (CNN、Transformer) 改善 decoding。BCI 主流 modality 之一。*
>
> **难度**:Intermediate
> **前置知识**:[Action_Potential](../02_Cellular_Molecular/Action_Potential.md)、[Cortex](../01_Neuroanatomy/Cortex.md)

---

## 1. 物理原理

- Pyramidal neuron 同步 postsynaptic potential 产生 dipole
- 万亿 neuron 同步 → 头皮检测
- 主信号是 EPSP/IPSP,不是 spike
- EEG ~ μV 量级,需 大增益放大器

---

## 2. 频段 (Rhythms)

| 名 | Hz | 关联 |
|---|---|---|
| Delta (δ) | 0.5-4 | 深睡眠、infant |
| Theta (θ) | 4-8 | 浅睡、海马、深思 |
| Alpha (α) | 8-13 | 放松闭眼(枕叶) |
| Beta (β) | 13-30 | 觉醒、active 思考 |
| Gamma (γ) | 30-100 | binding、attention、意识假说 |

---

## 3. 10-20 系统

国际标准 21 通道 placement:
- 字母:F (frontal)、C (central)、P (parietal)、O (occipital)、T (temporal)
- 数字:奇数左、偶数右
- 现代:64、128、256 channel

---

## 4. EEG vs MEG vs fMRI

| 技术 | 时间 | 空间 | 成本 |
|---|---|---|---|
| EEG | ms | cm | $1K-100K |
| MEG | ms | cm (好于 EEG) | $1M+ |
| fMRI | sec | mm | $1M-3M |
| fNIRS | sec | cm | $100K |

EEG 最便宜、最广。

---

## 5. 临床应用

- **Epilepsy diagnosis**: spike-wave discharge
- **Sleep staging**: NREM 1-3 + REM (PSG)
- **Coma / brain death**: 平直 EEG
- **Anesthesia depth**: BIS monitor
- **ICU monitoring**: 持续 EEG (cEEG)

---

## 6. ERP (Event-Related Potential)

- 任务 trigger 后 averaged EEG
- 几个 component:
  - **P300**: ~ 300 ms 后 oddball
  - **N400**: ~ 400 ms semantic
  - **N170**: 面部 / 字
  - **MMN** (Mismatch Negativity)
- BCI P300 speller 用此

---

## 7. PyTorch — EEG CNN

```python
import torch
import torch.nn as nn

class EEGNet(nn.Module):
    """EEGNet (Lawhern 2018) — 经典 EEG CNN."""
    def __init__(self, n_channels=64, n_classes=4, n_samples=128):
        super().__init__()
        self.conv1 = nn.Conv2d(1, 8, (1, 64), padding='same')
        self.bn1 = nn.BatchNorm2d(8)
        self.depthwise = nn.Conv2d(8, 16, (n_channels, 1), groups=8)
        self.bn2 = nn.BatchNorm2d(16)
        self.pool1 = nn.AvgPool2d((1, 4))
        self.sep_conv = nn.Conv2d(16, 16, (1, 16), padding='same', groups=16)
        self.pool2 = nn.AvgPool2d((1, 8))
        self.fc = nn.Linear(16 * (n_samples // 32), n_classes)
    
    def forward(self, x):
        x = self.bn1(self.conv1(x))
        x = self.bn2(self.depthwise(x))
        x = self.pool1(torch.relu(x))
        x = self.sep_conv(x)
        x = self.pool2(torch.relu(x))
        return self.fc(x.flatten(1))
```

---

## 8. 常见 BCI 范式

- **Motor imagery**: 想象动 → 控 cursor / arm
- **P300 speller**: 矩阵闪 → P300 → 选字
- **SSVEP**: 视觉刺激 → V1 相同频率 → 选项
- **Cognitive workload**: 评 alertness

---

## 9. Pitfall: Artifact

- **EMG**: 肌电(咬牙、瞬目)
- **EOG**: 眼动
- **Movement**: 头动
- **Power line** (50/60 Hz noise)
- ICA(独立分量分析)清除

---

## 10. 消费 EEG

- **Muse** (Interaxon): 4 channel,冥想
- **Emotiv EPOC / Insight**: 14 / 5 channel
- **Neurosky MindWave**: 1 channel
- **OpenBCI** (开源)
- **Neurable**: AR/VR
- 但精度有限,科研有局限

---

## 11. Common Pitfalls

### 11.1 空间精度 high

错;EEG 头皮信号是 mm² 区 sum,不是 single neuron。

### 11.2 EEG = thoughts

只能 decode 一些 motor intent、attention、emotion 类别,非"思想读取"。

### 11.3 Gamma = 意识

争议大;gamma 关联 attention,但 NCC 仍 open。

### 11.4 Single trial 可靠

Single trial SNR 差;通常 trial averaging 才看 ERP。

### 11.5 Consumer EEG = research

精度差、channel 少;严肃研究用 64+ 临床级。

---

## 12. Related Concepts

- **同节**:[fMRI BOLD](fMRI_BOLD.md)、[TMS](TMS.md)、[Neuralink](Neuralink.md)
- **BCI**:[BCI 综述](../06_Brain_Computer_Interface/index.md)
- **AI**: CNN / Transformer for EEG

---

## References

1. **Berger, H.** "Über das Elektrenkephalogramm des Menschen." *Arch Psychiatr Nervenkr*, 1929.
2. **Niedermeyer, E. & da Silva, F. L.** *Electroencephalography*. 5th ed., 2005.
3. **Lawhern, V. J. et al.** "EEGNet: A compact convolutional neural network for EEG-based brain-computer interfaces." *J Neural Eng*, 2018.
4. **Cohen, M. X.** *Analyzing Neural Time Series Data*. MIT Press, 2014.
