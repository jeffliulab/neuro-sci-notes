# 钙成像 (Calcium Imaging)

> *Calcium imaging 用 fluorescent indicator (GCaMP family) 监测 Ca²⁺ 变化间接 reading neural activity。Action potential 触发 Ca²⁺ 内流 → GCaMP fluorescence ↑ → 显微镜记录。Tsien 2008 Nobel(GFP)。Look 1000+ neuron 同时 (cf. electrophysiology 几十)。系统神经科学主流工具 (Allen Brain Observatory)。*
>
> **难度**:Advanced
> **前置知识**:[Action_Potential](../02_Cellular_Molecular/Action_Potential.md)、[Ion_Channels](../02_Cellular_Molecular/Ion_Channels.md)

---

## 1. 原理

- AP → 电压依 Ca²⁺ channel 打开 → Ca²⁺ 内流(从 100 nM → 1 μM,10× 变化)
- Genetically encoded Ca²⁺ indicator (GECI):
  - GFP + calmodulin + M13 peptide
  - Ca²⁺ 结合 → 构象变 → fluorescence ↑
- 显微镜测 ΔF/F

---

## 2. GCaMP 家族

| 版本 | 年 | SNR | 速度 |
|---|---|---|---|
| GCaMP1 | 2001 | 弱 | 慢 |
| GCaMP3 | 2009 | 中 | 中 |
| GCaMP6f/s | 2013 | 高 | f=fast, s=slow |
| GCaMP7 | 2019 | 高 | 改善 |
| GCaMP8f/m/s | 2023 | 极高 | 接近 single spike |
| jGCaMP8 | 2023 | 高 | Ca²⁺ peak resolution |

---

## 3. 成像技术

### 3.1 Wide-field

- 一镜 CCD 拍整个视野
- 快、廉
- SNR 差 in vivo

### 3.2 Two-photon (2P)

- 红外双光子激发 → 减少 散射
- ~ 500 μm 深度
- 单 neuron 分辨率
- 主流 in vivo

### 3.3 Three-photon (3P)

- 更深 (~ 1 mm)
- 但 photon 利用率低

### 3.4 Light-sheet

- 速度极快(zebrafish 全脑 1 Hz)

### 3.5 Miniscope (head-mounted)

- 1g 小镜
- 自由 behaving mouse
- UCLA Miniscope 开源

---

## 4. Indicator 类型

- **GECI (genetically encoded)**: GCaMP、jRCaMP (red)
- **Synthetic dyes**: Oregon Green BAPTA、Fluo-4
  - 优:高 SNR
  - 缺:非特异 labeling、消退

GECI 用 viral 注射 + cell-type specific promoter(GCaMP6 Thy1, ChR2 Vgat 等)。

---

## 5. 数据 + Pipeline

```
显微镜 → tif stack (frame × time)
        ↓
Motion correction (sub-pixel)
        ↓
ROI detection (Suite2p, CaImAn)
        ↓
Trace extraction (ΔF/F)
        ↓
Spike inference (MLspike, CASCADE)
        ↓
Population analysis (PCA, dynamics)
```

---

## 6. PyTorch — Ca trace 简化

```python
import torch

def simulate_calcium_trace(spike_train, tau_decay=200, dt=33):
    """Spike train → Ca trace via exponential decay (Helmchen model)."""
    F = torch.zeros_like(spike_train, dtype=torch.float)
    F[0] = 0
    for t in range(1, len(spike_train)):
        F[t] = F[t-1] * torch.exp(torch.tensor(-dt / tau_decay))
        if spike_train[t]:
            F[t] += 1.0  # jump per AP
    return F
```

---

## 7. 限制

- **时间分辨率**: ~ 50-200 ms (慢于 spike)
- **Spike inference**: 困难,GCaMP nonlinear、saturation
- **Indicator buffer**: 改变 Ca dynamics
- **Bleaching**: 长 record 困
- **Z-axis**: 单平面,3D 慢
- **Cell type specificity**: 需 transgenic / virus

---

## 8. 与电生理比较

| | Electrophysiology | Ca imaging |
|---|---|---|
| 时间分辨 | μs | 50-200 ms |
| 神经元数 | 1-100 | 1000+ |
| Cell identity | 难 | 可 (cre-lines) |
| Invasive | 极 | 较 (craniotomy) |
| Long-term | 天-月 | 月+ |

→ Ca imaging trade-off:大量但慢。

---

## 9. 大规模实验

- **Allen Brain Observatory**: 数十万 neuron Ca recording open data
- **Janelia MesoScope**: 多 area 同时
- **OASIS / CaImAn / Suite2p**: 处理 pipeline 开源
- **DeepLabCut**: behavior tracking pair with imaging

---

## 10. Robot / AI 应用

- Decode behavior from Ca traces(类似 BCI)
- Predict behavior with RNN
- Mouse neural data → 训 RNN → 转移 robot

---

## 11. Common Pitfalls

### 11.1 Ca = Spike

不;Ca trace 间接,GCaMP saturation 后 失 linearity。

### 11.2 ΔF/F absolute

baseline F 偏 → ΔF/F 偏。

### 11.3 Motion correction 不必

肌动 / 心跳 → 重要 artifact。

### 11.4 ROI 自动准

Suite2p auto ROI 仍 50% noise;manual curation 必。

### 11.5 GCaMP express 中性

Long-term express 影响 cell health、Ca dynamics。

---

## 12. Related Concepts

- **同节**:[Optogenetics](Optogenetics.md)、[fMRI BOLD](fMRI_BOLD.md)、[Neuralink](Neuralink.md)
- **基础**:[Action Potential](../02_Cellular_Molecular/Action_Potential.md)
- **方法**:[Research Methods](../00_Foundations/Research_Methods.md)

---

## References

1. **Tian, L. et al.** "Imaging neural activity in worms, flies and mice with improved GCaMP calcium indicators." *Nat Methods*, 2009.
2. **Chen, T.-W. et al.** "Ultrasensitive fluorescent proteins for imaging neuronal activity." *Nature*, 2013.
3. **Pachitariu, M. et al.** "Suite2p: beyond 10,000 neurons with standard two-photon microscopy." *bioRxiv*, 2017.
4. **Helmchen, F. & Denk, W.** "Deep tissue two-photon microscopy." *Nat Methods*, 2005.
