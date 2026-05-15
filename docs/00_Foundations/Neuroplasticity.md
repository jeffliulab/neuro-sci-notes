# 神经可塑性 (Neuroplasticity)

> *Neuroplasticity 是 brain 终生改变结构 + 功能的能力。从 synaptic (LTP/LTD) 到 structural (spine、axon sprouting) 到 systems (cortical remapping) 到 neurogenesis。是 learning、memory、recovery 的基础。"Neurons that fire together wire together" (Hebb)。也是 rehabilitation 与 BCI 训练的原理。*
>
> **难度**:Intermediate
> **前置知识**:[LTP_LTD](../02_Cellular_Molecular/LTP_LTD.md)、[Neurodevelopment](Neurodevelopment.md)

---

## 1. 可塑性层级

| 层级 | 例 | 时间尺度 |
|---|---|---|
| Synaptic | LTP / LTD | ms-小时 |
| Structural | spine 增减、axon sprouting | 小时-天 |
| Intrinsic | 离子通道密度变化 | 小时-天 |
| Systems | cortical map remapping | 天-月 |
| Neurogenesis | 新 neuron(海马 DG) | 周-月 |

---

## 2. Hebbian 原则

> "Cells that fire together wire together" — Hebb 1949

$$\Delta w_{ij} = \eta \, x_i \, x_j$$

- 共激活 → 突触增强
- 单纯 Hebbian 不稳定 → 需 normalization / BCM rule
- STDP 是 spike-timing 版本

---

## 3. Synaptic Plasticity

- **LTP**: 长时程增强(NMDA → Ca²⁺ → AMPA 插入)
- **LTD**: 长时程抑制
- **Homeostatic plasticity**: synaptic scaling(保持稳定)
- 见 [LTP_LTD](../02_Cellular_Molecular/LTP_LTD.md)

---

## 4. Structural Plasticity

- **Spine turnover**: 学习 → 新 spine 形成
- **Axon sprouting**: 损伤后侧支生长
- **Dendritic remodeling**
- Two-photon in vivo imaging 直接观察

---

## 5. Cortical Remapping

- **截肢**:相邻 cortical 区侵占(phantom limb)
- **盲人**:visual cortex 被 touch/语言征用(cross-modal)
- **音乐家**:手指 representation 扩大
- **London taxi**:海马后部增大(Maguire 2000)

---

## 6. 关键期 vs 终生可塑

- Critical period:童年特定窗口最强
- 但成人仍有(降低但存在)
- 重开机制:环境富化、药物(fluoxetine)、训练

---

## 7. PyTorch — Hebbian + Oja Rule

```python
import torch

def oja_update(W, x, lr=0.01):
    """Oja's rule: Hebbian + normalization (stable)."""
    y = W @ x                          # output
    # ΔW = lr * y * (x - y * W)  — prevents unbounded growth
    dW = lr * (torch.outer(y, x) - torch.outer(y, y) @ W)
    return W + dW
```

→ 纯 Hebbian 发散;Oja 加 normalization 收敛到主成分。

---

## 8. 临床应用

- **Stroke rehab**: constraint-induced movement therapy(强迫用患肢)
- **BCI training**: 用户学控制 → cortical plasticity
- **Tinnitus / phantom pain**: maladaptive plasticity → mirror therapy
- **Antidepressant**: SSRI 增 plasticity(BDNF)
- **Stroke + brain training**

---

## 9. 负面可塑("maladaptive")

- Phantom limb pain
- Chronic pain centralization
- Addiction(reward circuit 重塑)
- PTSD(fear memory 强化)
- Dystonia(过度 representation overlap)

---

## 10. AI 类比

- **Backprop ≠ Hebbian**:生物用 local rule;backprop 非生物 plausible
- **Continual learning**:AI catastrophic forgetting vs brain 持续学
- **Transfer learning** ↔ 经验迁移
- **Lottery ticket / pruning** ↔ developmental pruning

---

## 11. Common Pitfalls

### 11.1 成人脑不变

终生可塑;只是关键期后较弱。

### 11.2 可塑性总是好

Maladaptive plasticity(痛、成瘾)同样存在。

### 11.3 Hebbian 即 backprop

Backprop 非 biologically plausible(weight transport problem)。

### 11.4 Neurogenesis 普遍

成人主要限海马 DG(且人类争议);非全脑。

### 11.5 多用 = 一定变强

需正确 + 有意义训练;无效重复未必。

---

## 12. Related Concepts

- **同节**:[Neurodevelopment](Neurodevelopment.md)、[Neural Coding](Neural_Coding.md)
- **细胞**:[LTP_LTD](../02_Cellular_Molecular/LTP_LTD.md)、[Synapse](../02_Cellular_Molecular/Synapse.md)、[Dendrites](../02_Cellular_Molecular/Dendrites.md)
- **系统**:[Hippocampus Memory](../03_Systems_Neuroscience/Hippocampus_Memory.md)

---

## References

1. **Hebb, D. O.** *The Organization of Behavior*. Wiley, 1949.
2. **Pascual-Leone, A. et al.** "The plastic human brain cortex." *Annu Rev Neurosci*, 2005.
3. **Maguire, E. A. et al.** "Navigation-related structural change in the hippocampi of taxi drivers." *PNAS*, 2000.
4. **Holtmaat, A. & Svoboda, K.** "Experience-dependent structural synaptic plasticity in the mammalian brain." *Nat Rev Neurosci*, 2009.
