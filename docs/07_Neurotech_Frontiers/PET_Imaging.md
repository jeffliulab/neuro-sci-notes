# 正电子发射断层成像 (PET)

> *PET 注入正电子放射性示踪剂,经湮灭 → 双 511 keV 光子符合检测 → 分子级功能成像。独特优势:**分子特异**(受体、代谢、蛋白沉积)。FDG-PET(葡萄糖代谢)、amyloid/tau-PET(AD)、DA-PET(PD)是临床支柱。空间分辨中(~ 4 mm),需回旋加速器 + 辐射。*
>
> **难度**:Advanced
> **前置知识**:[Brain Energy Metabolism](../00_Foundations/Brain_Energy_Metabolism.md)、[fMRI BOLD](fMRI_BOLD.md)

---

## 1. 原理

```
放射性核素衰变 → 正电子 (β⁺)
   ↓ 行进 ~1-2 mm 后湮灭
e⁺ + e⁻ → 2 个 511 keV 光子 (反向 180°)
   ↓
环形探测器 符合检测 (coincidence)
   ↓
反投影 / 迭代重建 → 3D 示踪剂分布图
```

---

## 2. 常用示踪剂

| 示踪剂 | 测什么 | 应用 |
|---|---|---|
| **¹⁸F-FDG** | 葡萄糖代谢 | AD(海马顶叶↓)、肿瘤、癫痫灶 |
| **¹¹C-PiB / ¹⁸F-florbetapir** | 淀粉样斑块 | Alzheimer 诊断 |
| **¹⁸F-flortaucipir** | tau 缠结 | AD 分期、FTD |
| **¹⁸F-DOPA / ¹¹C-raclopride** | 多巴胺合成 / D2 | Parkinson |
| **¹⁵O-water** | 脑血流 | 激活研究(老式 fMRI 前身) |
| **受体 PET** | 5-HT/μ-opioid/GABA | 精神药理 |

---

## 3. 核素 + 半衰期

| 核素 | 半衰期 | 来源 |
|---|---|---|
| ¹⁸F | 110 min | 回旋加速器(可运输) |
| ¹¹C | 20 min | 现场回旋加速器 |
| ¹⁵O | 2 min | 现场,即用 |
| ⁶⁸Ga | 68 min | generator(无需回旋) |

短半衰期 → 需邻近回旋加速器(成本 + 物流)。

---

## 4. 定量

- **SUV**(standardized uptake value):半定量
- **Binding potential (BP)**:受体可用度(动力学建模)
- **Patlak / Logan plot**:图形分析不可逆/可逆示踪剂
- 需 arterial input function 或参考区

---

## 5. PyTorch — 符合检测线 (LOR) 简化

```python
import torch

def coincidence_backproject(detector_hits, n_pixels=64):
    """Each coincidence pair defines a Line Of Response; backproject."""
    img = torch.zeros(n_pixels, n_pixels)
    for (d1, d2) in detector_hits:           # detector index pairs
        # Simplified: deposit along straight line between detectors
        ys = torch.linspace(d1[0], d2[0], n_pixels).long()
        xs = torch.linspace(d1[1], d2[1], n_pixels).long()
        img[ys, xs] += 1
    return img   # iterative recon (OSEM) refines this in practice
```

---

## 6. 与 fMRI / SPECT 对比

| | PET | fMRI | SPECT |
|---|---|---|---|
| 信号 | 分子示踪剂 | 血氧 | 单光子示踪剂 |
| 特异性 | 分子级(受体/蛋白) | 血流代理 | 分子(较粗) |
| 空间 | ~ 4 mm | ~ 2 mm | ~ 8 mm |
| 时间 | min | sec | min |
| 辐射 | 有 | 无 | 有 |

→ PET 不可替代之处:**特定分子**(amyloid/tau/受体)成像。

---

## 7. AD 诊断革命

- **Amyloid-PET**:活体确认 Aβ 沉积(曾仅尸检)
- **Tau-PET**:与认知衰退更相关(Braak 分期活体化)
- 配合 CSF/血浆 biomarker → AT(N) 框架(见 [Alzheimer](../08_Neuro_Disorders/Alzheimer.md))
- 抗 Aβ 药(lecanemab)疗效监测

---

## 8. 局限

- **电离辐射**(限重复 / 儿童 / 健康对照)
- 空间分辨中等 + partial volume effect
- 时间分辨差(分钟)
- 昂贵 + 回旋加速器依赖
- 示踪剂特异性 / 脱靶需验证

---

## 9. 与 AI

- 低剂量 PET 重建(深度学习降噪 → 减辐射)
- Amyloid/tau-PET 自动定量 + 疾病预测
- PET-MRI 联合(同机多模态)
- Tracer kinetic 参数估计加速

---

## 10. Common Pitfalls

### 10.1 PET = 慢 fMRI

核心是**分子特异**(受体/蛋白),非血流速度比拼。

### 10.2 Amyloid-PET 阳性 = AD

Aβ 阳性可见于无症状老人;需结合临床 + tau。

### 10.3 SUV 是绝对定量

半定量;受体研究需动力学建模(BP)。

### 10.4 无辐射顾虑

电离辐射;限重复 + 健康受试。

### 10.5 短半衰期无关紧要

¹¹C/¹⁵O 需现场回旋加速器(重大成本/物流)。

---

## 11. Related Concepts

- **同节**:[fMRI BOLD](fMRI_BOLD.md)、[DTI Tractography](DTI_Tractography.md)
- **基础**:[Brain Energy Metabolism](../00_Foundations/Brain_Energy_Metabolism.md)
- **疾病**:[Alzheimer](../08_Neuro_Disorders/Alzheimer.md)、[Parkinson](../08_Neuro_Disorders/Parkinson.md)

---

## References

1. **Phelps, M. E.** "Positron emission tomography provides molecular imaging of biological processes." *PNAS*, 2000.
2. **Klunk, W. E. et al.** "Imaging brain amyloid in Alzheimer's disease with Pittsburgh Compound-B." *Ann Neurol*, 2004.
3. **Jack, C. R. et al.** "NIA-AA Research Framework: AT(N) biomarkers." *Alzheimers Dement*, 2018.
4. **Innis, R. B. et al.** "Consensus nomenclature for in vivo imaging of reversibly binding radioligands." *J Cereb Blood Flow Metab*, 2007.
