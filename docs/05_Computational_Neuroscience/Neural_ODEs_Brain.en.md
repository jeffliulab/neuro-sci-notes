# Neural ODEs & Continuous-Time Models

> *The brain is a continuous-time dynamical system, not discrete layers. Neural ODE (Chen 2018) views ResNet as ODE discretization, modeling with continuous dynamics. Naturally fits neuroscience rate models, conductance models. Connects the dynamical systems view of [Neural Population Dynamics](Neural_Population_Dynamics.en.md) with modern differentiable ODE solvers.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Neural Population Dynamics](Neural_Population_Dynamics.en.md), ODEs, automatic differentiation

---

## 1. Motivation

- Biological neurons evolve in continuous time (membrane voltage ODE)
- Discrete RNN/ResNet = Euler discretization of ODE
- Use continuous dynamics directly: fits irregular sampling, variable depth

---

## 2. Neural ODE (Chen 2018)

ResNet: $h_{t+1} = h_t + f(h_t, \theta)$
→ limit:
$$\frac{dh(t)}{dt} = f(h(t), t, \theta)$$

- ODE solver (RK4, Dopri5) for forward
- **Adjoint method**: O(1) memory backprop (solve adjoint ODE)
- "Infinitely deep" network

---

## 3. Isomorphic to Neuroscience Models

| Neuroscience | Neural ODE |
|---|---|
| Rate model $\tau\dot r = -r + f(Wr)$ | RNN ODE |
| Conductance (HH) | stiff ODE |
| Neural mass | low-dim ODE |
| Population dynamics | latent ODE |

→ Computational neuroscience has always used ODEs; Neural ODE gives a differentiable + learnable framework.

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

## 5. Latent ODE — Irregular Sampling

- Neural/behavioral data often irregularly sampled
- Latent ODE (Rubanova 2019): encoder → latent ODE → decoder
- Suits spike trains, clinical time series
- Better than discrete RNN for missing / irregular data

---

## 6. Stiff ODE Problem

- HH-type conductance models are stiff (fast Na + slow channels)
- Need implicit solver (implicit RK, BDF)
- Explicit solver step extremely small → slow
- See [Compartmental Models](Compartmental_Models.en.md)

---

## 7. Relation to RNN / Continuous RNN

- Continuous-time RNN (CTRNN) predates Neural ODE (common in neuroscience)
- Neural ODE = CTRNN + modern ODE solver + adjoint
- "Liquid Time-Constant Networks" (Hasani 2021): adaptive τ, bio-inspired
- ODE-RNN hybrid handles discrete observations

---

## 8. Pros and Cons

| Pro | Con |
|---|---|
| Continuous time (irregular data) | Slow training (solver calls) |
| O(1) memory (adjoint) | Adjoint numerical instability risk |
| Aligned with dynamical systems | Stiff hard |
| Adaptive computation | Harder to tune than discrete |

---

## 9. Neuroscience Applications

- Fit neural population trajectories (latent dynamics)
- LFADS-like: infer latent ODE explaining spikes
- Continuous control / motor models
- Disease dynamics (continuous-time disease progression)

---

## 10. Common Pitfalls

### 10.1 Neural ODE Brand New

Continuous-time neural models existed in computational neuroscience for decades (CTRNN).

### 10.2 Any ODE Solver

Stiff (HH) must use implicit; else diverges or extremely slow.

### 10.3 Adjoint Always Saves Memory

Numerical error can accumulate; sometimes backprop-through-solver more stable.

### 10.4 Continuous = More Biological

Form is closer, but still abstract; not automatically mechanistically correct.

### 10.5 Always Better Than RNN

Many tasks discrete RNN faster + more accurate; ODE suits irregular / continuous scenarios.

---

## 11. Related Concepts

- **Same section**: [Neural Population Dynamics](Neural_Population_Dynamics.en.md), [Attractor Networks](Attractor_Networks.en.md), [Compartmental Models](Compartmental_Models.en.md)
- **Cellular**: [Hodgkin Huxley](../02_Cellular_Molecular/Hodgkin_Huxley.en.md)
- **AI**: ResNet, ODE solver, differentiable programming

---

## References

1. **Chen, R. T. Q. et al.** "Neural ordinary differential equations." *NeurIPS*, 2018.
2. **Rubanova, Y. et al.** "Latent ODEs for irregularly-sampled time series." *NeurIPS*, 2019.
3. **Hasani, R. et al.** "Liquid time-constant networks." *AAAI*, 2021.
4. **Pandarinath, C. et al.** "Inferring single-trial neural population dynamics using sequential auto-encoders (LFADS)." *Nat Methods*, 2018.
