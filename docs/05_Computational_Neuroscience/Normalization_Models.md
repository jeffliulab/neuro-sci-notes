# 归一化模型 (Divisive Normalization)

> *Divisive normalization (Heeger 1992, Carandini & Heeger 2012) 是 brain 的"canonical computation":neuron 响应被邻近群体活动总和除。解释 V1 contrast saturation、cross-orientation suppression、attention、多感觉整合、value coding。是少数跨脑区通用的运算 — 类似 deep learning 的 LayerNorm/softmax。*
>
> **难度**:Advanced
> **前置知识**:[Visual System](../03_Systems_Neuroscience/Visual_System.md)、[Neural Circuits](../00_Foundations/Neural_Circuits.md)

---

## 1. 公式

$$R_i = \frac{D_i^n}{\sigma^n + \sum_j w_{ij} D_j^n}$$

- $D_i$:neuron i 的 driving input
- 分母:归一化池(normalization pool)
- $\sigma$:半饱和常数
- $n$:指数(~ 2)

→ 响应被群体活动"除"。

---

## 2. 解释的现象

| 现象 | 机制 |
|---|---|
| Contrast saturation | 高对比 → 分母大 → 饱和 |
| Cross-orientation suppression | 正交 grating 进 norm pool |
| Surround suppression | 周边进 pool |
| Attention | 调 normalization weight / gain |
| Redundancy reduction | 降相关 |
| Adaptation | 动态 σ |

---

## 3. Canonical Computation

- Carandini & Heeger 2012:normalization 出现在
  - Retina、LGN、V1、MT、IT
  - Auditory、olfactory
  - Multisensory、value(LIP、OFC)
- → 一种跨脑区**通用运算原语**

---

## 4. 与 Attention

- Reynolds & Heeger 2009 normalization model of attention
- Attention 调 normalization → contrast gain vs response gain
- 统一解释 spatial / feature attention 效应

---

## 5. PyTorch — Divisive Normalization

```python
import torch
import torch.nn.functional as F

def divisive_normalization(drive, sigma=0.5, n=2.0):
    """drive: (B, C, H, W). Normalize by local pool."""
    d_n = drive.abs() ** n
    # Normalization pool: sum over channels (cross-feature)
    pool = d_n.sum(dim=1, keepdim=True)
    return d_n / (sigma**n + pool)

# Compare with softmax / LayerNorm — same divisive motif
```

---

## 6. 与 Deep Learning

| Brain norm | DL |
|---|---|
| Divisive normalization | LayerNorm / softmax |
| Cross-orientation suppression | channel competition |
| Gain control | BatchNorm gain |
| Sigmoid saturation | activation saturation |
| Attention via norm | softmax attention |

Normalization 是 brain 与 DL 共享的核心 motif。

---

## 7. 机制实现

- Shunting inhibition(分流抑制 → 除法)
- Feedback inhibition pool
- Synaptic depression
- 多种生物机制可实现同一 normative 运算

---

## 8. Value Normalization (经济决策)

- Louie & Glimcher:LIP/OFC value 编码被 normalize
- → context-dependent choice、IIA violation
- "我对汉堡的价值取决于菜单其他选项"
- 解释 decoy effect 等行为经济异象

---

## 9. 适应性优势

- 扩展动态范围(有限 spike → 编码大范围)
- 去相关 → efficient coding(见 [Efficient Coding](Efficient_Coding.md))
- 不变性(对比不变的 orientation tuning)

---

## 10. Common Pitfalls

### 10.1 仅视觉

跨模态 + value + attention 普遍。

### 10.2 单一机制

多生物机制(shunting、feedback、depression)实现同运算。

### 10.3 Norm pool = 所有 neuron

Pool 有选择性 weight(非全等权)。

### 10.4 线性运算

本质非线性(除法 + 指数)。

### 10.5 与 attention 无关

Attention 正是通过调 normalization 起作用(Reynolds-Heeger)。

---

## 11. Related Concepts

- **同节**:[Efficient Coding](Efficient_Coding.md)、[Predictive Coding](Predictive_Coding.md)
- **基础**:[Neural Circuits](../00_Foundations/Neural_Circuits.md)
- **系统**:[Visual System](../03_Systems_Neuroscience/Visual_System.md)
- **认知**:[Attention](../04_Cognitive_Neuroscience/Attention.md)、[Decision Making](../04_Cognitive_Neuroscience/Decision_Making.md)
- **AI**: LayerNorm、softmax attention

---

## References

1. **Heeger, D. J.** "Normalization of cell responses in cat striate cortex." *Vis Neurosci*, 1992.
2. **Carandini, M. & Heeger, D. J.** "Normalization as a canonical neural computation." *Nat Rev Neurosci*, 2012.
3. **Reynolds, J. H. & Heeger, D. J.** "The normalization model of attention." *Neuron*, 2009.
4. **Louie, K. & Glimcher, P. W.** "Efficient coding and the neural representation of value." *Ann NY Acad Sci*, 2012.
