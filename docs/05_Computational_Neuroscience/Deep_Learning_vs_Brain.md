# 深度学习与大脑 (Deep Learning vs Brain)

> *Goal-driven deep network 已成 sensory cortex 最佳预测模型(Yamins & DiCarlo 2014:CNN 中层 ↔ IT firing r > 0.7)。但相似是表征层面,机制截然不同(backprop vs 局部规则;静态 vs 循环 + 反馈)。理解何处相似、何处不同,是 NeuroAI 核心。*
>
> **难度**:Advanced
> **前置知识**:[Visual System](../03_Systems_Neuroscience/Visual_System.md)、[Neural Population Dynamics](Neural_Population_Dynamics.md)、深度学习基础

---

## 1. Goal-Driven Modeling

- 训 CNN 做 ImageNet → 中层表征**预测 IT/V4 firing**
- Yamins & DiCarlo 2014:performance ↑ → brain predictivity ↑
- 不需 fit 神经数据;任务优化自然产生 brain-like 表征
- Brain-Score benchmark 量化

---

## 2. 表征相似度量

| 方法 | 思想 |
|---|---|
| **RSA** (Repr. Similarity Analysis) | 比较 representational dissimilarity matrix |
| **Linear encoding** | 用模型特征线性预测 neural |
| **CKA** | centered kernel alignment |
| **Brain-Score** | 综合 benchmark |

---

## 3. 哪里相似

- Ventral stream ↔ CNN 层级(V1→V4→IT vs conv 层)
- Auditory cortex ↔ audio CNN
- 语言区 ↔ LLM 表征(Schrimpf 2021:GPT 预测语言皮层)
- Grid-like code 在 RNN emerge(见 [Grid Cells](Grid_Cells.md))

---

## 4. 哪里不同(机制)

| 维度 | Brain | DL |
|---|---|---|
| 学习 | local rule、no backprop | backprop |
| 结构 | recurrent + feedback 重 | 多 feedforward |
| 神经元 | spiking、dendritic compute | scalar ReLU |
| 能量 | ~ 20 W | GPU kW |
| 数据 | few-shot | 海量 |
| 时间 | 连续动力学 | 离散层 |

---

## 5. PyTorch — RSA

```python
import torch

def rsa(features_a, features_b):
    """Representational Similarity Analysis between two systems."""
    def rdm(X):  # representational dissimilarity matrix
        X = X - X.mean(0)
        sim = X @ X.t()
        d = 1 - sim / (sim.diag().outer(sim.diag()).sqrt() + 1e-8)
        return d
    ra, rb = rdm(features_a), rdm(features_b)
    iu = torch.triu_indices(ra.size(0), ra.size(0), 1)
    va, vb = ra[iu[0], iu[1]], rb[iu[0], iu[1]]
    # Spearman-like correlation of RDMs
    return torch.corrcoef(torch.stack([va, vb]))[0, 1]
```

---

## 6. Backprop 的 bio plausibility

- 问题:weight transport、全局误差、双向、非局部
- 替代:feedback alignment、predictive coding、equilibrium prop、target prop
- 见 [Synaptic Plasticity Models](Synaptic_Plasticity_Models.md)
- Lillicrap 2020:大脑可能近似 backprop(争议)

---

## 7. 双向启发史

- CNN ← Hubel-Wiesel V1(1959)、Neocognitron(Fukushima 1980)
- RL ← dopamine RPE(见 [RL Brain](Reinforcement_Learning_Brain.md))
- Attention ← 视觉注意
- Hopfield/Boltzmann ← 统计物理 + neuro(2024 Nobel)
- 反哺:CNN → 理解 IT;RNN → 理解 PFC dynamics

---

## 8. NeuroAI 议程

- 用 DL 作 brain 的 image-computable model
- 用 neuroscience 改进 AI(robustness、efficiency、continual learning)
- "Embodied Turing test"(Zador 2023)
- Foundation models of neural data

---

## 9. 警示

- 高 predictivity ≠ 同机制(可能 degenerate solutions)
- Untrained network 也部分 predict(架构先验)
- Benchmark gaming 风险
- 相似性 metric 选择敏感

---

## 10. Common Pitfalls

### 10.1 CNN = visual cortex

仅近似;缺 recurrence、feedback、attention、adaptation。

### 10.2 高 r → 同算法

表征相似 ≠ 机制相同。

### 10.3 Backprop 不可能在脑

可能有近似;未定论(active research)。

### 10.4 LLM = 语言脑

部分表征相似;但 grounding、发育、能量天差。

### 10.5 NeuroAI = 把 DL 套 brain

双向:也用 neuro 原理改 AI。

---

## 11. Related Concepts

- **同节**:[Neural Population Dynamics](Neural_Population_Dynamics.md)、[Synaptic Plasticity Models](Synaptic_Plasticity_Models.md)、[Grid Cells](Grid_Cells.md)
- **系统**:[Visual System](../03_Systems_Neuroscience/Visual_System.md)
- **基础**:[Levels of Analysis](../00_Foundations/Levels_of_Analysis.md)
- **AI**: CNN/Transformer — https://jeffliulab.github.io/ai-notes/02_Deep_Learning/

---

## References

1. **Yamins, D. L. K. & DiCarlo, J. J.** "Using goal-driven deep learning models to understand sensory cortex." *Nat Neurosci*, 2016.
2. **Schrimpf, M. et al.** "The neural architecture of language: Integrative modeling converges on predictive processing." *PNAS*, 2021.
3. **Lillicrap, T. P. et al.** "Backpropagation and the brain." *Nat Rev Neurosci*, 2020.
4. **Zador, A. et al.** "Catalyzing next-generation artificial intelligence through NeuroAI." *Nat Commun*, 2023.
