# Reservoir Computing

> *Reservoir computing (Echo State Network, Jaeger 2001; Liquid State Machine, Maass 2002) uses a random fixed recurrent network as a "reservoir," training only the readout layer. High-dimensional nonlinear projection + simple linear readout. Biological correspondence: cerebellum, cortical microcircuit may act as reservoirs. An elegant bypass of RNN training difficulties.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Attractor Networks](Attractor_Networks.en.md), RNN basics

---

## 1. Core Idea

```
input → [random fixed recurrent reservoir] → train only readout (linear)
```

- Reservoir: large, random, fixed, recurrent → high-dim dynamic state
- Train only readout (linear regression) → very fast, stable training
- Avoids BPTT (backprop through time) difficulties

---

## 2. Two Schools

| | Echo State Network (Jaeger) | Liquid State Machine (Maass) |
|---|---|---|
| Unit | rate neuron | spiking neuron |
| Origin | machine learning | computational neuroscience |
| Time | discrete | continuous spike |

---

## 3. Echo State Property

- Reservoir state must asymptotically depend only on input history (forgets initial condition)
- Ensured via spectral radius $\rho(W) < 1$ (approx)
- Too small → no memory; too large → chaos (edge of chaos optimal)

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

- Best computational power at ordered ↔ chaotic criticality
- Balance point of memory + nonlinear separation
- Bertschinger & Natschläger 2004
- Biology may tune to criticality (criticality hypothesis)

---

## 6. Biological Correspondence

- **Cerebellum**: granule cell layer = high-dim random expansion (Marr-Albus + reservoir view)
- **Cortical microcircuit**: Maass proposed cortex is a liquid computer
- **Prefrontal**: mixed selectivity = reservoir-like high-dim projection (see [Neural Population Dynamics](Neural_Population_Dynamics.en.md))

---

## 7. FORCE Learning (Sussillo & Abbott 2009)

- Train chaotic RNN: real-time recursive least squares adjusting readout + feedback
- Makes chaotic reservoir produce complex target outputs
- Biological plasticity resembles reservoir + strong feedback learning
- Bridges reservoir and trainable RNN

---

## 8. Pros and Cons

| Pro | Con |
|---|---|
| Very fast training (only linear) | Reservoir not optimized → must be large |
| Stable (no BPTT) | Hyperparameter sensitive (ρ, scaling) |
| Strong on time series | Doesn't scale like end-to-end RNN |
| Hardware-friendly (physical reservoir) | Task-specific tuning |

---

## 9. Physical / Neuromorphic Reservoir

- Any high-dim nonlinear dynamical system can be a reservoir:
  - Photonic, memristor, spintronic, mechanical, even a bucket of water
- Suits edge / low-power temporal processing
- Synergizes with neuromorphic computing

---

## 10. Common Pitfalls

### 10.1 Reservoir Needs Training

The core is **not** training the reservoir; only the readout.

### 10.2 Bigger Always Better

Must balance + echo state property; too large / ρ>1 → chaos fails.

### 10.3 = Ordinary RNN

RNN fully trained; reservoir fixed recurrent.

### 10.4 Edge of Chaos Mysteriously Optimal

Empirical + theoretical tendency; not strictly optimal for all tasks.

### 10.5 Biology Is Just a Reservoir

A hypothesis / analogy; cortex also has learned recurrent weights.

---

## 11. Related Concepts

- **Same section**: [Attractor Networks](Attractor_Networks.en.md), [Neural Population Dynamics](Neural_Population_Dynamics.en.md), [Spiking Neural Networks](Spiking_Neural_Networks.en.md)
- **Anatomy**: [Cerebellum](../01_Neuroanatomy/Cerebellum.en.md)
- **AI**: RNN, neuromorphic, physical computing

---

## References

1. **Jaeger, H.** "The 'echo state' approach to analysing and training recurrent neural networks." *GMD Report*, 2001.
2. **Maass, W., Natschläger, T., Markram, H.** "Real-time computing without stable states (Liquid State Machine)." *Neural Comput*, 2002.
3. **Sussillo, D. & Abbott, L. F.** "Generating coherent patterns of activity from chaotic neural networks (FORCE)." *Neuron*, 2009.
4. **Lukoševičius, M. & Jaeger, H.** "Reservoir computing approaches to recurrent neural network training." *Comput Sci Rev*, 2009.
