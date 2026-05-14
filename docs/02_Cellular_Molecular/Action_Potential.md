# 动作电位 (Action Potential) — 神经元的数字信号

> *动作电位是神经元的核心 communication unit:1ms 全有或全无 (all-or-none) 的电压 spike,沿 axon 传播。本篇从离子机制 (Na+/K+) → Hodgkin-Huxley 方程 → 髓鞘传导,完整介绍 spike 的产生与传播。是神经科学的"hello world"。*
>
> **难度**:Intermediate

---

## 1. 动作电位形状

经典波形:
- **静息**: -70 mV
- **阈值**: -55 mV
- **峰值**: +30 mV
- **复极化**
- **超极化**: -80 mV (短暂)
- **回静息**

总时长 ~ 1 ms。

```
+30 |          /\
    |         /  \
 -55|________/____\____________
    |       (threshold)
 -70|_______________\___________
    |                \________
 -80|________________________(undershoot)
```

---

## 2. 离子机制

### 2.1 静息

- K+ leak channel 开 → 接近 K+ 平衡电位 (-90 mV)
- Na+/K+ pump 维持 gradient

### 2.2 Phase 1: 去极化 (Rising)

阈值 (-55 mV) 触发 voltage-gated Na+ channel 开:
- Na+ 大量内流
- 进一步 depolarize → 更多 Na+ channel 开
- **正反馈**,~ 0.5 ms 达 +30 mV

### 2.3 Phase 2: 峰值

- Na+ channel 自动**失活** (inactivation gate 关)
- Voltage-gated K+ channel 开 (慢启动)
- K+ 外流开始 dominant

### 2.4 Phase 3: 复极化

- Na+ channel 仍 inactivated
- K+ 外流主导,电位下降

### 2.5 Phase 4: 超极化

- K+ channel 关闭慢
- 短暂超过 -80 mV (K+ 平衡)
- 此时 absolute / relative refractory period — 不能 / 难再 fire

### 2.6 回静息

- Na+/K+ pump 恢复 gradient
- Na+ inactivation reset (可再 fire)

---

## 3. Hodgkin-Huxley 方程

1952 年 Hodgkin & Huxley 在乌贼 axon 数学化:

$$C \frac{dV}{dt} = -g_{Na} m^3 h (V - E_{Na}) - g_K n^4 (V - E_K) - g_L (V - E_L) + I_{\text{ext}}$$

其中:
- $V$ = membrane potential
- $C$ = 膜电容 (~ 1 μF/cm²)
- $g_{Na}, g_K, g_L$ = 最大 conductance
- $m, h, n$ = gating variables, 各自 ODE
- $E_{Na}, E_K, E_L$ = 平衡电位
- $I_{\text{ext}}$ = 外加电流

### 3.1 Gating 动力学

$$\frac{dm}{dt} = \alpha_m(V)(1-m) - \beta_m(V) m$$

(其中 $\alpha, \beta$ 是 V 的函数)。同形式 for $h, n$。

→ 5 个 ODE 完整描述 spike。Nobel 1963。

---

## 4. 阈值与全有或全无

阈值不是固定 voltage,而是 Na+ channel 开关动力学的 tipping point。
低于阈值 → 不 spike (但有 sub-threshold integration)。
之上 → spike 大小 / 形状几乎一致 (信息编码在 **timing + rate**,不是 amplitude)。

---

## 5. 传播 (Propagation)

### 5.1 无髓 axon (无 myelin)

- Spike 在 axon 上 wave-like 传播
- 速度: 0.5 - 2 m/s
- 慢,适合短距离

### 5.2 有髓 axon

- Myelin 是绝缘层,跳过中间区域
- Spike 在 **Ranvier nodes** (节间) 间跳跃 (saltatory conduction)
- 速度: 10 - 100 m/s
- 神经直径越大越快 (e.g. spinal motor 30 m/s)

### 5.3 失髓鞘疾病 (MS)

- Multiple Sclerosis 自身免疫破坏 myelin
- 信号变慢 / 错误 → 神经症状

---

## 6. PyTorch — 数值积分 HH 方程

