# fMRI 与 BOLD 信号

> *fMRI (functional MRI) 是现代认知神经科学最主流工具。利用 **BOLD (Blood-Oxygen-Level Dependent) signal** 间接测脑活动。1992 起 boom,Logothetis 等揭示 BOLD ↔ neural activity 关系。*
>
> **难度**:Intermediate
> **前置知识**:[Cortex](../01_Neuroanatomy/Cortex.md)

---

## 1. fMRI 原理

### 1.1 MRI 基础

强磁场 + RF pulse → 测组织 magnetization 弛豫。
不同组织 / 状态 contrast 不同。

### 1.2 BOLD Signal

神经活动 → 局部需氧 ↑ → 血流增 → **过量供氧** → 局部 deoxyhemoglobin ↓ → MRI T2* 信号增。

→ BOLD 是 **vascular 信号**,间接反映 neural activity。

---

## 2. 时空分辨率

- **空间**: 1-3 mm (typical), 0.5 mm (7T)
- **时间**: 1-3 sec (Hemodynamic response 5-7 sec 高峰)

→ 比 EEG/MEG 空间好,时间差。

---

## 3. BOLD ↔ Neural Activity

Logothetis 2001 同时测:
- **LFP (local field potential)** ↔ BOLD 相关 r > 0.8
- **MUA (multi-unit activity)** ↔ BOLD r ~ 0.5
- BOLD 主要反映 **input + processing** (synaptic activity),不是 output spikes

---

## 4. 主流 paradigms

### 4.1 Block design

任务 A vs B 交替 30s blocks → compare averages。

### 4.2 Event-related

单独 trials → reconstruct hemodynamic response。

### 4.3 Resting-state

静息 → 测 networks (DMN, salience, etc.)。

### 4.4 Task-based

特定认知任务 (Stroop, n-back, etc.)。

### 4.5 MVPA (Multi-Voxel Pattern Analysis)

不只 averaged activation,decoding voxel patterns。

---

## 5. 分析流程

```
Raw scans
   ↓
Motion correction
   ↓
Spatial normalization (MNI / Talairach)
   ↓
Spatial smoothing (8 mm FWHM Gaussian)
   ↓
GLM (General Linear Model) fitting
   ↓
Statistical maps (t-test / F-test per voxel)
   ↓
Multiple comparison correction (FWE / FDR)
   ↓
Visualization
```

---

## 6. 工具

- **SPM** (London)
- **FSL** (Oxford)
- **AFNI** (NIMH)
- **fMRIPrep** (preprocessing pipeline)
- **Nilearn** (Python)

---

## 7. PyTorch / Python — 简化 GLM

```python
import numpy as np

def hrf(t, peak=6, undershoot=16):
    """Canonical hemodynamic response function."""
    return t**peak * np.exp(-t) - 0.35 * t**undershoot * np.exp(-t) / 50

def fit_glm(voxel_timeseries, task_design):
    """Fit BOLD = β * (task * hrf) + noise."""
    # Convolve task with HRF
    t = np.arange(0, 30, 1)
    hrf_canonical = hrf(t)
    predicted = np.convolve(task_design, hrf_canonical, mode='same')
    # OLS
    X = np.column_stack([predicted, np.ones(len(predicted))])
    beta, *_ = np.linalg.lstsq(X, voxel_timeseries, rcond=None)
    return beta
```

---

## 8. 著名研究

- **Cognitive networks (Raichle 2001)**: Default Mode Network
- **MVPA decoding (Kamitani 2005)**: 解码看的内容 from V1
- **Reading brain (Talairach atlas)**
- **fMRI lie detection** (有争议)
- **Connectome (HCP)**: 1000+ subjects resting-state

---

## 9. 局限

### 9.1 时间分辨率差

BOLD 慢 5-7 sec;不能分辨 < 1s 事件。

### 9.2 间接 measurement

BOLD ≠ neural firing directly。

### 9.3 Multiple comparison

每 voxel test → 100k+ tests → 严 control needed。

### 9.4 Reverse inference

"X 区 active = X 处理任务 Y" 错误推理 (因果不可)。

### 9.5 Subject variability

个人解剖差异,需 normalization。

---

## 10. 现代发展

- **7T MRI**: sub-mm 分辨率
- **Real-time fMRI**: BCI / neurofeedback
- **fMRI + DTI**: structure + function
- **Multi-modal**: fMRI + EEG / MEG
- **AI decoding**: deep learning + fMRI

---

## 11. Common Pitfalls

### 11.1 "活动" ≠ 因果

fMRI 是 correlation;needs lesion / TMS for causal。

### 11.2 Subliminal effects

弱信号可能 random noise。

### 11.3 Salt fish fMRI (经典笑话)

死鱼 + 多 comparison + no correction → 假 activation。

### 11.4 ROI 选择

不能 fishing for significance。

### 11.5 Publication bias

负结果不发,夸大 effect size。

---

## 12. Related Concepts

- **同节**:[Optogenetics](Optogenetics.md)、[TMS](TMS.md)
- **解剖**:[Cortex](../01_Neuroanatomy/Cortex.md)

---

## References

1. **Ogawa, S. et al.** "Brain magnetic resonance imaging with contrast dependent on blood oxygenation (BOLD)." *PNAS*, 1990.
2. **Logothetis, N. K.** "What we can do and what we cannot do with fMRI." *Nature*, 2008.
3. **Raichle, M. E. et al.** "A default mode of brain function." *PNAS*, 2001.
4. **Poldrack, R. A.** "Inferring mental states from neuroimaging data." *NeuroImage*, 2011.
