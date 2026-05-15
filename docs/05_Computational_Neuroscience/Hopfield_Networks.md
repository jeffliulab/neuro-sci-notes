# Hopfield 网络 (Hopfield Networks)

> *Hopfield 1982 提出的能量基础递归神经网络。Pattern 存储为 attractor state,recall = energy 下降到 fixed point。Inspire CAM (content-addressable memory) + 现代 Hopfield network (Ramsauer 2020 attention 联系)。是 Modern AI Transformer attention 的 unfair "ancestor"。Hopfield 与 Hinton 2024 Nobel 物理奖共同获得。*
>
> **难度**:Advanced
> **前置知识**:[LTP_LTD](../02_Cellular_Molecular/LTP_LTD.md)、[Hippocampus_Anatomy](../01_Neuroanatomy/Hippocampus_Anatomy.md)

---

## 1. 基本概念

- N 个 binary 神经元 $s_i \in \{-1, +1\}$
- 全连接 + symmetric weights $W_{ij} = W_{ji}$, $W_{ii} = 0$
- Energy function:

$$E = -\frac{1}{2} \sum_{i \neq j} W_{ij} s_i s_j$$

- Recall dynamics:每步随机选 neuron 翻转,只保留降低 E 的翻转
- Pattern 存储为 local minima of E

---

## 2. Hebbian Storage

存储 P 个 pattern $\{\xi^\mu\}_{\mu=1}^P$:

$$W_{ij} = \frac{1}{N} \sum_{\mu=1}^P \xi_i^\mu \xi_j^\mu$$

- Capacity: $P_{\max} \approx 0.138 N$ (Amit 1985)
- 超过 capacity → spurious mixed states

---

## 3. Recall 过程

1. 输入 noisy / partial pattern as initial $s(0)$
2. 异步更新: $s_i \leftarrow \text{sign}(\sum_j W_{ij} s_j)$
3. 网络 converge 到最近 attractor
4. 输出 clean pattern

---

## 4. PyTorch — Hopfield

```python
import torch

class HopfieldNet:
    def __init__(self, N):
        self.W = torch.zeros(N, N)
        self.N = N
    
    def store(self, patterns):
        """Hebbian rule: W = (1/N) Σ ξ ξ^T."""
        for p in patterns:
            self.W += torch.outer(p, p) / self.N
        self.W.fill_diagonal_(0)
    
    def recall(self, s, steps=100):
        """Async update."""
        for _ in range(steps):
            i = torch.randint(0, self.N, (1,)).item()
            s[i] = torch.sign(self.W[i] @ s)
        return s
    
    def energy(self, s):
        return -0.5 * (s @ self.W @ s)
```

---

## 5. 与神经科学对应

- **CA3 in Hippocampus**: 高度 recurrent,常被建模为 Hopfield-like attractor
- **Pattern completion**: 海马功能与 Hopfield recall 类似
- **Episodic memory**: pattern 存储 + recall
- McNaughton & Morris (1987) 提出 CA3 实现 attractor network

---

## 6. Modern Hopfield Network (Ramsauer 2020)

- Continuous (非 binary) Hopfield network
- Capacity 指数 scale($\sim e^N$)
- Update rule **等价于 Transformer attention**:

$$s^{\text{new}} = X \cdot \text{softmax}(\beta X^T s)$$

- 即 Hopfield 是 Transformer 隐含基础

---

## 7. 与 Boltzmann Machine

- Hopfield (deterministic) + temperature → Boltzmann machine (probabilistic)
- Hinton & Sejnowski 1985 引入 hidden units → RBM → 现代 DL
- 2024 Nobel 物理给 Hopfield + Hinton (基础贡献)

---

## 8. Spin Glass 类比

- Hopfield 网络是 Ising model 类似
- Statistical physics: free energy landscape
- Replica trick (Amit, Gutfreund, Sompolinsky 1985) 给 capacity 严格 bound

---

## 9. 局限

- Binary neuron 不实
- Capacity 0.138N 太低
- Spurious states 多
- Async update 慢
- Modern Hopfield + attention 已基本取代

---

## 10. Common Pitfalls

### 10.1 Storing > 0.138N

存储太多 pattern → 完全混乱,无法 recall。

### 10.2 W 非 Symmetric

破坏 energy 保证;可能无 convergence。

### 10.3 Energy 单调下降

仅 async + 唯一 flip 时;sync 更新可能 oscillate。

### 10.4 Hopfield = LSTM 错误

LSTM 是 RNN 通过 gated memory;Hopfield 是 attractor-based associative。

### 10.5 现代 Hopfield ≠ 经典

Continuous 版本 capacity 指数 scale,关联 Transformer。

---

## 11. Related Concepts

- **同节**:[Predictive_Coding](Predictive_Coding.md)、[Bayesian_Brain](Bayesian_Brain.md)
- **解剖**:[Hippocampus](../01_Neuroanatomy/Hippocampus_Anatomy.md)
- **细胞**:[LTP_LTD](../02_Cellular_Molecular/LTP_LTD.md)
- **AI**: Transformer attention — https://jeffliulab.github.io/ai-notes/02_Deep_Learning/05_Transformers/

---

## References

1. **Hopfield, J. J.** "Neural networks and physical systems with emergent collective computational abilities." *PNAS*, 1982.
2. **Amit, D. J., Gutfreund, H., Sompolinsky, H.** "Storing infinite numbers of patterns in a spin-glass model of neural networks." *Phys Rev Lett*, 1985.
3. **Ramsauer, H. et al.** "Hopfield networks is all you need." *ICLR*, 2021.
4. **Hertz, Krogh, Palmer** *Introduction to the Theory of Neural Computation*, 1991.