```python
import torch

def hh_step(V, m, h, n, I_ext, dt=0.01):
    # Gating rates (in mV, ms units)
    alpha_m = 0.1 * (V + 40) / (1 - torch.exp(-(V + 40) / 10))
    beta_m = 4 * torch.exp(-(V + 65) / 18)
    alpha_h = 0.07 * torch.exp(-(V + 65) / 20)
    beta_h = 1 / (1 + torch.exp(-(V + 35) / 10))
    alpha_n = 0.01 * (V + 55) / (1 - torch.exp(-(V + 55) / 10))
    beta_n = 0.125 * torch.exp(-(V + 65) / 80)
    
    # Update gating
    m = m + dt * (alpha_m * (1 - m) - beta_m * m)
    h = h + dt * (alpha_h * (1 - h) - beta_h * h)
    n = n + dt * (alpha_n * (1 - n) - beta_n * n)
    
    # Currents
    g_Na, g_K, g_L = 120, 36, 0.3
    E_Na, E_K, E_L = 50, -77, -54.4
    I_Na = g_Na * m**3 * h * (V - E_Na)
    I_K = g_K * n**4 * (V - E_K)
    I_L = g_L * (V - E_L)
    
    # Update voltage (C = 1)
    dV = -I_Na - I_K - I_L + I_ext
    V = V + dt * dV
    return V, m, h, n

# Simulate
V, m, h, n = -65, 0.05, 0.6, 0.32
trace = []
for t in range(10000):
    I = 10 if 2000 < t < 5000 else 0  # current pulse
    V, m, h, n = hh_step(V, m, h, n, I)
    trace.append(V)
```

---

## 7. 信息编码

### 7.1 Rate coding

Spike frequency (~ 1-1000 Hz) encode information。
- Sensory neurons: 强 stimulus → 高 rate
- Motor neurons: 高 rate → 强 muscle contract

### 7.2 Temporal coding

Precise spike timing (1 ms 级) 更 informative:
- Auditory: phase locking 用于 sound localization
- Olfactory: spike pattern 编码 odor

### 7.3 Population coding

多个 neuron 协同;e.g. motor cortex 用 population vector 编码运动方向。

---

## 8. 经典实验 — 乌贼 axon

Hodgkin & Huxley 1939 选乌贼:
- 巨型 axon 直径 0.5-1mm,可插玻璃电极
- 测 voltage clamp:固定 V,测 ion current
- 推 ion conductance V-dependence

→ 这是神经科学最优雅实验之一。

---

## 9. Bezier 简化 vs 真实 spike

实际 cortical neuron spike:
- 形状有 variation
- AP 后 afterdepolarization / afterhyperpolarization
- Burst firing (PFC L5b cells)
- Calcium spike (long, 100ms+)

LIF 是过度简化;HH 是好近似;real neuron 更复杂。

---

## 10. Common Pitfalls

### 10.1 阈值不是 fixed

阈值随 stimulus history / channel state 变。

### 10.2 Spike ≠ binary

虽 all-or-none,real spike 有 width / amplitude variation。

### 10.3 一种 channel 不够

Real neuron 有 10+ 种 channels (Ca, Na 多 subtype, leak K, etc.)。

### 10.4 Refractory period 不绝对

强 stimulus 可在 relative refractory 中 fire,但 amplitude 降。

### 10.5 Dendrite 也 spike

Dendritic Na/Ca spikes 影响 integration,不只是 cable propagation。

---

## 11. Related Concepts

- **历史**:[神经科学历史](../00_Foundations/Neuroscience_History.md)

---

## References

1. **Hodgkin, A. L. & Huxley, A. F.** "A quantitative description of membrane current and its application to conduction and excitation in nerve." *J Physiol*, 1952.
2. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
3. **Bear, M. F. et al.** *Neuroscience: Exploring the Brain*. 4th ed., 2015.
4. **Koch, C.** *Biophysics of Computation*. Oxford, 1999.
5. **Trappenberg, T.** *Fundamentals of Computational Neuroscience*. 2nd ed., 2010.
