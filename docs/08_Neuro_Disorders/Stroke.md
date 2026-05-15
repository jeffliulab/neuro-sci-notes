# 中风 (Stroke)

> *Stroke 是急性脑血管事件 — ischemic (缺血,87%) 或 hemorrhagic (出血,13%)。是全球**第二死因 + 第一致残原因**。"Time is brain" — 每 minute 失 ~ 2M neurons。tPA + thrombectomy 革命性救治。*
>
> **难度**:Intermediate
> **前置知识**:[Cortex](../01_Neuroanatomy/Cortex.md)、[Brainstem](../01_Neuroanatomy/Brainstem.md)

---

## 1. 类型

### 1.1 Ischemic Stroke (87%)

- **Thrombotic**: 局部 atherosclerotic plaque 破 → 血栓
- **Embolic**: 心脏 (afib) 或大动脉 来源 → 远端 blockage
- **Lacunar**: 小 perforating 动脉 (hypertension related)

### 1.2 Hemorrhagic (13%)

- **Intracerebral hemorrhage** (ICH): 高血压 / amyloid angiopathy
- **Subarachnoid hemorrhage** (SAH): aneurysm 破

### 1.3 TIA (Transient Ischemic Attack)

- "Mini-stroke",症状 < 24 hr 复
- 但 high risk 后续 full stroke

---

## 2. 病理生理

```
Vessel occlusion (or rupture)
  ↓
局部 brain ischemia (无 O₂ + glucose)
  ↓
ATP 枯竭 → Na+/K+ pump 失
  ↓
Glutamate massive release (excitotoxicity)
  ↓
Ca²⁺ inflow → apoptosis + necrosis
  ↓
Neurons 死 within minutes (core)
Penumbra (semi-saved 周围) 可 hours 救回
```

→ "Time is brain"。

---

## 3. 风险因素

- 高血压 (#1)
- Atrial fibrillation
- 糖尿病
- 吸烟
- 高胆固醇
- 久坐
- 家族史
- 年龄 > 65

---

## 4. 症状 (FAST)

- **F**ace drooping
- **A**rm weakness
- **S**peech difficulty
- **T**ime to call 911

Plus visual loss, dizzy, severe headache (SAH)。

---

## 5. 诊断

- **CT head** (首先, 排除 hemorrhage)
- **CT angiography** (找 LVO, large vessel occlusion)
- **MRI DWI**: 早期 ischemic detection
- **NIHSS** score (severity)

---

## 6. 急性治疗

### 6.1 Ischemic Stroke

- **tPA (alteplase) IV**: < 4.5 hr from onset → 溶栓
- **Thrombectomy** (mechanical clot removal): < 24 hr for LVO, **革命性** (2015+)
- **Aspirin** 抗血小板

### 6.2 Hemorrhagic

- BP 控制
- Anticoagulation reversal
- Neurosurgery 必要时

---

## 7. 二级预防

- **Antiplatelet**: aspirin, clopidogrel
- **Anticoagulant** (if afib): warfarin, DOAC
- **Statin** (LDL)
- **BP control** < 130/80
- **Lifestyle**: smoking 戒, 锻炼

---

## 8. 康复

- Acute (week 1-2): hospital
- Subacute: rehab facility (PT, OT, speech)
- Chronic: home / outpatient
- Recovery 最大 in first 3 months

### Cortical Reorganization

- 健 hemisphere 部分 compensate
- Plasticity-based therapy (CIMT, mirror therapy)
- TMS / tDCS 实验

---

## 9. BCI for Stroke

- Motor BCI:解码 imagined movement → control exoskeleton / FES
- Plasticity boost (closed-loop training)
- 部分 chronic 患者恢复 fine motor

---

## 10. AI 应用

- **CT/MRI imaging AI**: 早期 detection (Aidoc, Viz.ai)
- **Stroke scale auto-scoring**
- **Outcome prediction**
- **Rehabilitation robot** (Lokomat, Hocoma)
- **VR therapy**

---

## 11. PyTorch — Stroke imaging classification

```python
import torch
import torch.nn as nn

class StrokeCTClassifier(nn.Module):
    """Classify CT: normal / ischemic / hemorrhagic."""
    def __init__(self):
        super().__init__()
        # Use ResNet50-like 3D
        self.backbone = nn.Sequential(
            nn.Conv3d(1, 64, 7, stride=2), nn.BatchNorm3d(64), nn.ReLU(),
            nn.MaxPool3d(3, stride=2),
            # ... more blocks
            nn.AdaptiveAvgPool3d(1),
        )
        self.fc = nn.Linear(64, 3)
    
    def forward(self, ct_volume):
        feat = self.backbone(ct_volume).flatten(1)
        return self.fc(feat)
```

---

## 12. Common Pitfalls

### 12.1 FAST 不够全

posterior circulation stroke (dizzy, balance) 可不 fit FAST。

### 12.2 tPA 错过 window

> 4.5 hr → 不能 IV tPA;thrombectomy 仍可 to 24 hr。

### 12.3 Hemorrhage 不能 tPA

CT 排除 hemorrhage 是关键。

### 12.4 Mimics

Hypoglycemia, migraine, seizure 可类似 stroke。

### 12.5 BP target 平衡

太高 → 出血 risk;太低 → penumbra 减灌注。

---

## 13. Related Concepts

- **同节**:[Alzheimer](Alzheimer.md)、[Parkinson](Parkinson.md)
- **解剖**:[Cortex](../01_Neuroanatomy/Cortex.md)、[Brainstem](../01_Neuroanatomy/Brainstem.md)

---

## References

1. **Powers, W. J. et al.** "2018 Guidelines for the Early Management of Patients With Acute Ischemic Stroke." *Stroke*, 2018.
2. **Hacke, W. et al.** "Thrombolysis with alteplase 3 to 4.5 hours after acute ischemic stroke." *NEJM*, 2008.
3. **Goyal, M. et al.** "Endovascular thrombectomy after large-vessel ischaemic stroke." *Lancet*, 2016.
4. **Murphy, T. H. & Corbett, D.** "Plasticity during stroke recovery: from synapse to behaviour." *Nat Rev Neurosci*, 2009.
