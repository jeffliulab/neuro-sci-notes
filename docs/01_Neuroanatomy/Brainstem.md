# 脑干 (Brainstem) — 生命中枢

> *脑干位于 brain 底部,连接 cortex / cerebellum 与脊髓。3 部分:midbrain (中脑), pons (脑桥), medulla (延髓)。控制呼吸、心率、睡眠 / 唤醒、12 对 cranial nerves 中 10 对。损伤致命。*
>
> **难度**:Introduction-Intermediate
> **前置知识**:[Cortex](Cortex.md)

---

## 1. 三部分

### 1.1 Midbrain (中脑)

- 包含 substantia nigra (SNc/SNr), VTA, superior colliculus, inferior colliculus
- 视 / 听 reflex (saccade, startle)
- DA neurons (运动 + reward)

### 1.2 Pons (脑桥)

- 连接 cerebellum 与 cortex (cerebellar peduncles)
- 含 locus coeruleus (NE), raphe nuclei (5-HT)
- 睡眠 / REM 调节

### 1.3 Medulla (延髓)

- **生命中枢**: 呼吸、心率、血压
- 损伤 → 死亡
- Pyramidal decussation: motor 通路 90% 交叉

---

## 2. Cranial Nerves (12 对中的 10 对)

- III Oculomotor (眼动)
- IV Trochlear
- V Trigeminal (面部 sensory + 咀嚼)
- VI Abducens
- VII Facial (面表情)
- VIII Vestibulocochlear (平衡 + 听)
- IX Glossopharyngeal
- X Vagus (内脏 control)
- XI Spinal accessory
- XII Hypoglossal (舌)

I (olfactory) + II (optic) 不经脑干。

---

## 3. RAS (Reticular Activating System)

弥散网络从 medulla → midbrain → thalamus → cortex:
- 控制 arousal / consciousness
- 损伤 → coma
- 麻醉作用点

---

## 4. 重要核团

- **Locus Coeruleus** (pons): 主 NE source — 注意 / arousal
- **Raphe nuclei**: 主 5-HT source — 情绪 / 睡眠
- **PAG (Periaqueductal Gray)**: 痛调节 (opioid action site)
- **Nucleus tractus solitarius (NTS)**: 内脏 sensory hub
- **DMN (dorsal motor of vagus)**: parasympathetic output

---

## 5. 反射

- Pupillary light reflex
- Gag reflex
- Cough reflex
- Vestibulo-ocular reflex (VOR)
- Babinski (新生儿)

许多 reflexes 测脑干完整性 (临床 neurology exam)。

---

## 6. 脑干死亡

临床定义 brain death:
- 无 cranial nerve reflexes
- 无 self-respiratory effort
- Coma 排除 reversible cause (drug, hypothermia)

→ 即使心跳维持,法律 declared dead。

---

## 7. 病理

- **Stroke (Brainstem)**: 严重残疾 / 死亡;Wallenberg syndrome (PICA stroke)
- **Locked-in syndrome**: pons 损但 cortex OK → 全身瘫但意识清
- **Multiple system atrophy**: 脑干自主神经退行
- **Brainstem tumors**: 多发儿童

---

## 8. PyTorch — RAS 简化 model

```python
import torch
import torch.nn as nn

class ReticularActivating(nn.Module):
    """Simplified RAS modulating cortical arousal."""
    def __init__(self, n_cortex=100):
        super().__init__()
        # Diffuse projection
        self.projection = nn.Linear(1, n_cortex, bias=False)
    
    def forward(self, arousal_level, cortex_input):
        """arousal_level in [0, 1]."""
        modulation = self.projection(arousal_level.unsqueeze(-1))
        return cortex_input * (1 + modulation)  # gain modulation
```

---

## 9. 与 AI / Architecture

Brainstem-like global arousal modulation 启发:
- Attention gain modulation in deep learning
- 全局调节信号 (e.g. noise injection)
- 但 brain 比 AI 复杂多

---

## 10. Common Pitfalls

### 10.1 "Brainstem 仅是简单 reflex"

近代发现复杂 modulation + cognition role。

### 10.2 损伤定位难

脑干小 + 结构密 → 小损伤多影响。

### 10.3 RAS ≠ 单一 nucleus

是 distributed network。

### 10.4 Cranial nerves 编号 ≠ 位置

I = olfactory bulb (forebrain), II = retina (eye),都不在脑干。

### 10.5 Decussation

Motor 在 medulla 交叉;sensory 部分 spinal cord 交叉 → 左脑控右身。

---

## 11. Related Concepts

- **同节**:[Cortex](Cortex.md)、[Cerebellum](Cerebellum.md)、[Basal Ganglia](Basal_Ganglia.md)
- **细胞**:[神经递质](../02_Cellular_Molecular/Neurotransmitters.md)

---

## References

1. **Brodal, P.** *The Central Nervous System*. 5th ed., 2016.
2. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
3. **Wijdicks, E. F.** *The Practice of Emergency and Critical Care Neurology*. Oxford, 2010.
