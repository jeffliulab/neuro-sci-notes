# 脑磁图 (Magnetoencephalography)

> *MEG 测量神经电流产生的极微弱磁场(~ fT,地磁的十亿分之一)。优势:ms 时间分辨 + 优于 EEG 的空间定位(磁场不被颅骨扭曲)。需 SQUID + 磁屏蔽室(贵)。OPM-MEG(光泵磁强计,2018+)是可穿戴革命。源定位 + 振荡研究主力。*
>
> **难度**:Advanced
> **前置知识**:[EEG](EEG.md)、[Brain Rhythms](../00_Foundations/Brain_Rhythms.md)

---

## 1. 原理

- 神经元突触后电流 → 微弱磁场(Biot-Savart)
- 主要源:**皮层沟壁**的锥体细胞(切向电流,磁场可测)
- 量级 ~ 10-1000 fT(femtoTesla)
- 检测器:SQUID(超导量子干涉仪,需液氦)或 OPM

---

## 2. MEG vs EEG

| | MEG | EEG |
|---|---|---|
| 测量 | 磁场 | 电位 |
| 颅骨影响 | 几乎无(磁透明) | 强扭曲扩散 |
| 空间定位 | 较好(~ mm-cm) | 较差 |
| 敏感源 | 切向(沟壁) | 切向+径向 |
| 参考电极 | 无需(绝对) | 需(相对) |
| 便携 | 传统不可(OPM 可) | 可 |

---

## 3. 逆问题 (Source Localization)

- 由头表磁场反推脑内电流源 = **病态逆问题**(无唯一解)
- 方法:
  - **Dipole fitting**(等效电流偶极)
  - **Beamforming**(LCMV)
  - **Minimum norm estimate (MNE)**
  - **MUSIC**
- 需 head model(MRI 配准)+ 先验约束

---

## 4. PyTorch — Minimum Norm 逆解(简化)

```python
import torch

def minimum_norm_estimate(B, leadfield, reg=1e-3):
    """B: (n_sensors,) measured field; L: (n_sensors, n_sources) leadfield."""
    L = leadfield
    # MNE: J = L^T (L L^T + reg I)^-1 B  (Tikhonov-regularized)
    LLt = L @ L.t()
    inv = torch.linalg.solve(LLt + reg * torch.eye(LLt.size(0)), B)
    J = L.t() @ inv          # estimated source currents
    return J
```

---

## 5. OPM-MEG 革命

- **Optically Pumped Magnetometer**:无需液氦,室温
- 可贴头皮(更近源 → 信号↑)
- 可穿戴 + 允许头动 → 自然行为、儿童、临床
- 2018+ 快速发展(取代 SQUID 趋势)

---

## 6. 应用

- **癫痫**:致痫灶定位(术前)— 临床主用途
- **认知神经科学**:语言、注意、记忆时空动态
- **振荡**:gamma/beta(运动)、theta — 优于 EEG 定位
- **MEG-fMRI 融合**:时间(MEG)+ 空间(fMRI)

---

## 7. 优势总结

- ms 时间分辨(同 EEG)
- 空间定位优于 EEG(磁场不被组织扭曲)
- 无需参考电极、无电极胶
- 直接测神经电流(不像 fMRI 间接血氧)

---

## 8. 局限

- 设备极贵 + 磁屏蔽室(传统 SQUID)
- 对**径向**源不敏感(脑回顶 vs 沟壁)
- 深部源信号弱(随距离快衰减)
- 逆问题非唯一(需约束 + 假设)
- 对运动敏感(传统;OPM 改善)

---

## 9. 与 AI

- 源定位 = 病态逆问题 → 深度学习重建(类 MRI 重建)
- MEG decoding(解码知觉/语言)+ Transformer
- Real-time BCI(OPM 可穿戴 → 新可能)

---

## 10. Common Pitfalls

### 10.1 MEG = EEG 磁版

互补但敏感不同源(MEG 切向,EEG 兼径向)。

### 10.2 空间定位精确唯一

逆问题非唯一;依 head model + 先验。

### 10.3 测所有 neuron

主要切向沟壁锥体细胞同步活动;深部弱。

### 10.4 SQUID 是唯一

OPM-MEG 室温可穿戴正在取代。

### 10.5 无需 MRI

源定位需 MRI 解剖配准(head model)。

---

## 11. Related Concepts

- **同节**:[EEG](EEG.md)、[fMRI BOLD](fMRI_BOLD.md)、[TMS](TMS.md)
- **基础**:[Brain Rhythms](../00_Foundations/Brain_Rhythms.md)、[Research Methods](../00_Foundations/Research_Methods.md)
- **疾病**:[Epilepsy](../08_Neuro_Disorders/Epilepsy.md)

---

## References

1. **Hämäläinen, M. et al.** "Magnetoencephalography—theory, instrumentation, and applications." *Rev Mod Phys*, 1993.
2. **Baillet, S.** "Magnetoencephalography for brain electrophysiology and imaging." *Nat Neurosci*, 2017.
3. **Boto, E. et al.** "Moving magnetoencephalography towards real-world applications with a wearable system (OPM)." *Nature*, 2018.
4. **Gross, J. et al.** "Good practice for conducting and reporting MEG research." *NeuroImage*, 2013.
