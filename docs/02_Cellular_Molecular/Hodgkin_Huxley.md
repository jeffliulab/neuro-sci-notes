# Hodgkin-Huxley 方程 — 神经元生物物理模型

> *1952 年 Alan Hodgkin 与 Andrew Huxley 在乌贼巨型 axon 上推导出**精确数学方程**描述动作电位 — 4 个 ODE 完整 capture Na+/K+ channel dynamics。仍是计算神经科学的金标准模型。Nobel 1963。*
>
> **难度**:Advanced
> **前置知识**:[动作电位](Action_Potential.md)、[神经元](Neuron.md)、微分方程
> **后续阅读**:[离子 channel](Ion_Channels.md)

---

## 1. 方程总览

膜电压 V 演化:

$$C \frac{dV}{dt} = I_{\text{ext}} - g_{Na} m^3 h (V - E_{Na}) - g_K n^4 (V - E_K) - g_L (V - E_L)$$

3 个 gating variables 各满足:

$$\frac{dm}{dt} = \alpha_m(V)(1-m) - \beta_m(V) m$$
$$\frac{dh}{dt} = \alpha_h(V)(1-h) - \beta_h(V) h$$
$$\frac{dn}{dt} = \alpha_n(V)(1-n) - \beta_n(V) n$$

---

## 2. 物理意义

### 2.1 通道导电公式

$$g_X = g_X^{\max} \cdot p_{\text{open}}$$

- $g_{Na} m^3 h$: Na+ channel 开关概率
  - $m^3$: 3 个 activation gate (都开,channel 活)
  - $h$: 1 个 inactivation gate
- $g_K n^4$: K+ channel — 4 个 n gate

### 2.2 平衡电位 (Nernst)

$$E_X = \frac{RT}{zF} \ln \frac{[X]_o}{[X]_i}$$

乌贼参数:$E_{Na} = +50$ mV, $E_K = -77$ mV, $E_L = -54.4$ mV。

### 2.3 Gating rates

实验拟合的函数:

$$\alpha_m(V) = \frac{0.1(V+40)}{1 - e^{-(V+40)/10}}$$
$$\beta_m(V) = 4 e^{-(V+65)/18}$$

(类似 for $h, n$)

---

## 3. 求解动态

### 3.1 时间常数 + 稳态

$$\tau_X = \frac{1}{\alpha_X + \beta_X}, \quad X_\infty = \frac{\alpha_X}{\alpha_X + \beta_X}$$

$\frac{dX}{dt} = \frac{X_\infty - X}{\tau_X}$

- $\tau_m$ 极快 (< 1 ms)
- $\tau_h, \tau_n$ 慢 (~ 几 ms)

### 3.2 Spike 过程

1. 输入电流 → V 上升
2. $V \uparrow \to m_\infty(V) \uparrow$ 快 → Na+ 内流
3. 进一步 $V \uparrow$ → 正反馈
4. 慢: $h_\infty(V) \downarrow$, Na+ inactivation
5. 慢: $n_\infty(V) \uparrow$, K+ 外流
6. V 下降 → 复极化

---

## 4. PyTorch 数值积分

```python
import torch

def hh_dynamics(V, m, h, n, I_ext, dt=0.01):
    # Gating rates
    am = 0.1*(V+40) / (1 - torch.exp(-(V+40)/10))
    bm = 4*torch.exp(-(V+65)/18)
    ah = 0.07*torch.exp(-(V+65)/20)
    bh = 1/(1 + torch.exp(-(V+35)/10))
    an = 0.01*(V+55) / (1 - torch.exp(-(V+55)/10))
    bn = 0.125*torch.exp(-(V+65)/80)
    
    # Currents (mS/cm², mV)
    I_Na = 120 * m**3 * h * (V - 50)
    I_K = 36 * n**4 * (V - (-77))
    I_L = 0.3 * (V - (-54.4))
    
    # Update
    dV = (I_ext - I_Na - I_K - I_L) / 1.0  # C=1
    V_new = V + dt * dV
    m_new = m + dt * (am*(1-m) - bm*m)
    h_new = h + dt * (ah*(1-h) - bh*h)
    n_new = n + dt * (an*(1-n) - bn*n)
    return V_new, m_new, h_new, n_new

# Simulate single spike
V, m, h, n = -65.0, 0.05, 0.6, 0.32
trace = []
for t in range(20000):  # 200 ms
    I = 10 if 5000 < t < 6000 else 0
    V, m, h, n = hh_dynamics(V, m, h, n, I)
    trace.append(V.item() if torch.is_tensor(V) else V)
```

---

## 5. 简化模型

### 5.1 FitzHugh-Nagumo (2D)

把 HH 4D 简化为 2D phase plane,分析极限环。

### 5.2 Hindmarsh-Rose

3D,支持 bursting。

### 5.3 Leaky Integrate-and-Fire (LIF)

最简,不带 ion mechanism — 但失去 spike shape。

---

## 6. 应用

- **计算神经科学**: 准确 single-neuron simulation
- **大脑仿真**: Blue Brain / Human Brain Project 用 HH 变种
- **药理**: drug 影响特定 channel,用 HH 模拟效果
- **神经病理**: epilepsy / arrhythmia 涉及 channel mutation

---

## 7. 局限

- 仅 Na/K/leak; real neuron 有 Ca, M-type K, HCN 等多种 channel
- 单 compartment (assume axon 同质);real neuron 有 dendrite + soma
- 计算慢 (复杂网络仿真用 LIF)

---

## 8. 历史

- **1939** — Hodgkin & Huxley 乌贼 axon 电压钳
- **1949** — 推断 Na+/K+ mechanism
- **1952** — 完整方程 + 论文
- **1963** — Nobel Prize
- **1970s+** — 扩展到其他 channel (Cole, Hille)

---

## 9. Common Pitfalls

### 9.1 dt 选

dt 太大 → 数值发散。HH 用 dt ≤ 0.01 ms。

### 9.2 初始 m, h, n 

应取 steady-state 值 (else transient 误差)。

### 9.3 工作单位

mV, mS/cm², μF/cm² — 混用导致 bug。

### 9.4 Real-time 仿真贵

万级 neuron HH 需 GPU 才实时。

### 9.5 不能解释复杂行为

HH 只描述 axon spike;synaptic / network 动力学需扩展。

---

## 10. Related Concepts

- **同节**:[动作电位](Action_Potential.md)、[神经元](Neuron.md)
- **AI 对比**:https://jeffliulab.github.io/ai-notes/01_AI/03_Frontiers/03_World_Models/RSSM_PlaNet/ (类比 RSSM 动力学 ODE)

---

## References

1. **Hodgkin, A. L. & Huxley, A. F.** *J Physiol*, 1952.
2. **Koch, C.** *Biophysics of Computation*. Oxford, 1999.
3. **Trappenberg, T.** *Fundamentals of Computational Neuroscience*. 2010.
4. **Izhikevich, E. M.** *Dynamical Systems in Neuroscience*. MIT, 2007.
