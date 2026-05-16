# 自主神经系统 (Autonomic Nervous System)

> *ANS 调控内脏 + 腺体 + 平滑肌(不随意)。Sympathetic("战或逃")vs Parasympathetic("休与化")vs Enteric("肠脑")。中枢:下丘脑 + 脑干 + 岛叶。双重支配 + 张力平衡。HRV(心率变异)是其 readout。与情绪(James-Lange)、压力、内感受紧密。*
>
> **难度**:Intermediate
> **前置知识**:[Brainstem](../01_Neuroanatomy/Brainstem.md)、[Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.md)

---

## 1. 三分支

| | Sympathetic | Parasympathetic | Enteric |
|---|---|---|---|
| 功能 | 战/逃(动员) | 休/化(保存) | 胃肠半自主 |
| 起源 | 胸腰(T1-L2) | 脑干 + 骶 | 肠壁(~5 亿元) |
| 节后递质 | NE(多数) | ACh | 多种 |
| 心率 | ↑ | ↓ | — |

---

## 2. 双重支配 + 张力

- 多数器官受交感 + 副交感**双重**支配,常拮抗
- 非"开关"而是**张力平衡**(tone)动态调节
- 例:静息心率 ~ 70 = 内在 ~100 被迷走持续抑制
- HRV(心率变异)反映迷走张力

---

## 3. 神经化学

- **节前**:均 ACh(烟碱型)
- **交感节后**:NE(→ α/β 肾上腺素受体);汗腺例外用 ACh
- **副交感节后**:ACh(毒蕈碱型)
- 肾上腺髓质 = 特化交感节(直接释 Epi 入血)

---

## 4. 中枢控制

```
皮层(岛叶/mPFC/ACC)
   ↓
下丘脑(整合中枢:体温/渗透/应激)
   ↓
脑干(NTS 输入 ↔ 疑核/RVLM 输出)
   ↓
脊髓中间外侧柱
   ↓
节 → 靶器官
```

岛叶 = 内脏感觉皮层(见 [Interoception](Interoception.md))。

---

## 5. PyTorch — 交感/副交感张力平衡

```python
import torch

def autonomic_balance(stressor, baseline_hr=70.0):
    """Sympatho-vagal balance sets heart rate dynamically."""
    sympathetic = torch.sigmoid(torch.tensor(stressor - 0.5)) * 60   # ↑HR
    parasympathetic = torch.sigmoid(torch.tensor(0.5 - stressor)) * 30  # ↓HR
    hr = baseline_hr + sympathetic - parasympathetic
    hrv = parasympathetic / (sympathetic + 1e-3)   # vagal tone proxy
    return hr, hrv
```

---

## 6. 反射

- **压力感受器反射**(baroreflex):BP↑ → NTS → 迷走↑/交感↓ → BP↓(负反馈)
- **化学感受器反射**:O₂↓/CO₂↑ → 呼吸/循环
- **胃肠反射**:enteric + 迷走
- 反射弧:感受器 → NTS → 脑干 → 输出

---

## 7. 情绪 + 应激

- **James-Lange**:情绪 = 对身体(自主)状态的感知
- **HPA 轴**(应激):下丘脑 CRH → 垂体 ACTH → 肾上腺皮质醇(慢);交感-肾上腺髓质(快)
- 慢性应激 → 自主失衡 → 心血管病、免疫抑制
- Polyvagal theory(Porges,有争议)

---

## 8. 病理

- **直立性低血压 / POTS**:自主调节失衡
- **糖尿病自主神经病**:无症状心梗、胃轻瘫
- **多系统萎缩(MSA)**:严重自主衰竭
- **惊恐发作**:交感激增 + 内感受误读
- **迷走神经刺激(VNS)**:难治癫痫/抑郁治疗(见 [DBS](../07_Neurotech_Frontiers/DBS.md))

---

## 9. 与 AI / 工程

- HRV biofeedback ↔ closed-loop 调节
- 自主信号(GSR/HR)→ 情感计算(affective computing)
- 稳态调节 ↔ 控制论 homeostasis(见 [Hypothalamus_Homeostasis](Hypothalamus_Homeostasis.md))
- VNS = closed-loop neuromodulation(见 [Closed_Loop_Neuromodulation](../07_Neurotech_Frontiers/Closed_Loop_Neuromodulation.md))

---

## 10. Common Pitfalls

### 10.1 交感 = 坏,副交感 = 好

二者协同;均必需,失衡才病理。

### 10.2 ANS 完全不可控

呼吸/biofeedback/冥想可部分调(经迷走)。

### 10.3 节后均用 NE/ACh 简单二分

汗腺交感用 ACh;肠神经多递质(例外多)。

### 10.4 战或逃 = 全或无

是连续张力调节,非开关。

### 10.5 ANS 与情绪/认知无关

岛叶/mPFC 调控;内感受是情绪基础(James-Lange)。

---

## 11. Related Concepts

- **同节**:[Interoception](Interoception.md)、[Hypothalamus_Homeostasis](Hypothalamus_Homeostasis.md)、[Sleep_Wake](Sleep_Wake.md)
- **解剖**:[Brainstem](../01_Neuroanatomy/Brainstem.md)
- **认知**:[Emotion](../04_Cognitive_Neuroscience/Emotion.md)
- **细胞**:[Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.md)(ACh/NE)

---

## References

1. **Jänig, W.** *The Integrative Action of the Autonomic Nervous System*. Cambridge, 2006.
2. **Benarroch, E. E.** "The central autonomic network: functional organization." *Mayo Clin Proc*, 1993.
3. **Thayer, J. F. & Lane, R. D.** "A model of neurovisceral integration in emotion regulation." *J Affect Disord*, 2000.
4. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
