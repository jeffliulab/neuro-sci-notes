# 光遗传学 (Optogenetics) — 用光控制神经元

> *Optogenetics (Deisseroth 2005) 让神经元表达**光敏 ion channel** (channelrhodopsin / halorhodopsin) → 光照触发激活 / 抑制 specific 神经元。比 electrical stimulation 更**精确 + 因果**。革命性,改变了 systems neuroscience。*
>
> **难度**:Advanced
> **前置知识**:[神经元](../02_Cellular_Molecular/Neuron.md)、[离子 channel](../02_Cellular_Molecular/Ion_Channels.md)

---

## 1. 关键 opsin

- **ChR2** (Channelrhodopsin-2): 蓝光 (470nm) → cation channel 开 → depolarize → 激活
- **NpHR** (Halorhodopsin): 黄光 → Cl- pump → hyperpolarize → 抑制
- **Arch** (Archaerhodopsin): 黄光 → proton pump → 抑制
- **C1V1, ChrimsonR**: 红光 (深穿)

---

## 2. 实验流程

1. 病毒 vector (AAV) 注入脑 → 表达 ChR2 in 特定 cell type
2. 植入 optical fiber
3. 给光 → 激活 / 抑制
4. 观察 behavior / 记录 neurons

→ Cell type specificity 通过 promoter (e.g. CaMKII for excitatory)。

---

## 3. 与传统对比

| 方法 | 时空精度 | Cell-type 特异 | 因果 |
|---|---|---|---|
| Electrical stimulation | 高 | ❌ (激活附近所有) | ✓ |
| Lesion | 中 | ❌ | ✓ |
| fMRI | 低 (空间 mm, 时间秒) | ❌ | ❌ |
| Drug | 慢 | 部分 | ✓ |
| **Optogenetics** | **高 + 高** | **✓** | **✓** |

---

## 4. 代表实验

### 4.1 Memory engram (Tonegawa 2012-2015)

激活 fear memory 在 DG → mouse 反复表达 freeze。
证明 memory 有 cellular substrate。

### 4.2 DA reward (Tsai 2009)

光激活 VTA DA → mouse 自发去激活区域 (place preference)。
证明 DA = reward。

### 4.3 Parkinson (Kravitz 2010)

激活 direct vs indirect pathway → 运动 vs 抑制。
证明 BG model 因果性。

### 4.4 Sleep (Adamantidis 2007)

激活 hypocretin → mouse wake up。

---

## 5. 应用

- **基础研究**:解析 specific cell types' 功能
- **临床探索** (Retinal degeneration 治疗试验)
- **AI training data**: precise neural manipulation 数据

---

## 6. 局限

### 6.1 侵入性

需要打开 skull + 注病毒。
非人类应用极受限。

### 6.2 时间长

mouse 表达 ChR2 需 2-4 周。

### 6.3 光透深度

蓝光只能透 ~ 1 mm 脑组织。
红光透 ~ 3-5 mm。
深部需 fiber + 光源 implant。

### 6.4 病毒 specificity

AAV 不完美 target;有 spillover。

---

## 7. PyTorch — Optogenetic Experiment 概念

```python
import torch

class Neuron:
    def __init__(self, has_ChR2=False):
        self.V = -70
        self.has_ChR2 = has_ChR2
        self.spike = False
    
    def step(self, I_input, blue_light=False):
        # Normal input
        self.V += 0.1 * (I_input - (self.V + 70) * 0.1)
        # ChR2 effect
        if self.has_ChR2 and blue_light:
            self.V += 5  # depolarize via cation inflow
        # Spike
        if self.V > -55:
            self.spike = True
            self.V = -75
        else:
            self.spike = False
        return self.spike
```

---

## 8. 现代演进

- **Soft optogenetics**: 红 / NIR + heat
- **2-photon optogenetics**: 单 cell 精度
- **Genetically encoded sensors** (GCaMP for Ca, iGluSnFR for Glu)
- **All-optical**: 同时 read + write
- **Sonogenetics**: 超声替代光

---

## 9. Common Pitfalls

### 9.1 ChR2 行为干扰

强 stimulation 可能引起非生理 firing。

### 9.2 Off-target effect

AAV 表达 ChR2 in glia / 其他 cell。

### 9.3 Light heating

强光 → 发热 → 影响 readout。

### 9.4 Specificity 不完美

Promoter 不是 100% specific。

### 9.5 翻译到 human

伦理 + 病毒 vectors 仍是 challenge。

---

## 10. Related Concepts

- **同节**:[fMRI BOLD](fMRI_BOLD.md)、[TMS](TMS.md)
- **细胞**:[离子 channel](../02_Cellular_Molecular/Ion_Channels.md)
- **应用**:[BCI Foundations](../06_Brain_Computer_Interface/01_Foundations/index.md)

---

## References

1. **Boyden, E. S. et al.** "Millisecond-timescale, genetically targeted optical control of neural activity." *Nat Neurosci*, 2005.
2. **Deisseroth, K.** "Optogenetics." *Nat Methods*, 2011.
3. **Tonegawa, S. et al.** "Memory engram cells." *Cell*, 2015.
4. **Kravitz, A. V. et al.** "Regulation of parkinsonian motor behaviors by optogenetic control of basal ganglia circuitry." *Nature*, 2010.
