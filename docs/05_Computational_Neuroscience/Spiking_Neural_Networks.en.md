# Spiking Neural Network (SNN) — Neuromorphic Computing

> *SNN is a more biologically plausible ANN — using discrete spikes instead of continuous activations for communication. Represented by LIF, Izhikevich, Hodgkin-Huxley neuron models + STDP learning. Considered a potential direction for low-power + time-series tasks (Loihi, SpiNNaker).*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Neuron](../02_Cellular_Molecular/Neuron.en.md), [Action Potential](../02_Cellular_Molecular/Action_Potential.en.md)

---

## 1. SNN vs ANN

| Aspect | ANN (CNN/Transformer) | SNN |
|---|---|---|
| Unit output | continuous (float) | discrete (spike: 0/1) |
| Time | synchronous forward pass | event-driven, time-sensitive |
| Learning | backprop | STDP, surrogate gradients |
| Energy | high | very low (event-driven) |
| Hardware | GPU/TPU | Loihi, SpiNNaker, NorthPole |

---

## 2. Neuron Models

### 2.1 LIF (Leaky Integrate-and-Fire)

Simplest:

$$\tau \frac{dV}{dt} = -(V - V_{\text{rest}}) + R I$$

- V reaches threshold → spike + reset
- No ion mechanisms but preserves spike timing

### 2.2 Izhikevich

2D model, rich firing patterns:

$$\dot{v} = 0.04v^2 + 5v + 140 - u + I$$
$$\dot{u} = a(bv - u)$$

### 2.3 Hodgkin-Huxley

4D, most biologically accurate.

LIF computationally cheap, standard for engineering SNN.

---

## 3. STDP Learning

```python
# Pre before post → LTP (weight ↑)
# Post before pre → LTD (weight ↓)
if pre_spike at t_pre and post_spike at t_post:
    dt = t_post - t_pre
    if dt > 0:
        w += A_+ * exp(-dt / tau_+)
    else:
        w -= A_- * exp(dt / tau_-)
```

→ Hebbian, local rule, biologically plausible.

---

## 4. Surrogate Gradient (Training SNN)

Spike not differentiable → surrogate:

$$\frac{\partial \text{spike}}{\partial V} \approx \frac{1}{\pi} \cdot \frac{1}{1 + (\pi V)^2}$$

Or ATan, Sigmoid smooth approximation. Makes backprop feasible on SNN.

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
        I = self.fc(x)
        self.V = self.V + (I - self.V) / self.tau * dt
        spike = (self.V > self.V_th).float()
        self.V = self.V * (1 - spike)
        return spike

class SNN(nn.Module):
    def __init__(self):
        super().__init__()
        self.l1 = LIFLayer(784, 128)
        self.l2 = LIFLayer(128, 10)
    
    def forward(self, x, T=10):
        spike_counts = torch.zeros(x.size(0), 10)
        for t in range(T):
            input_spike = torch.rand_like(x) < x
            h1 = self.l1(input_spike.float())
            h2 = self.l2(h1)
            spike_counts += h2
        return spike_counts / T
```

---

## 6. Neuromorphic Hardware

- **Intel Loihi 2** (2021): 130k cores, 1B synapses, ultra-low power
- **IBM TrueNorth** (2014): 1M neurons, 256M synapses
- **SpiNNaker** (Manchester): large-scale parallel SNN sim
- **NorthPole** (IBM 2023): 22B transistors, near-memory compute

---

## 7. Applications (SNN Advantages)

- **Event-based vision**: DVS camera + SNN
- **Edge AI**: low power (wearable, IoT)
- **Online learning**: STDP real-time adapt
- **Time series**: naturally handles timing
- **Robotics**: real-time sensor processing

---

## 8. Limitations

- Accuracy still below SOTA ANN
- Slow training (surrogate gradient inefficient)
- Lack Transformer-scale SNN
- Tooling immature

---

## 9. Common Pitfalls

### 9.1 SNN Not Necessarily More Efficient

Theoretically low power; depends on hardware support.

### 9.2 Spike Count ≠ Firing Rate

Careful encoding input → spikes needed.

### 9.3 STDP Limits

Can't directly backprop; need reward modulation tricks for credit assignment.

### 9.4 LIF Lacks Biological Detail

Real neurons more complex; LIF is toy.

### 9.5 Brain ≠ SNN

Real brain far more complex than SNN.

---

## 10. Related Concepts

- **Same section**: [Predictive Coding](Predictive_Coding.en.md)
- **Cellular**: [Action Potential](../02_Cellular_Molecular/Action_Potential.en.md), [LTP/LTD](../02_Cellular_Molecular/LTP_LTD.en.md)
- **AI**: SNN vs ANN — https://jeffliulab.github.io/ai-notes/02_Deep_Learning/

---

## References

1. **Maass, W.** "Networks of spiking neurons: the third generation of NN models." *Neural Netw*, 1997.
2. **Izhikevich, E. M.** "Simple model of spiking neurons." *IEEE Trans Neural Netw*, 2003.
3. **Davies, M. et al.** "Loihi: A Neuromorphic Manycore Processor." *IEEE Micro*, 2018.
4. **Neftci, E. O. et al.** "Surrogate Gradient Learning in Spiking Neural Networks." *IEEE SPM*, 2019.
