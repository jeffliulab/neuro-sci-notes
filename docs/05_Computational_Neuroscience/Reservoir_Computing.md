# 储备池计算 (Reservoir Computing)

> *Reservoir computing(Echo State Network, Jaeger 2001;Liquid State Machine, Maass 2002)用一个随机固定的 recurrent 网络作"储备池",只训练读出层。高维非线性投影 + 简单线性读出。生物对应:cerebellum、cortical microcircuit 可能作 reservoir。是 RNN 训练难题的优雅旁路。*
>
> **难度**:Advanced
> **前置知识**:[Attractor Networks](Attractor_Networks.md)、RNN 基础

---

## 1. 核心思想

```
input → [随机固定 recurrent reservoir] → 只训读出层 (linear)
```

- Reservoir:大、随机、固定、recurrent → 高维动态状态
- 只训 readout(linear regression)→ 训练极快、稳定
- 避开 BPTT(backprop through time)难题

---

## 2. 两大流派

| | Echo State Network (Jaeger) | Liquid State Machine (Maass) |
|---|---|---|
| 单元 | rate neuron | spiking neuron |
| 起源 | 机器学习 | 计算神经科学 |
| 时间 | 离散 | 连续 spike |

---

## 3. Echo State Property

- Reservoir 状态须 asymptotically 只依赖输入历史(初始条件遗忘)
- 通过谱半径 $\rho(W) < 1$(约)保证
- 太小 → 无记忆;太大 → chaos(edge of chaos 最佳)

---

## 4. PyTorch — Echo State Network

```python
import torch

class EchoStateNetwork:
    def __init__(self, n_in, n_res=300, n_out=1, rho=0.95):
        self.Win = torch.randn(n_res, n_in) * 0.5
        W = torch.randn(n_res, n_res)
        # Scale spectral radius
        eig = torch.linalg.eigvals(W).abs().max()
        self.W = W * (rho / eig)
        self.Wout = None
        self.n_res = n_res
    
    def run(self, inputs):
        x = torch.zeros(self.n_res)
        states = []
        for u in inputs:
            x = torch.tanh(self.Win @ u + self.W @ x)
            states.append(x.clone())
        return torch.stack(states)
    
    def train_readout(self, inputs, targets, reg=1e-4):
        S = self.run(inputs)
        # Ridge regression readout (the ONLY trained part)
        self.Wout = torch.linalg.solve(
            S.t() @ S + reg * torch.eye(self.n_res), S.t() @ targets)
```

---

## 5. Edge of Chaos

- 最佳计算能力在 ordered ↔ chaotic 临界
- 记忆 + 非线性分离 平衡点
- Bertschinger & Natschläger 2004
- 生物可能调到临界(criticality 假说)

---

## 6. 生物对应

- **Cerebellum**:granule cell 层 = 高维随机扩展(Marr-Albus + reservoir 视角)
- **Cortical microcircuit**:Maass 提议 cortex 是 liquid computer
- **前额叶**:mixed selectivity = 类 reservoir 高维投影(见 [Neural Population Dynamics](Neural_Population_Dynamics.md))

---

## 7. FORCE Learning (Sussillo & Abbott 2009)

- 训练 chaotic RNN:实时递归最小二乘调 readout + 反馈
- 让 chaotic reservoir 产生复杂目标输出
- 生物可塑似 reservoir + 强反馈学习
- 桥接 reservoir 与可训 RNN

---

## 8. 优缺点

| 优 | 缺 |
|---|---|
| 训练极快(只 linear) | reservoir 不优化 → 需够大 |
| 稳定(无 BPTT) | 超参敏感(ρ、scaling) |
| 时间序列强 | 不如端到端 RNN scale |
| 硬件友好(physical reservoir) | 任务特定调参 |

---

## 9. Physical / Neuromorphic Reservoir

- 任何高维非线性动态系统可作 reservoir:
  - 光子、忆阻器、自旋电子、机械、even 水桶
- 适合 edge / 低功耗 时序处理
- 与 neuromorphic computing 协同

---

## 10. Common Pitfalls

### 10.1 Reservoir 需训练

核心是**不**训 reservoir;只训 readout。

### 10.2 越大越好

需平衡 + echo state property;过大 / ρ>1 → chaos 失效。

### 10.3 = 普通 RNN

RNN 全训;reservoir 固定 recurrent。

### 10.4 Edge of chaos 神秘最优

是经验 + 理论倾向;非所有任务严格最优。

### 10.5 生物就是 reservoir

是假说 / 类比;cortex 也有学习的 recurrent 权重。

---

## 11. Related Concepts

- **同节**:[Attractor Networks](Attractor_Networks.md)、[Neural Population Dynamics](Neural_Population_Dynamics.md)、[Spiking Neural Networks](Spiking_Neural_Networks.md)
- **解剖**:[Cerebellum](../01_Neuroanatomy/Cerebellum.md)
- **AI**: RNN、neuromorphic、physical computing

---

## References

1. **Jaeger, H.** "The 'echo state' approach to analysing and training recurrent neural networks." *GMD Report*, 2001.
2. **Maass, W., Natschläger, T., Markram, H.** "Real-time computing without stable states (Liquid State Machine)." *Neural Comput*, 2002.
3. **Sussillo, D. & Abbott, L. F.** "Generating coherent patterns of activity from chaotic neural networks (FORCE)." *Neuron*, 2009.
4. **Lukoševičius, M. & Jaeger, H.** "Reservoir computing approaches to recurrent neural network training." *Comput Sci Rev*, 2009.
