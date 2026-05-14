# Spiking Neural Network (SNN) — 神经形态计算

> *SNN 是更生物 plausible 的 ANN — 用 discrete spikes 而非连续 activation 通信。代表 LIF, Izhikevich, Hodgkin-Huxley 等 neuron models + STDP 学习。被认为是低功耗 + 时间序列任务的潜在方向 (Loihi, SpiNNaker)。*
>
> **难度**:Advanced
> **前置知识**:[神经元](../02_Cellular_Molecular/Neuron.md)、[动作电位](../02_Cellular_Molecular/Action_Potential.md)

---

## 1. SNN vs ANN

| 维度 | ANN (CNN/Transformer) | SNN |
|---|---|---|
| 单元输出 | 连续 (float) | discrete (spike: 0/1) |
| 时间 | 同步 forward pass | event-driven, 时间敏感 |
| 学习 | backprop | STDP, surrogate gradients |
| 能耗 | 高 | 极低 (event-driven) |
| 硬件 | GPU/TPU | Loihi, SpiNNaker, NorthPole |

---

## 2. Neuron Models

### 2.1 LIF (Leaky Integrate-and-Fire)

最简:

$$\tau \frac{dV}{dt} = -(V - V_{\text{rest}}) + R I$$

- V 达阈值 → spike + reset
- 不带 ion mechanism,但保留 spike timing

### 2.2 Izhikevich

2D 模型,丰富 firing patterns (regular, fast spiking, bursting):

$$\dot{v} = 0.04v^2 + 5v + 140 - u + I$$
$$\dot{u} = a(bv - u)$$

### 2.3 Hodgkin-Huxley

4D,最 accurate biological。

LIF 计算便宜,工程 SNN 标准。

---

## 3. STDP 学习

```python
# Pre 先于 post → LTP (weight ↑)
# Post 先于 pre → LTD (weight ↓)
if pre_spike at t_pre and post_spike at t_post:
    dt = t_post - t_pre
    if dt > 0:
        w += A_+ * exp(-dt / tau_+)
    else:
        w -= A_- * exp(dt / tau_-)
```

→ Hebbian, 局部 rule, 生物 plausible。

---

## 4. Surrogate Gradient (训 SNN)

Spike 不可微 → surrogate:

$$\frac{\partial \text{spike}}{\partial V} \approx \frac{1}{\pi} \cdot \frac{1}{1 + (\pi V)^2}$$

或 ATan, Sigmoid 等 smooth approximation。让 backprop 在 SNN 上可行。

---

## 5. PyTorch — LIF SNN

```python
import torch
import torch.nn as nn

class LIFLayer(nn.Module):
    def __init__(self, in_features, out_features, tau=20):
        super().__init__()
        self.fc = nn.Linear(in_features, out_features)
        self.V = None
        self.tau = tau
        self.V_th = 1.0
    
    def forward(self, x, dt=1.0):
        if self.V is None or self.V.size(0) != x.size(0):
            self.V = torch.zeros_like(self.fc(x))
        # Integrate
        I = self.fc(x)
        self.V = self.V + (I - self.V) / self.tau * dt
        # Spike
        spike = (self.V > self.V_th).float()
        # Reset
        self.V = self.V * (1 - spike)
        return spike

# Multi-layer SNN
class SNN(nn.Module):
    def __init__(self):
        super().__init__()
        self.l1 = LIFLayer(784, 128)
        self.l2 = LIFLayer(128, 10)
    
    def forward(self, x, T=10):
        # 输入 firing rate encoding
        spike_counts = torch.zeros(x.size(0), 10)
        for t in range(T):
            input_spike = torch.rand_like(x) < x  # rate encode
            h1 = self.l1(input_spike.float())
            h2 = self.l2(h1)
            spike_counts += h2
        return spike_counts / T
```

---

## 6. 神经形态硬件

- **Intel Loihi 2** (2021): 130k cores, 1B synapses, 极低功耗
- **IBM TrueNorth** (2014): 1M neurons, 256M synapses
- **SpiNNaker** (Manchester): 大规模 parallel SNN sim
- **NorthPole** (IBM 2023): 22B transistors, near-memory compute

---

## 7. 应用 (SNN 优势)

- **Event-based vision**: DVS camera + SNN
- **Edge AI**: 低功耗 (wearable, IoT)
- **Online learning**: STDP 实时 adapt
- **Time series**: 自然处理 timing
- **Robotics**: 实时 sensor processing

---

## 8. 不足

- 准确率仍低于 SOTA ANN
- 训练慢 (surrogate gradient 效率差)
- 缺少 Transformer-scale SNN
- Tooling 不成熟

---

## 9. Common Pitfalls

### 9.1 SNN 不一定更 efficient

理论低功耗;实际依赖硬件支持。

### 9.2 spike count ≠ firing rate

需 careful encoding 输入 → spikes。

### 9.3 STDP 局限

不能直接做 backprop;需 reward modulation 等技巧 for credit assignment。

### 9.4 LIF 缺生物 detail

Real neurons 复杂多;LIF 是 toy。

### 9.5 Brain ≠ SNN

实际 brain 比 SNN 复杂得多。

---

## 10. Related Concepts

- **细胞**:[动作电位](../02_Cellular_Molecular/Action_Potential.md)、[LTP/LTD](../02_Cellular_Molecular/LTP_LTD.md)
- **AI**: SNN 与 ANN 对比 — https://jeffliulab.github.io/ai-notes/02_Deep_Learning/

---

## References

1. **Maass, W.** "Networks of spiking neurons: the third generation of NN models." *Neural Netw*, 1997.
2. **Izhikevich, E. M.** "Simple model of spiking neurons." *IEEE Trans Neural Netw*, 2003.
3. **Davies, M. et al.** "Loihi: A Neuromorphic Manycore Processor." *IEEE Micro*, 2018.
4. **Neftci, E. O. et al.** "Surrogate Gradient Learning in Spiking Neural Networks." *IEEE SPM*, 2019.
