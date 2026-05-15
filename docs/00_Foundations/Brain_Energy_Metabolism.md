# 脑能量代谢 (Brain Energy Metabolism)

> *Brain 占体重 2% 却耗 20% 能量。几乎全靠 glucose + O₂ 氧化(无脂肪储备)。ATP 大部分用于维持离子梯度(Na/K pump)。Astrocyte-neuron lactate shuttle 是能量供给模型。能量约束塑造了 sparse coding + 神经计算效率。fMRI BOLD 信号正是基于能量需求。*
>
> **难度**:Intermediate
> **前置知识**:[Membrane Potential](../02_Cellular_Molecular/Membrane_Potential.md)、基础代谢

---

## 1. 数字

| 项 | 值 |
|---|---|
| Brain 质量 | ~ 2% 体重 |
| 能耗 | ~ 20% 静息代谢 |
| 功率 | ~ 20 W |
| Glucose | ~ 120 g/day |
| O₂ | ~ 20% 全身消耗 |
| ATP turnover | 极高,无储备 |

---

## 2. 能量去向

- **~ 50-70%**: Na⁺/K⁺-ATPase(恢复离子梯度)
- 其余:synaptic transmission、neurotransmitter recycling、轴浆运输、housekeeping
- 结论:维持 excitability 本身就极耗能

---

## 3. 主能源 — Glucose

```
Glucose → Glycolysis → Pyruvate
              ↓
        Mitochondria (TCA + OxPhos)
              ↓
        ~ 30-32 ATP / glucose
```

- 脑几乎只用 glucose(饥饿时 ketone bodies 可替代)
- 不能用 fatty acid(血脑屏障)

---

## 4. Astrocyte-Neuron Lactate Shuttle (ANLS)

- Pellerin & Magistretti 1994 假说
- Glutamate 释放 → astrocyte 摄取 → 触发 astrocyte glycolysis → lactate
- Lactate → neuron 作燃料
- 仍有争议,但解释 fMRI 与能量耦合

---

## 5. Neurovascular Coupling

- 神经活动 → 局部血流 ↑(供 glucose + O₂)
- 这是 **fMRI BOLD** 的物理基础
- 延迟 ~ 1-2 sec(hemodynamic response)
- 见 [fMRI BOLD](../07_Neurotech_Frontiers/fMRI_BOLD.md)

---

## 6. 能量约束 → 计算原则

- Sparse coding 节能(少 spike 少 ATP)
- Attneave / Barlow:efficient coding hypothesis
- Spike 本身昂贵 → brain 优化 spike 数
- Levy & Baxter 1996:能量最优 → sparse firing rate ~ 几 Hz

---

## 7. PyTorch — 能量约束 sparse loss

```python
import torch

def energy_constrained_loss(activations, target, lambda_energy=0.01):
    """Task loss + metabolic (sparsity) cost — efficient coding."""
    task_loss = ((activations.sum(1) - target) ** 2).mean()
    # Each spike costs energy → L1 penalty on activation
    energy_cost = activations.abs().mean()
    return task_loss + lambda_energy * energy_cost
```

→ 与 brain energy budget 类比:L1 稀疏 = 代谢约束。

---

## 8. 病理

- **Stroke**: 缺血 → 能量崩溃 → excitotoxicity → neuron death
- **Hypoglycemia**: 低血糖 → 意识丧失(brain 无糖储备)
- **Mitochondrial disease**: Leigh syndrome 等
- **Alzheimer**: glucose hypometabolism(FDG-PET 早 marker)
- **Aging**: 代谢效率下降

---

## 9. 测量

- **FDG-PET**: 放射 glucose analog → 代谢图
- **fMRI BOLD**: 间接(血氧)
- **MRS**: lactate / NAA / ATP
- **Calorimetry**: 全脑能耗

---

## 10. Common Pitfalls

### 10.1 思考越多耗能越大

差异其实很小(~ 5%);brain baseline 已极高(Raichle "dark energy")。

### 10.2 Brain 用脂肪

不能;BBB 阻挡 fatty acid;饥饿用 ketone。

### 10.3 ANLS 已定论

仍争议;部分 neuron 直接用 glucose。

### 10.4 BOLD = neural activity

是能量/血流代理,延迟且非线性。

### 10.5 更多 neuron = 更聪明

能量约束限制 brain size;效率 > 数量。

---

## 11. Related Concepts

- **同节**:[Neural Coding](Neural_Coding.md)、[Research Methods](Research_Methods.md)
- **细胞**:[Membrane Potential](../02_Cellular_Molecular/Membrane_Potential.md)、[Glia](../02_Cellular_Molecular/Glia.md)
- **前沿**:[fMRI BOLD](../07_Neurotech_Frontiers/fMRI_BOLD.md)

---

## References

1. **Attwell, D. & Laughlin, S. B.** "An energy budget for signaling in the grey matter of the brain." *J Cereb Blood Flow Metab*, 2001.
2. **Pellerin, L. & Magistretti, P. J.** "Glutamate uptake into astrocytes stimulates aerobic glycolysis." *PNAS*, 1994.
3. **Raichle, M. E. & Mintun, M. A.** "Brain work and brain imaging." *Annu Rev Neurosci*, 2006.
4. **Magistretti, P. J. & Allaman, I.** "A cellular perspective on brain energy metabolism." *Neuron*, 2015.
