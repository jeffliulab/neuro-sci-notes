# 能量基础模型与大脑 (Energy-Based Models & the Brain)

> *Energy-based models (EBM) 用 energy landscape 定义状态分布:低能量 = 高概率。Hopfield → Boltzmann machine → 现代 EBM/diffusion。2024 Nobel 物理给 Hopfield + Hinton 正基于此。生物对应:attractor dynamics 是能量下降。连接 [Hopfield Networks](Hopfield_Networks.md)、[Free Energy Principle](Free_Energy_Principle.md) 与 generative AI。*
>
> **难度**:Advanced
> **前置知识**:[Hopfield Networks](Hopfield_Networks.md)、[Free Energy Principle](Free_Energy_Principle.md)、统计物理基础

---

## 1. 核心思想

$$p(x) = \frac{1}{Z} e^{-E(x)/T}$$

- $E(x)$:energy function(低 = 偏好状态)
- $Z$:partition function(配分函数,难算)
- $T$:temperature
- 学习 = 塑造 energy landscape 使数据处低能量

---

## 2. 谱系

| 模型 | 特点 |
|---|---|
| **Hopfield** (1982) | deterministic、associative memory |
| **Boltzmann machine** (1985) | + 随机 + hidden units |
| **RBM** | 二部图、对比散度训练 |
| **Deep Boltzmann / DBN** | 深层(2006 DL 复兴) |
| **Modern EBM** | score matching、Langevin |
| **Diffusion models** | EBM 的连续 score 视角 |

---

## 3. Boltzmann Machine

- 随机二值单元 + 对称权重
- 平衡分布 = Boltzmann distribution
- Hidden units → 学复杂分布
- Hinton & Sejnowski 1985
- 训练慢(需 sampling)→ RBM + CD 加速

---

## 4. 生物对应

- **Attractor dynamics** = energy 下降到 minima(见 [Attractor Networks](Attractor_Networks.md))
- **Hopfield 记忆** = energy 谷 = stored pattern
- **Free energy principle**:variational free energy 最小化(见 [Free Energy Principle](Free_Energy_Principle.md))
- 突触 = 塑造 energy landscape

---

## 5. PyTorch — RBM (Contrastive Divergence)

```python
import torch

class RBM:
    def __init__(self, n_vis, n_hid):
        self.W = torch.randn(n_vis, n_hid) * 0.01
        self.b_v = torch.zeros(n_vis)
        self.b_h = torch.zeros(n_hid)
    def sample_h(self, v):
        p = torch.sigmoid(v @ self.W + self.b_h)
        return p, (torch.rand_like(p) < p).float()
    def sample_v(self, h):
        p = torch.sigmoid(h @ self.W.t() + self.b_v)
        return p, (torch.rand_like(p) < p).float()
    def cd1(self, v0, lr=0.01):
        ph0, h0 = self.sample_h(v0)
        _, v1 = self.sample_v(h0)
        ph1, _ = self.sample_h(v1)
        self.W += lr * (v0.t() @ ph0 - v1.t() @ ph1) / v0.size(0)
```

---

## 6. 2024 Nobel 物理

- Hopfield + Hinton 获奖
- Hopfield network(能量基础联想记忆)
- Boltzmann machine(随机 EBM,DL 先驱)
- 表彰 statistical-physics × neural networks 基础贡献

---

## 7. Energy ↔ Inference

- 推断 = energy 最小化(MAP)或 sampling(posterior)
- Langevin dynamics:$dx = -\nabla E(x) dt + \sqrt{2T}\,dW$
- 类比:神经动力学下降 energy → 推断
- 连接 predictive coding(prediction error = energy 梯度)

---

## 8. 与 Diffusion / 现代生成

- Diffusion model = 学 score $\nabla \log p = -\nabla E$
- EBM 复兴(score matching 绕开 Z)
- 大脑 generative model 假说:用 energy landscape 生成预测(见 [Free Energy Principle](Free_Energy_Principle.md))

---

## 9. 难点 — Partition Function

- $Z = \sum_x e^{-E(x)}$ 通常 intractable
- 解法:MCMC、对比散度、score matching、NCE
- 生物如何"算 Z"?→ 可能不需(只需 relative energy / 局部梯度)

---

## 10. Common Pitfalls

### 10.1 EBM = Hopfield

Hopfield 是最简 EBM;还有 stochastic、deep、score-based。

### 10.2 能量有物理意义

是抽象 scalar(借统计物理形式);非真实能量。

### 10.3 大脑算 partition function

可能只用局部梯度 / 相对能量,无需全局 Z。

### 10.4 Boltzmann machine 实用

原始训练极慢;RBM/CD + 现代 score matching 才实用。

### 10.5 Energy minima = 唯一计算

也有 transient / 非平衡计算;EBM 是一种视角。

---

## 11. Related Concepts

- **同节**:[Hopfield Networks](Hopfield_Networks.md)、[Attractor Networks](Attractor_Networks.md)、[Free Energy Principle](Free_Energy_Principle.md)、[Predictive Coding](Predictive_Coding.md)
- **细胞**:[LTP_LTD](../02_Cellular_Molecular/LTP_LTD.md)
- **AI**: diffusion models、score matching、generative models

---

## References

1. **Hopfield, J. J.** "Neural networks and physical systems with emergent collective computational abilities." *PNAS*, 1982.
2. **Ackley, D. H., Hinton, G. E., Sejnowski, T. J.** "A learning algorithm for Boltzmann machines." *Cogn Sci*, 1985.
3. **LeCun, Y. et al.** "A tutorial on energy-based learning." *Predicting Structured Data*, 2006.
4. **Song, Y. & Ermon, S.** "Generative modeling by estimating gradients of the data distribution." *NeurIPS*, 2019.
