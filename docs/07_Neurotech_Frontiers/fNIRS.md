# 功能近红外光谱 (fNIRS)

> *fNIRS 用近红外光(~ 700-900 nm)测皮层血氧变化(类 fMRI 但便携、廉价、抗运动)。基于 oxy/deoxy-Hb 吸收差。穿透 ~ 1-3 cm(仅皮层)。优势:自然场景、婴儿、可穿戴、BCI;局限:浅、空间分辨低。介于 EEG 与 fMRI 之间。*
>
> **难度**:Intermediate
> **前置知识**:[fMRI BOLD](fMRI_BOLD.md)、[Brain Energy Metabolism](../00_Foundations/Brain_Energy_Metabolism.md)

---

## 1. 原理

- 近红外"光学窗"(~ 700-900 nm):水/组织吸收低,Hb 吸收主导
- **oxy-Hb** 与 **deoxy-Hb** 吸收谱不同
- 神经活动 → 局部血流↑ → oxy-Hb↑ / deoxy-Hb↓(同 BOLD 基础)
- 光源 + 探测器对(~ 3 cm 间距)→ 香蕉形光路探皮层

---

## 2. 改良 Beer-Lambert 定律

$$\Delta A = \varepsilon \cdot \Delta c \cdot d \cdot \text{DPF}$$

- $\Delta A$:吸光度变化
- $\varepsilon$:消光系数(oxy/deoxy 不同)
- DPF:differential pathlength factor(散射致光程延长)
- 双波长 → 解 oxy/deoxy-Hb 浓度变化

---

## 3. 与 fMRI / EEG 对比

| | fNIRS | fMRI | EEG |
|---|---|---|---|
| 信号 | 血氧(光学) | 血氧(磁) | 电 |
| 时间 | ~ sec | ~ sec | ms |
| 空间 | cm(浅) | mm(全脑) | cm |
| 便携 | ✓ | ✗ | ✓ |
| 抗运动 | 较好 | 差 | 中 |
| 深部 | ✗(~ 3 cm) | ✓ | ✗ |
| 成本 | 低 | 极高 | 低 |

---

## 4. PyTorch — 双波长解 Hb

```python
import torch

def fnirs_hb(dA_lambda1, dA_lambda2, eps, dpf, d):
    """Solve Δ[HbO], Δ[HbR] from two wavelengths (modified Beer-Lambert)."""
    # eps: 2x2 extinction matrix [[e_HbO_l1, e_HbR_l1],[e_HbO_l2, e_HbR_l2]]
    dA = torch.tensor([dA_lambda1, dA_lambda2]) / (dpf * d)
    dC = torch.linalg.solve(eps, dA)   # [ΔHbO, ΔHbR]
    return dC
```

---

## 5. 应用优势场景

- **婴儿/儿童**:fMRI 难配合,fNIRS 可
- **自然行为**:行走、社交、运动(fMRI 不可)
- **床旁 / 临床**:新生儿监护、术中
- **BCI**:hemodynamic BCI(慢但抗噪)
- **Hyperscanning**:双人同测(社交神经科学)

---

## 6. 信号处理

- **运动伪迹**:虽抗运动优于 fMRI 仍需校正(spline、wavelet)
- **生理噪声**:心跳、呼吸、Mayer 波 → 滤波 / 短通道回归
- **Short-separation channel**:测头皮血流 → 回归去除浅层污染
- GLM(同 fMRI)分析

---

## 7. 局限

- 仅皮层(~ 1-3 cm)→ 深部不可及
- 空间分辨低(光散射)
- 头发 / 肤色影响信号(光衰减)— 设备公平性议题
- 浅层(头皮血流)污染
- 绝对定量难(DPF 个体差异)

---

## 8. 设备类型

- **CW**(continuous wave):最常见,测强度变化
- **Frequency-domain**:+ 相位 → 部分绝对量
- **Time-domain**:光子飞行时间 → 最佳深度区分(贵)
- **Wearable / 高密度 DOT**:接近图像化(diffuse optical tomography)

---

## 9. 与 AI

- BCI 解码(慢 hemodynamic + ML)
- 运动伪迹去除(深度学习)
- Hyperscanning 社交同步分析
- 可穿戴 + 自然场景 → 真实世界神经科学

---

## 10. Common Pitfalls

### 10.1 fNIRS = 便携 fMRI

仅皮层 + 空间低;非全脑等价。

### 10.2 测神经活动直接

间接(血氧),~ sec 延迟,同 BOLD 局限。

### 10.3 完全抗运动

优于 fMRI 但仍需运动校正。

### 10.4 信号纯脑源

头皮血流污染显著;需 short-channel 回归。

### 10.5 肤色/头发无影响

显著影响信号质量 → 设备包容性问题(近年关注)。

---

## 11. Related Concepts

- **同节**:[fMRI BOLD](fMRI_BOLD.md)、[EEG](EEG.md)
- **基础**:[Brain Energy Metabolism](../00_Foundations/Brain_Energy_Metabolism.md)
- **BCI**:[BCI 综述](../06_Brain_Computer_Interface/index.md)

---

## References

1. **Jöbsis, F. F.** "Noninvasive, infrared monitoring of cerebral and myocardial oxygen sufficiency." *Science*, 1977.
2. **Scholkmann, F. et al.** "A review on continuous wave functional NIRS." *NeuroImage*, 2014.
3. **Pinti, P. et al.** "The present and future use of functional near-infrared spectroscopy (fNIRS) for cognitive neuroscience." *Ann NY Acad Sci*, 2020.
4. **Kwasa, J. et al.** "Demographic reporting and phenotypic exclusion in fNIRS." *Front Neurosci*, 2023.
