# 神经编码 (Neural Coding)

> *神经编码研究 neuron 如何用 spike 表示信息。Rate coding (Adrian 1926) vs temporal coding vs population coding vs sparse coding。理解 coding 是连接 single-neuron 与 behavior、也是 brain-AI 比较的核心。Decoding (从 spike 重建 stimulus) 是 BCI 基础。*
>
> **难度**:Intermediate
> **前置知识**:[Action Potential](../02_Cellular_Molecular/Action_Potential.md)、概率基础

---

## 1. 编码方案

| 方案 | 信息载体 |
|---|---|
| **Rate coding** | spike 频率 |
| **Temporal coding** | precise spike timing |
| **Population coding** | 群体活动模式 |
| **Sparse coding** | 少数 neuron 活跃 |
| **Phase coding** | 相对 oscillation 相位 |

---

## 2. Rate Coding (Adrian 1926)

- 信息 = firing rate(spike / 秒)
- 强 stimulus → 高 rate
- 简单 robust
- 但慢(需积分窗 ~ 100 ms)

$$r = \frac{n_{\text{spikes}}}{\Delta T}$$

---

## 3. Temporal Coding

- Precise spike timing 携带信息
- 例:听觉 phase locking (< 1 ms 精度)
- 快(单 spike 即信息)
- Spike-timing-dependent plasticity (STDP) 利用此

---

## 4. Population Coding

- 单 neuron noisy → 群体平均
- **Population vector** (Georgopoulos 1986):M1 运动方向
$$\vec{P} = \sum_i r_i \vec{c}_i$$
- 冗余 + robust + 快

---

## 5. Sparse Coding

- 任意时刻仅少数 neuron 活跃
- 能量高效 + 高容量
- Olshausen & Field 1996:sparse coding 自然 emerge V1 Gabor-like filter
- 与 autoencoder sparsity 关联

---

## 6. Tuning Curve

- Neuron response vs stimulus parameter
- 例:V1 orientation tuning(bell-shaped)
- $r(\theta) = r_{\max} \exp\left(-\frac{(\theta - \theta_{pref})^2}{2\sigma^2}\right)$

---

## 7. PyTorch — Population Decoding

```python
import torch

def population_decode(rates, preferred_dirs):
    """Population vector decoding (Georgopoulos)."""
    # rates: (N,) firing rates; preferred_dirs: (N, 2) unit vectors
    pop_vector = (rates.unsqueeze(1) * preferred_dirs).sum(dim=0)
    decoded_angle = torch.atan2(pop_vector[1], pop_vector[0])
    return decoded_angle

N = 100
preferred = torch.randn(N, 2)
preferred = preferred / preferred.norm(dim=1, keepdim=True)
true_dir = torch.tensor([1.0, 0.0])
rates = torch.relu((preferred @ true_dir)) * 10  # cosine tuning
print(population_decode(rates, preferred))  # ≈ 0 rad
```

---

## 8. Information Theory

- **Mutual information**: $I(S; R) = H(R) - H(R|S)$
- 衡量 spike train 携带多少 stimulus 信息
- **Fisher information**: 编码精度下界(Cramér-Rao)

---

## 9. Noise + Variability

- Spike count 常 Poisson-like(variance ≈ mean)
- **Fano factor** = var/mean ≈ 1
- Noise correlation 影响 population coding 效率
- Trial-to-trial variability 是 active research

---

## 10. 与 AI 对应

| Brain | AI |
|---|---|
| Rate coding | ANN continuous activation |
| Temporal coding | SNN spike timing |
| Population coding | distributed representation |
| Sparse coding | sparse autoencoder, L1 |
| Tuning curve | receptive field / feature detector |

---

## 11. Common Pitfalls

### 11.1 Rate vs temporal 二选一

实际 brain 混合用,依任务。

### 11.2 单 neuron = 单概念("grandmother cell")

少数证据(Quiroga concept cells),但多数 distributed。

### 11.3 高 firing = 重要

Sparse coding 中,沉默也是信息。

### 11.4 Poisson 假设普适

许多 neuron sub/super-Poisson;假设需验证。

### 11.5 Decoding = brain 这样做

Decoder 是观察者工具;不代表 downstream neuron 用同法。

---

## 12. Related Concepts

- **同节**:[Levels of Analysis](Levels_of_Analysis.md)、[Research Methods](Research_Methods.md)
- **基础**:[Action Potential](../02_Cellular_Molecular/Action_Potential.md)
- **计算**:[SNN](../05_Computational_Neuroscience/Spiking_Neural_Networks.md)、[Grid Cells](../05_Computational_Neuroscience/Grid_Cells.md)
- **系统**:[Motor System](../03_Systems_Neuroscience/Motor_System.md)

---

## References

1. **Adrian, E. D.** "The impulses produced by sensory nerve endings." *J Physiol*, 1926.
2. **Georgopoulos, A. P. et al.** "Neuronal population coding of movement direction." *Science*, 1986.
3. **Olshausen, B. A. & Field, D. J.** "Emergence of simple-cell receptive field properties by learning a sparse code." *Nature*, 1996.
4. **Dayan, P. & Abbott, L. F.** *Theoretical Neuroscience*. MIT Press, 2001.
