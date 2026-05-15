# 吸引子网络 (Attractor Networks)

> *Attractor network 通过 recurrent 连接形成稳定 activity 状态(吸引子)。Point attractor(Hopfield 记忆)、line/ring attractor(头朝向、眼位)、continuous attractor(grid/place cells)。是 working memory、决策、空间表征的统一计算框架。Recurrent dynamics 是 brain 计算核心,也启发 RNN。*
>
> **难度**:Advanced
> **前置知识**:[Hopfield Networks](Hopfield_Networks.md)、动力系统基础

---

## 1. 吸引子类型

| 类型 | 几何 | 例 |
|---|---|---|
| Point attractor | 离散点 | Hopfield 记忆、决策 |
| Line attractor | 1D 连续线 | 眼位积分(oculomotor) |
| Ring attractor | 环 | 头朝向(HD cells) |
| Plane / torus | 2D | grid cells、空间 |
| Limit cycle | 周期轨道 | CPG 节律 |

---

## 2. 动力系统视角

$$\tau \dot{\mathbf{r}} = -\mathbf{r} + f(W\mathbf{r} + \mathbf{I})$$

- Fixed point:$\dot{\mathbf{r}} = 0$
- 稳定 fixed point = attractor
- Energy landscape:吸引子 = 谷底
- Basin of attraction:吸引域

---

## 3. Point Attractor — Working Memory

- Recurrent excitation 维持 persistent activity(无 input 也保持)
- DLPFC delay-period firing(见 [Working Memory](../04_Cognitive_Neuroscience/Working_Memory.md))
- Multiple discrete attractors = multiple memory states
- Decision:两 attractor 竞争(winner)

---

## 4. Ring Attractor — Head Direction

- Drosophila ellipsoid body "bump"(Seelig & Jayaraman 2015 直接观测!)
- 神经元按朝向环状排列 + 局部兴奋 + 全局抑制
- Bump 位置 = 当前朝向
- Self-motion → bump 移动(path integration)

---

## 5. Continuous Attractor — Grid/Place

- 2D continuous attractor 维持位置表征
- Grid cells:torus attractor
- Place cells:bump on 2D sheet
- 见 [Grid Cells](Grid_Cells.md)

---

## 6. PyTorch — Ring Attractor

```python
import torch

def ring_attractor(N=100, steps=200, drift=0.0):
    """1D ring attractor: bump tracks heading."""
    theta = torch.linspace(0, 2*torch.pi, N)
    # Mexican-hat connectivity: local excite, global inhibit
    diff = theta.unsqueeze(0) - theta.unsqueeze(1)
    W = torch.exp(torch.cos(diff)) - 0.6
    r = torch.exp(torch.cos(theta - torch.pi))  # initial bump
    for _ in range(steps):
        r = torch.relu(W @ r / N + drift)
        r = r / (r.sum() + 1e-6) * N * 0.3      # normalization
    return theta, r  # stable bump = attractor state
```

---

## 7. Balanced / 制衡

- 纯兴奋 recurrent → 爆炸
- 需 inhibition 平衡(E/I balance)
- Mexican-hat connectivity(近兴奋、远抑制)→ localized bump

---

## 8. 决策 = 吸引子竞争

- Wong & Wang 2006:两群体互相抑制 → winner-take-all
- 证据累积 → 跨越分水岭 → 落入一个 attractor
- 解释 reaction time 分布、speed-accuracy tradeoff

---

## 9. 与 RNN

- 训练的 RNN 常自发形成 attractor / line attractor 解任务
- Sussillo & Barak 2013:逆向工程 RNN → fixed point 分析
- Attractor 是 RNN 与 brain 共同的计算 primitive
- 现代:dynamical systems 视角看神经计算(Vyas 2020)

---

## 10. Common Pitfalls

### 10.1 Attractor = Hopfield only

Hopfield 只是 point attractor;还有 line/ring/continuous。

### 10.2 Persistent activity = 唯一 WM 机制

也有 activity-silent(短期突触)WM 理论。

### 10.3 Recurrent → 必不稳

E/I balance → 稳定 attractor。

### 10.4 吸引子静态

可移动(ring bump 随朝向);continuous attractor 动态。

### 10.5 一定有显式吸引子

部分计算是 transient dynamics(非吸引子);两者并存。

---

## 11. Related Concepts

- **同节**:[Hopfield Networks](Hopfield_Networks.md)、[Grid Cells](Grid_Cells.md)、[Neural Population Dynamics](Neural_Population_Dynamics.md)
- **认知**:[Working Memory](../04_Cognitive_Neuroscience/Working_Memory.md)、[Decision Making](../04_Cognitive_Neuroscience/Decision_Making.md)
- **基础**:[Neural Circuits](../00_Foundations/Neural_Circuits.md)

---

## References

1. **Amari, S.** "Dynamics of pattern formation in lateral-inhibition type neural fields." *Biol Cybern*, 1977.
2. **Seelig, J. D. & Jayaraman, V.** "Neural dynamics for landmark orientation and angular path integration." *Nature*, 2015.
3. **Wong, K.-F. & Wang, X.-J.** "A recurrent network mechanism of time integration in perceptual decisions." *J Neurosci*, 2006.
4. **Khona, M. & Fiete, I. R.** "Attractor and integrator networks in the brain." *Nat Rev Neurosci*, 2022.
