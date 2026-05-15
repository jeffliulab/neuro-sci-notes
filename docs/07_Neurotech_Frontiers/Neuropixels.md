# Neuropixels — 高密度硅探针

> *Neuropixels(2017 IMEC + Allen + HHMI + UCL)是电生理革命:单根硅探针 ~ 1000 记录位点,数百 neuron 同时、多脑区贯穿。2.0(2021)~ 5000 位点 + 多 shank。彻底改变 systems neuroscience 数据规模 — 从几十 neuron 到上千。配合 Kilosort spike sorting。*
>
> **难度**:Advanced
> **前置知识**:[Action Potential](../02_Cellular_Molecular/Action_Potential.md)、[Research Methods](../00_Foundations/Research_Methods.md)

---

## 1. 为何革命

| | 传统(tetrode/Utah) | Neuropixels |
|---|---|---|
| 位点 | ~ 4-100 | 960 (1.0) / 5120 (2.0) |
| 同时 neuron | 10s | 100s-1000s |
| 脑区 | 单 | 贯穿多区(一探针) |
| 形态 | 粗 | 细长(< 100 μm 宽) |

---

## 2. 规格 (1.0 / 2.0)

| 参数 | 1.0 | 2.0 |
|---|---|---|
| 电极位点 | 960 | 5120 |
| 同时读 | 384 ch | 384 ch(可切换) |
| Shank | 1 | 1 或 4 |
| 长度 | 10 mm | 10 mm |
| 采样 | 30 kHz | 30 kHz |
| CMOS 集成 | 片上 amp + ADC + MUX | 更小更稳 |

---

## 3. CMOS 集成

- 探针即芯片:on-shank amplifier + ADC + multiplexer
- 解决"几千位点如何引线"难题(片上数字化 + 时分复用)
- 仅 ~ 数字线输出 → 可自由活动动物

---

## 4. Spike Sorting 必需

- 一位点录多 neuron + 一 neuron 被多位点录
- **Kilosort**(template matching + GPU)、MountainSort、SpyKING CIRCUS
- 高密度 → drift tracking(脑动)更关键
- 仍需人工 curation(假阳/合并)

---

## 5. PyTorch — 高密度模板匹配(简化)

```python
import torch

def template_match(traces, templates):
    """traces: (n_ch, T); templates: (n_units, n_ch, L). Detect spikes."""
    n_units = templates.shape[0]
    scores = []
    for u in range(n_units):
        tmpl = templates[u]                        # (n_ch, L)
        # Cross-correlate across channels (matched filter)
        sc = torch.nn.functional.conv1d(
            traces.unsqueeze(0), tmpl.unsqueeze(0)).squeeze()
        scores.append(sc)
    return torch.stack(scores)   # peaks = spike times per unit
```

---

## 6. 科学影响

- **大规模群体动力学**(见 [Neural Population Dynamics](../05_Computational_Neuroscience/Neural_Population_Dynamics.md))
- **多区同时**:跨脑区交互(如 IBL brain-wide map)
- **行为关联**:数千 neuron + naturalistic behavior
- **Brain-wide single-cell**:接近"全脑单细胞活动图"目标

---

## 7. International Brain Lab (IBL)

- 多实验室标准化 Neuropixels + 统一决策任务
- "Brain-wide map" of decision-making
- 开放数据(见 [Open Neuroscience](../00_Foundations/Open_Neuroscience.md))
- 可复制性 + 大规模典范

---

## 8. 局限

- 仍**侵入**(插入致组织损伤 + 胶质反应)
- 长期稳定性(慢性记录 drift、信号衰减)
- 数据量巨大(TB/实验 → 存储 + pipeline 挑战)
- Spike sorting 仍不完美(ground truth 难)
- 主要啮齿 / 灵长(人类仅术中有限)

---

## 9. 与其它高密度

- **Utah array**:皮层面阵(人类 BCI 用,如 BrainGate)
- **Neuropixels**:深插贯穿(动物为主)
- **Neuralink threads**:柔性 + 无线(见 [Neuralink](Neuralink.md))
- **NeuroGrid / 柔性 ECoG**:表面高密度

---

## 10. Common Pitfalls

### 10.1 非侵入

是侵入式插入探针;有组织损伤。

### 10.2 自动给出 neuron

需 spike sorting + curation;非即插即得。

### 10.3 一位点 = 一 neuron

一位点录多 neuron;一 neuron 跨多位点。

### 10.4 长期稳定

慢性 drift + 信号衰减;长期记录仍挑战。

### 10.5 已用于人类常规

人类仅术中 / 研究有限;主要动物。

---

## 11. Related Concepts

- **同节**:[Calcium Imaging](Calcium_Imaging.md)、[Neuralink](Neuralink.md)
- **计算**:[Neural Population Dynamics](../05_Computational_Neuroscience/Neural_Population_Dynamics.md)
- **基础**:[Research Methods](../00_Foundations/Research_Methods.md)、[Open Neuroscience](../00_Foundations/Open_Neuroscience.md)

---

## References

1. **Jun, J. J. et al.** "Fully integrated silicon probes for high-density recording of neural activity (Neuropixels)." *Nature*, 2017.
2. **Steinmetz, N. A. et al.** "Neuropixels 2.0: A miniaturized high-density probe for stable, long-term brain recordings." *Science*, 2021.
3. **Pachitariu, M. et al.** "Kilosort: realtime spike-sorting for extracellular electrophysiology." *bioRxiv*, 2016.
4. **International Brain Laboratory** "A brain-wide map of neural activity during complex behaviour." *bioRxiv*, 2023.
