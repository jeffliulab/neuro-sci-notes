# 神经群体动力学 (Neural Population Dynamics)

> *现代 systems neuroscience 范式转变:从单 neuron tuning 到 population state space + low-dimensional manifold + dynamics。Churchland 2012:M1 不编码运动参数,而是 dynamical system 生成 muscle command。PCA/jPCA/dPCA + dynamical systems 是工具。RNN 是理论模型。*
>
> **难度**:Advanced
> **前置知识**:[Neural Coding](../00_Foundations/Neural_Coding.md)、[Attractor Networks](Attractor_Networks.md)、线性代数

---

## 1. 范式转变

| 旧 (single-neuron) | 新 (population) |
|---|---|
| 每 neuron 编码一变量 | 群体 state 在低维 manifold |
| Tuning curve | Neural trajectory |
| 平均 firing rate | Dynamics（如何演化） |
| 还原论 | 涌现的 collective computation |

---

## 2. State Space

- N neuron → N 维空间,每时刻一点
- 群体活动 = 轨迹(trajectory)
- 实际维度 ≪ N(low-dimensional manifold)
- 降维:PCA、Factor Analysis、jPCA、dPCA、UMAP

---

## 3. Low-Dimensional Manifold

- M1 数百 neuron,但 ~ 6-10 维捕获大部分方差
- "Neural manifold" 跨 trial / 时间稳定
- 学习 = manifold 内 vs 外移动(Sadtler 2014:within-manifold 易学,outside 难)

---

## 4. Churchland 2012 — Rotational Dynamics

- M1 准备 → 执行:群体活动呈 **旋转**(jPCA 揭示)
- 单 neuron 看似杂乱,群体层面规律
- M1 不是 representational(编码方向),是 dynamical(生成 pattern 的引擎)
- "Neural population as a machine, not a code"

---

## 5. Dynamical Systems 视角

$$\dot{\mathbf{x}} = F(\mathbf{x}, \mathbf{u})$$

- $\mathbf{x}$:低维 latent state
- 计算 = trajectory 演化(非瞬时映射)
- Fixed points、line attractor、rotation 等 motif
- 与 [Attractor Networks](Attractor_Networks.md) 同框架

---

## 6. PyTorch — jPCA 思想(旋转成分)

```python
import torch

def find_rotational_dynamics(X):
    """X: (T, N) population trajectory. Find skew-symmetric M: dX ≈ X M."""
    dX = X[1:] - X[:-1]
    Xc = X[:-1]
    # Solve least squares dX = Xc M, constrain M skew-symmetric
    M = torch.linalg.lstsq(Xc, dX).solution
    M_skew = 0.5 * (M - M.t())          # rotational part
    eigvals = torch.linalg.eigvals(M_skew)
    return M_skew, eigvals  # imaginary eigvals → rotation freq
```

---

## 7. dPCA — Demixed PCA

- 标准 PCA 混合 task variables
- dPCA(Kobak 2016):分解方差到 stimulus / decision / time 各成分
- 解释 mixed selectivity(单 neuron 编码多变量)

---

## 8. Mixed Selectivity

- PFC neuron 常对多变量非线性混合编码
- Rigotti 2013:high-dimensional mixed selectivity → 线性可读任意组合
- 类似 kernel trick / reservoir
- 群体层面才显现功能

---

## 9. RNN 作为模型

- 训 RNN 解任务 → 比对 neural data(同 dynamics?)
- Mante 2013:context-dependent 计算,RNN 与 PFC dynamics 一致
- 逆向工程 RNN(fixed points)→ 理解 brain
- "Computation through dynamics"(Vyas 2020 综述)

---

## 10. Common Pitfalls

### 10.1 单 neuron tuning 足够

群体 dynamics 揭示单 neuron 不可见的结构。

### 10.2 低维 = 简单

低维 manifold 上可有复杂非线性 dynamics。

### 10.3 PCA 成分有生物意义

PCA 轴是统计的,未必对应具体机制。

### 10.4 Manifold 固定

学习 / 任务可改变 manifold(虽 within-manifold 更易)。

### 10.5 Representation vs dynamics 二选一

互补视角;现代倾向 dynamics 但 representation 仍有用。

---

## 11. Related Concepts

- **同节**:[Attractor Networks](Attractor_Networks.md)、[Hopfield Networks](Hopfield_Networks.md)、[Reinforcement Learning Brain](Reinforcement_Learning_Brain.md)
- **基础**:[Neural Coding](../00_Foundations/Neural_Coding.md)、[Levels of Analysis](../00_Foundations/Levels_of_Analysis.md)
- **系统**:[Motor System](../03_Systems_Neuroscience/Motor_System.md)
- **AI**: RNN dynamics

---

## References

1. **Churchland, M. M. et al.** "Neural population dynamics during reaching." *Nature*, 2012.
2. **Mante, V. et al.** "Context-dependent computation by recurrent dynamics in prefrontal cortex." *Nature*, 2013.
3. **Rigotti, M. et al.** "The importance of mixed selectivity in complex cognitive tasks." *Nature*, 2013.
4. **Vyas, S. et al.** "Computation through neural population dynamics." *Annu Rev Neurosci*, 2020.
