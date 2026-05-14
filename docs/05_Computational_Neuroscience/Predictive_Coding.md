# 预测编码 (Predictive Coding)

> *Predictive Coding 1990s Rao & Ballard 提出:皮层不被动接收 input,而**主动预测** input → only **error** 上传。Friston 后将其与 free energy 统一。是脑计算 + AI 交叉的核心 framework。*
>
> **难度**:Advanced
> **前置知识**:[Hodgkin-Huxley](../02_Cellular_Molecular/Hodgkin_Huxley.md)、[Cortex](../01_Neuroanatomy/Cortex.md)

---

## 1. 核心思想

```
上层 → 预测 → 下层
        ↑
      error (实际 - 预测)
        |
下层 → 上传 error → 上层 update
```

层级:
- L1 预测 sensory input
- L2 预测 L1
- ... up to PFC

→ Only **prediction error** 沿层级上传。

---

## 2. 数学 (Rao & Ballard 1999)

每层 representation $r_i$,prediction $\hat{r}_{i-1} = f(r_i)$:

$$\dot{r}_i = -\frac{\partial F}{\partial r_i} = (r_{i-1} - \hat{r}_{i-1}) \cdot f'(r_i) - (r_i - g(r_{i+1}))$$

→ minimize free energy = squared prediction error。

---

## 3. 神经实现

Cortex 6 层:
- Layer 2/3: prediction signal
- Layer 5/6: error signal
- 上下投射 + 横向 inhibition 实现 prediction-error 循环

---

## 4. 与 VAE / Free Energy

Friston FEP:

$$F = D_{KL}(q(s) \| p(s)) - \mathbb{E}_q[\log p(o|s)]$$

minimizing F ⟺ minimizing prediction error。

VAE ELBO 数学等价 (见 WM_Predictive_Coding.md)。

---

## 5. 与 AI

- DeepMind PredNet (2017): 实现 Rao-Ballard 在 video prediction
- LeCun JEPA 路线:predict latent (不 reconstruct pixel)
- World Models 路径

---

## 6. 实证支持

- **MisMatch Negativity (MMN)**: EEG paradigm, 反映 prediction error
- **fMRI omitted stimulus**: surprise 触发活动
- **Visual illusions**: 解释为 prior overriding sensory

---

## 7. PyTorch — Hierarchical Predictive Coding

```python
import torch
import torch.nn as nn

class PredictiveCoderLayer(nn.Module):
    def __init__(self, dim_lower, dim_upper):
        super().__init__()
        self.predict_down = nn.Linear(dim_upper, dim_lower)
        self.update_up = nn.Linear(dim_lower, dim_upper)
        self.r_upper = None
    
    def forward(self, r_lower):
        if self.r_upper is None:
            self.r_upper = torch.zeros(r_lower.size(0), self.update_up.out_features)
        # Predict lower
        pred_lower = self.predict_down(self.r_upper)
        # Error
        error = r_lower - pred_lower
        # Update upper representation
        self.r_upper = self.r_upper + 0.1 * self.update_up(error)
        return error, self.r_upper
```

---

## 8. 病理

- **Schizophrenia**: 错误 prediction → 幻觉 (prediction 过度 dominant)
- **Autism**: prediction 不足 → hyper-sensitive to detail
- **Dyslexia**: 听觉 prediction 异常

---

## 9. 历史

- **1958** — Helmholtz "unconscious inference" 启发
- **1999** — Rao & Ballard predictive coding for V1
- **2005** — Friston Free Energy Principle
- **2010s** — Predictive coding 解释多种现象
- **2020s** — 与 AI World Models 合流

---

## 10. Common Pitfalls

### 10.1 不是唯一 framework

也有 sparse coding, efficient coding 等其他理论。

### 10.2 实证有限

特定 prediction error neurons 难定位。

### 10.3 与 backprop 关系

理论可计算 backprop,但实现不一致。

### 10.4 抽象 vs 具体

许多 high-level prediction coding 难落实到 cortex。

### 10.5 不解释 hard problem

仅 functional 层面,不解释 subjective experience。

---

## 11. Related Concepts

- **AI**: [WM Predictive Coding](https://jeffliulab.github.io/ai-notes/01_AI/03_Frontiers/03_World_Models/WM_Predictive_Coding/)

---

## References

1. **Rao, R. P. N. & Ballard, D. H.** "Predictive coding in the visual cortex." *Nat Neurosci*, 1999.
2. **Friston, K.** "The free-energy principle." *Nat Rev Neurosci*, 2010.
3. **Lotter, W. et al.** "Deep Predictive Coding Networks (PredNet)." *ICLR*, 2017.
4. **Seth, A.** *Being You*. 2021.
