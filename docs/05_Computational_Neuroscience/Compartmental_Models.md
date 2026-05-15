# 房室模型 (Compartmental Models)

> *Compartmental modeling 把 neuron 分割成多个等电位"房室",每个用 cable equation + Hodgkin-Huxley 通道描述,房室间电耦合。是 biophysically detailed simulation 标准(NEURON、Arbor)。从单房室 LIF 到 morphologically detailed Blue Brain reconstruction。连接 [Dendrites](../02_Cellular_Molecular/Dendrites.md) 与计算。*
>
> **难度**:Advanced
> **前置知识**:[Hodgkin Huxley](../02_Cellular_Molecular/Hodgkin_Huxley.md)、[Dendrites](../02_Cellular_Molecular/Dendrites.md)、微分方程

---

## 1. 基本思想

- Neuron 形态复杂 → 离散成 N 个 isopotential compartment
- 每房室:膜电容 + 离子通道 + 漏电
- 相邻房室:轴向电阻耦合
- 解大型 ODE 系统

---

## 2. Cable Equation

$$\frac{\partial V}{\partial t} = \frac{a}{2 R_i C_m} \frac{\partial^2 V}{\partial x^2} - \frac{V}{R_m C_m} + \frac{I_{\text{ion}}}{C_m}$$

- $\lambda = \sqrt{a R_m / 2 R_i}$:length constant
- $\tau = R_m C_m$:time constant
- 离散化 → 房室方程

---

## 3. 单房室 → 多房室

| 模型 | 房室 | 用途 |
|---|---|---|
| LIF | 1(point) | 网络仿真(快) |
| Izhikevich | 1 | 丰富 dynamics + 快 |
| Hodgkin-Huxley | 1 | 单细胞 AP |
| 2-3 compartment | soma+dendrite | dendritic 计算简化 |
| Morphological | 数百-数千 | Blue Brain 级真实 |

---

## 4. 房室方程

每房室 i:
$$C_m \frac{dV_i}{dt} = -I_{\text{ion},i} + \sum_{j \in \text{neighbors}} \frac{V_j - V_i}{R_{ij}} + I_{\text{ext}}$$

- 离子电流 = HH 类型(Na、K、Ca、…)
- 耦合项 = 房室间电流

---

## 5. PyTorch — 3-Compartment 仿真

```python
import torch

def three_compartment(T=1000, dt=0.025, I_dend=0.0, I_soma=2.0):
    """soma - prox dendrite - dist dendrite, coupled."""
    V = torch.tensor([-65.0, -65.0, -65.0])  # soma, prox, dist
    g_couple = torch.tensor([0.5, 0.3])       # soma-prox, prox-dist
    trace = []
    for t in range(T):
        leak = -0.3 * (V + 65.0)
        coup = torch.zeros(3)
        coup[0] = g_couple[0] * (V[1] - V[0])
        coup[1] = g_couple[0]*(V[0]-V[1]) + g_couple[1]*(V[2]-V[1])
        coup[2] = g_couple[1] * (V[1] - V[2])
        I = torch.tensor([I_soma, 0.0, I_dend])
        V = V + dt * (leak + coup + I)
        if V[0] > -50:                # simple soma spike + reset
            V[0] = -65.0
        trace.append(V.clone())
    return torch.stack(trace)
```

---

## 6. 仿真工具

| 工具 | 特点 |
|---|---|
| **NEURON** | 经典,Hines method,标准 |
| **Arbor** | 现代,GPU/HPC,多核 |
| **Brian2** | Python,易用 |
| **NEST** | 大网络 point-neuron |
| **GENESIS** | 老牌 |
| **CoreNEURON** | NEURON GPU 加速 |

---

## 7. 数值方法

- **Hines method**:利用树状拓扑 → O(N) 解三对角(非 O(N³))
- Implicit (Crank-Nicolson) 保稳定
- 自适应步长(刚性方程)
- 大网络:并行 + 域分解

---

## 8. Blue Brain / 详细重建

- Markram Blue Brain:morphologically + biophysically detailed cortical column
- 每 neuron 数百-数千房室 + 多通道
- 争议:细节是否必要?vs 简化模型
- 教训:模型粒度依问题(见 [Levels of Analysis](../00_Foundations/Levels_of_Analysis.md))

---

## 9. 简化 vs 细节权衡

| | 简化 (LIF) | 详细 (multi-comp) |
|---|---|---|
| 速度 | 快(百万 neuron) | 慢 |
| 解释力 | 网络层面 | 单细胞机制 |
| 参数 | 少 | 多(难约束) |
| 用 | 大规模网络 | dendritic 计算、药理 |

---

## 10. Common Pitfalls

### 10.1 越详细越对

参数过多 → 难约束 + 过拟合;粒度依问题。

### 10.2 Point neuron 够

忽略 dendritic computation(见 [Dendrites](../02_Cellular_Molecular/Dendrites.md))。

### 10.3 显式积分 OK

HH 刚性 → 需 implicit / 小步长,否则发散。

### 10.4 房室越多越准

需匹配 length constant;过粗失真,过细浪费。

### 10.5 仿真 = 真实

参数 + 通道分布不确定;须实验约束 + 验证。

---

## 11. Related Concepts

- **同节**:[Spiking Neural Networks](Spiking_Neural_Networks.md)、[Reservoir Computing](Reservoir_Computing.md)
- **细胞**:[Hodgkin Huxley](../02_Cellular_Molecular/Hodgkin_Huxley.md)、[Dendrites](../02_Cellular_Molecular/Dendrites.md)、[Ion Channels](../02_Cellular_Molecular/Ion_Channels.md)
- **基础**:[Levels of Analysis](../00_Foundations/Levels_of_Analysis.md)

---

## References

1. **Rall, W.** "Cable theory for dendritic neurons." In *Methods in Neuronal Modeling*, 1989.
2. **Hines, M. L. & Carnevale, N. T.** "The NEURON simulation environment." *Neural Comput*, 1997.
3. **Markram, H. et al.** "Reconstruction and simulation of neocortical microcircuitry." *Cell*, 2015.
4. **Koch, C.** *Biophysics of Computation*. Oxford, 1999.
