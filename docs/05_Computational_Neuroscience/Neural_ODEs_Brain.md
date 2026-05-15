# 神经常微分方程与大脑 (Neural ODEs & Continuous-Time Models)

> *脑是连续时间动力系统,不是离散层。Neural ODE(Chen 2018)把 ResNet 视为 ODE 离散化,用连续动力学建模。与神经科学 rate model、conductance model 天然契合。连接 [Neural Population Dynamics](Neural_Population_Dynamics.md) 的 dynamical systems 视角与现代可微 ODE 求解。*
>
> **难度**:Advanced
> **前置知识**:[Neural Population Dynamics](Neural_Population_Dynamics.md)、ODE、自动微分

---

## 1. 动机

- 生物 neuron 连续时间演化(膜电位 ODE)
- 离散 RNN/ResNet = ODE 的 Euler 离散
- 直接用连续动力学:适配不规则采样、可变深度

---

## 2. Neural ODE (Chen 2018)

ResNet:$h_{t+1} = h_t + f(h_t, \theta)$
→ 极限:
$$\frac{dh(t)}{dt} = f(h(t), t, \theta)$$

- 用 ODE solver(RK4、Dopri5)前向
- **Adjoint method**:O(1) 内存反传(解伴随 ODE)
- "无限深"网络

---

## 3. 与神经科学模型同构

| 神经科学 | Neural ODE |
|---|---|
| Rate model $\tau\dot r = -r + f(Wr)$ | RNN ODE |
| Conductance (HH) | stiff ODE |
| Neural mass | low-dim ODE |
| Population dynamics | latent ODE |

→ 计算神经科学一直在用 ODE;Neural ODE 给了可微 + 可学框架。

---

## 4. PyTorch — Neural ODE (rate model)

```python
import torch
# pip install torchdiffeq
from torchdiffeq import odeint

class RateODE(torch.nn.Module):
    """Continuous-time rate network: tau dr/dt = -r + tanh(W r + b)."""
    def __init__(self, n=50, tau=10.0):
        super().__init__()
        self.W = torch.nn.Parameter(torch.randn(n, n) * 0.1)
        self.b = torch.nn.Parameter(torch.zeros(n))
        self.tau = tau
    def forward(self, t, r):
        return (-r + torch.tanh(r @ self.W.t() + self.b)) / self.tau

model = RateODE()
r0 = torch.randn(50)
t = torch.linspace(0, 50, 100)
traj = odeint(model, r0, t)   # continuous-time trajectory
```

---

## 5. Latent ODE — 不规则采样

- 神经/行为数据常不规则采样
- Latent ODE(Rubanova 2019):encoder → latent ODE → decoder
- 适合 spike trains、临床时序
- 优于离散 RNN 处理 missing / irregular

---

## 6. Stiff ODE 问题

- HH 类型 conductance model 是 stiff(快 Na + 慢通道)
- 需 implicit solver(隐式 RK、BDF)
- 显式 solver 步长极小 → 慢
- 见 [Compartmental Models](Compartmental_Models.md)

---

## 7. 与 RNN / 连续 RNN

- Continuous-time RNN(CTRNN)早于 Neural ODE(神经科学常用)
- Neural ODE = CTRNN + 现代 ODE solver + adjoint
- "Liquid Time-Constant Networks"(Hasani 2021):自适应 τ,bio-inspired
- ODE-RNN 混合处理离散观测

---

## 8. 优缺点

| 优 | 缺 |
|---|---|
| 连续时间(不规则数据) | 训练慢(solver 调用) |
| O(1) 内存(adjoint) | adjoint 数值不稳风险 |
| 与 dynamical systems 对齐 | stiff 难 |
| 自适应计算 | 比离散难调 |

---

## 9. 神经科学应用

- 拟合 neural population trajectory(latent dynamics)
- LFADS-like:推断 latent ODE 解释 spikes
- 连续控制 / motor model
- 疾病动力学(连续时间疾病进展)

---

## 10. Common Pitfalls

### 10.1 Neural ODE 全新

连续时间神经模型在计算神经科学已数十年(CTRNN)。

### 10.2 ODE solver 任选

Stiff(HH)必 implicit;否则发散或极慢。

### 10.3 Adjoint 永远省内存

数值误差可累积;有时 backprop-through-solver 更稳。

### 10.4 连续 = 更生物

形式更近,但仍是抽象;不自动等于机制正确。

### 10.5 比 RNN 总更好

许多任务离散 RNN 更快更准;ODE 适不规则 / 连续场景。

---

## 11. Related Concepts

- **同节**:[Neural Population Dynamics](Neural_Population_Dynamics.md)、[Attractor Networks](Attractor_Networks.md)、[Compartmental Models](Compartmental_Models.md)
- **细胞**:[Hodgkin Huxley](../02_Cellular_Molecular/Hodgkin_Huxley.md)
- **AI**: ResNet、ODE solver、可微编程

---

## References

1. **Chen, R. T. Q. et al.** "Neural ordinary differential equations." *NeurIPS*, 2018.
2. **Rubanova, Y. et al.** "Latent ODEs for irregularly-sampled time series." *NeurIPS*, 2019.
3. **Hasani, R. et al.** "Liquid time-constant networks." *AAAI*, 2021.
4. **Pandarinath, C. et al.** "Inferring single-trial neural population dynamics using sequential auto-encoders (LFADS)." *Nat Methods*, 2018.
