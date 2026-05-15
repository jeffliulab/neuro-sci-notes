# Alzheimer's Disease (AD)

> *Alzheimer 是 most common 神经退行性疾病 — 60-70% of dementia cases。特征:amyloid plaques + tau tangles + 海马早期 atrophy + 记忆衰退。50+ 年研究尚无根治,但 2023-2024 anti-amyloid 药物 (Lecanemab, Donanemab) 是首个 disease-modifying。*
>
> **难度**:Intermediate
> **前置知识**:[Hippocampus 解剖](../01_Neuroanatomy/Hippocampus_Anatomy.md)、[Glia](../02_Cellular_Molecular/Glia.md)

---

## 1. 临床特征

- 渐进 episodic memory 衰退 (海马早期)
- 后期:语言、运动、人格变化
- 7-10 年从诊到死亡
- 全球 5500 万患者 (2025)

---

## 2. Pathology

### 2.1 Amyloid Plaques

- β-amyloid (Aβ 40, 42) 蛋白沉积
- 来源:APP (Amyloid Precursor Protein) 切错位
- 在 cortex 间隙 (extracellular)
- 影响 synaptic function + neuronal toxicity

### 2.2 Tau Tangles

- Hyperphosphorylated tau
- 形成 Neurofibrillary Tangles (NFT)
- 在 neuron 内 (intracellular)
- 破坏 microtubule transport

### 2.3 Neuron Loss

- 海马 + entorhinal 早期
- 后期 widespread cortex
- 50% 之后 才出明显症状

### 2.4 Inflammation

- Microglia + astrocytes 慢性激活
- 推动 progression

---

## 3. Amyloid Cascade Hypothesis

```
APP 切错 → Aβ 增 → plaque → 触发 tau 异常 → tangle → synaptic loss → cell death
```

50 年主流假说,但临床试验失败迫使 reconsider。

---

## 4. Tau Hypothesis

Tau 才是真正 driver?
- Tau spread 与 cognitive decline 关联更紧
- Tau immunotherapy 试验中

---

## 5. 遗传

- **APOE4 allele**: 高风险 (杂合 3×, 纯合 12×)
- **APP, PSEN1, PSEN2 mutations**: 早发型 (家族性) AD
- 大多数 AD 是 sporadic (后天)

---

## 6. 诊断

- **Clinical**: 认知 tests (MoCA, MMSE)
- **MRI**: 海马 + cortex atrophy
- **PET amyloid (Pittsburgh compound B)**: 可视 plaque
- **PET tau (e.g. AV-1451)**
- **CSF biomarkers**: Aβ42, tau
- **Plasma biomarkers** (新): p-tau217

---

## 7. 治疗 (2025 状态)

### 7.1 症状治疗 (无 disease-modifying 效果)

- **Cholinesterase inhibitors** (Donepezil, Rivastigmine): 提 ACh
- **Memantine**: NMDA modulator

### 7.2 Disease-modifying (2023-2024)

- **Lecanemab (Leqembi)**: anti-Aβ antibody, **FDA approved 2023**
- **Donanemab**: 类似,**FDA 2024**
- 减缓 ~ 25% 进展 (但 brain edema 风险)

### 7.3 实验

- Tau immunotherapy
- BACE inhibitors (减 Aβ 生产)
- Anti-inflammatory
- Lifestyle (运动, 认知训练)

---

## 8. Risk Factors

- 年龄 (主要)
- 遗传 (APOE)
- 教育低
- 心血管病
- 高血压, 糖尿病
- 缺乏运动 / 社交
- 睡眠少 (清 amyloid 受阻)
- 头部外伤

---

## 9. 与 AI

- DL diagnosis from MRI (~ 95% accuracy)
- Drug discovery (AlphaFold + screening)
- Cognitive testing apps
- Speech analysis for early signs

---

## 10. PyTorch — Hypothetical AD prediction

```python
import torch
import torch.nn as nn

class ADPredictor(nn.Module):
    """Predict AD from MRI."""
    def __init__(self):
        super().__init__()
        # 3D CNN on brain MRI
        self.conv = nn.Sequential(
            nn.Conv3d(1, 32, 3), nn.ReLU(),
            nn.MaxPool3d(2),
            nn.Conv3d(32, 64, 3), nn.ReLU(),
            nn.AdaptiveAvgPool3d(1),
        )
        self.fc = nn.Linear(64, 2)
    
    def forward(self, mri):
        feat = self.conv(mri).flatten(1)
        return self.fc(feat)  # [healthy, AD]
```

---

## 11. Common Pitfalls

### 11.1 Amyloid hypothesis 不完全

许多 amyloid+ 人 cognitively normal;tau 更预测 decline。

### 11.2 早期诊断难

症状前 plaque 已积累 10-20 年。

### 11.3 一线药物有限

Lecanemab 仅 ~25% 减缓,且有 ARIA 风险。

### 11.4 个体差异

Heterogeneous → 多 subtypes;single drug 难 fit all。

### 11.5 临床试验失败多

多个 anti-Aβ 药物失败 (verubecestat, solanezumab 等)。

---

## 12. Related Concepts

- **同节**:[Parkinson](Parkinson.md)、[Depression](Depression.md)
- **解剖**:[Hippocampus](../01_Neuroanatomy/Hippocampus_Anatomy.md)
- **细胞**:[Glia](../02_Cellular_Molecular/Glia.md)

---

## References

1. **Hardy, J. & Selkoe, D. J.** "The amyloid hypothesis of Alzheimer's disease." *Science*, 2002.
2. **Selkoe, D. J. & Hardy, J.** "The amyloid hypothesis of Alzheimer's disease at 25 years." *EMBO Mol Med*, 2016.
3. **van Dyck, C. H. et al.** "Lecanemab in Early Alzheimer's Disease." *NEJM*, 2023.
4. **Jack, C. R. et al.** "NIA-AA Research Framework: Toward a biological definition of Alzheimer's disease." *Alzheimers Dement*, 2018.
