# Energy-Based Models & the Brain

> *Energy-based models (EBM) define a state distribution via an energy landscape: low energy = high probability. Hopfield → Boltzmann machine → modern EBM/diffusion. The 2024 Nobel Prize in Physics to Hopfield + Hinton is based on this. Biological correspondence: attractor dynamics is energy descent. Connects [Hopfield Networks](Hopfield_Networks.en.md), [Free Energy Principle](Free_Energy_Principle.en.md) with generative AI.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Hopfield Networks](Hopfield_Networks.en.md), [Free Energy Principle](Free_Energy_Principle.en.md), statistical physics basics

---

## 1. Core Idea

$$p(x) = \frac{1}{Z} e^{-E(x)/T}$$

- $E(x)$: energy function (low = preferred state)
- $Z$: partition function (intractable)
- $T$: temperature
- Learning = shape energy landscape so data sits at low energy

---

## 2. Lineage

| Model | Feature |
|---|---|
| **Hopfield** (1982) | deterministic, associative memory |
| **Boltzmann machine** (1985) | + stochastic + hidden units |
| **RBM** | bipartite, contrastive divergence training |
| **Deep Boltzmann / DBN** | deep (2006 DL revival) |
| **Modern EBM** | score matching, Langevin |
| **Diffusion models** | continuous score view of EBM |

---

## 3. Boltzmann Machine

- Stochastic binary units + symmetric weights
- Equilibrium distribution = Boltzmann distribution
- Hidden units → learn complex distributions
- Hinton & Sejnowski 1985
- Slow training (needs sampling) → RBM + CD accelerates

---

## 4. Biological Correspondence

- **Attractor dynamics** = energy descent to minima (see [Attractor Networks](Attractor_Networks.en.md))
- **Hopfield memory** = energy valley = stored pattern
- **Free energy principle**: minimizing variational free energy (see [Free Energy Principle](Free_Energy_Principle.en.md))
- Synapses = shaping the energy landscape

---

## 5. PyTorch — RBM (Contrastive Divergence)

```python
import torch

class RBM:
    def __init__(self, n_vis, n_hid):
        self.W = torch.randn(n_vis, n_hid) * 0.01
        self.b_v = torch.zeros(n_vis)
        self.b_h = torch.zeros(n_hid)
    def sample_h(self, v):
        p = torch.sigmoid(v @ self.W + self.b_h)
        return p, (torch.rand_like(p) < p).float()
    def sample_v(self, h):
        p = torch.sigmoid(h @ self.W.t() + self.b_v)
        return p, (torch.rand_like(p) < p).float()
    def cd1(self, v0, lr=0.01):
        ph0, h0 = self.sample_h(v0)
        _, v1 = self.sample_v(h0)
        ph1, _ = self.sample_h(v1)
        self.W += lr * (v0.t() @ ph0 - v1.t() @ ph1) / v0.size(0)
```

---

## 6. 2024 Nobel Physics

- Hopfield + Hinton awarded
- Hopfield network (energy-based associative memory)
- Boltzmann machine (stochastic EBM, DL pioneer)
- Recognizing statistical-physics × neural networks foundational contributions

---

## 7. Energy ↔ Inference

- Inference = energy minimization (MAP) or sampling (posterior)
- Langevin dynamics: $dx = -\nabla E(x) dt + \sqrt{2T}\,dW$
- Analogy: neural dynamics descending energy → inference
- Connects to predictive coding (prediction error = energy gradient)

---

## 8. Relation to Diffusion / Modern Generative

- Diffusion model = learn score $\nabla \log p = -\nabla E$
- EBM revival (score matching bypasses Z)
- Brain generative model hypothesis: use energy landscape to generate predictions (see [Free Energy Principle](Free_Energy_Principle.en.md))

---

## 9. Difficulty — Partition Function

- $Z = \sum_x e^{-E(x)}$ usually intractable
- Solutions: MCMC, contrastive divergence, score matching, NCE
- How does biology "compute Z"? → may not need (only relative energy / local gradient)

---

## 10. Common Pitfalls

### 10.1 EBM = Hopfield

Hopfield is the simplest EBM; also stochastic, deep, score-based.

### 10.2 Energy Has Physical Meaning

It's an abstract scalar (borrowed statistical physics form); not real energy.

### 10.3 Brain Computes Partition Function

May use only local gradients / relative energy, no global Z needed.

### 10.4 Boltzmann Machine Practical

Original training extremely slow; RBM/CD + modern score matching made it practical.

### 10.5 Energy Minima = Only Computation

Also transient / non-equilibrium computation; EBM is one view.

---

## 11. Related Concepts

- **Same section**: [Hopfield Networks](Hopfield_Networks.en.md), [Attractor Networks](Attractor_Networks.en.md), [Free Energy Principle](Free_Energy_Principle.en.md), [Predictive Coding](Predictive_Coding.en.md)
- **Cellular**: [LTP/LTD](../02_Cellular_Molecular/LTP_LTD.en.md)
- **AI**: diffusion models, score matching, generative models

---

## References

1. **Hopfield, J. J.** "Neural networks and physical systems with emergent collective computational abilities." *PNAS*, 1982.
2. **Ackley, D. H., Hinton, G. E., Sejnowski, T. J.** "A learning algorithm for Boltzmann machines." *Cogn Sci*, 1985.
3. **LeCun, Y. et al.** "A tutorial on energy-based learning." *Predicting Structured Data*, 2006.
4. **Song, Y. & Ermon, S.** "Generative modeling by estimating gradients of the data distribution." *NeurIPS*, 2019.
