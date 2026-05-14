# Predictive Coding

> *Predictive Coding (Rao & Ballard 1999): cortex doesn't passively receive input — it **actively predicts** input → only **error** propagates up. Friston later unified this with free energy. A core framework at the intersection of brain computation + AI.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Hodgkin-Huxley](../02_Cellular_Molecular/Hodgkin_Huxley.en.md), [Cortex](../01_Neuroanatomy/Cortex.en.md)

---

## 1. Core Idea

```
Upper layer → prediction → Lower layer
        ↑
      error (actual - predicted)
        |
Lower layer → upload error → Upper layer update
```

Hierarchy:
- L1 predicts sensory input
- L2 predicts L1
- ... up to PFC

→ Only **prediction error** propagates up.

---

## 2. Math (Rao & Ballard 1999)

Each layer representation $r_i$, prediction $\hat{r}_{i-1} = f(r_i)$:

$$\dot{r}_i = -\frac{\partial F}{\partial r_i} = (r_{i-1} - \hat{r}_{i-1}) \cdot f'(r_i) - (r_i - g(r_{i+1}))$$

→ minimize free energy = squared prediction error.

---

## 3. Neural Implementation

Cortex 6 layers:
- Layer 2/3: prediction signal
- Layer 5/6: error signal
- Top-down/bottom-up projection + lateral inhibition implements prediction-error loop

---

## 4. vs VAE / Free Energy

Friston FEP:

$$F = D_{KL}(q(s) \| p(s)) - \mathbb{E}_q[\log p(o|s)]$$

minimizing F ⟺ minimizing prediction error.

VAE ELBO mathematically equivalent.

---

## 5. AI Connections

- DeepMind PredNet (2017): Rao-Ballard implementation for video prediction
- LeCun JEPA route: predict latent (don't reconstruct pixels)
- World Models path

---

## 6. Empirical Support

- **MisMatch Negativity (MMN)**: EEG paradigm reflecting prediction error
- **fMRI omitted stimulus**: surprise triggers activity
- **Visual illusions**: explained as prior overriding sensory

---

## 7. PyTorch — Hierarchical Predictive Coding

```python
import torch
import torch.nn as nn

class PredictiveCoderLayer(nn.Module):
    def __init__(self, dim_lower, dim_upper):
        super().__init__()
        self.predict_down = nn.Linear(dim_upper, dim_lower)
        self.update_up = nn.Linear(dim_lower, dim_upper)
        self.r_upper = None
    
    def forward(self, r_lower):
        if self.r_upper is None:
            self.r_upper = torch.zeros(r_lower.size(0), self.update_up.out_features)
        pred_lower = self.predict_down(self.r_upper)
        error = r_lower - pred_lower
        self.r_upper = self.r_upper + 0.1 * self.update_up(error)
        return error, self.r_upper
```

---

## 8. Pathology

- **Schizophrenia**: faulty prediction → hallucinations (prediction overly dominant)
- **Autism**: insufficient prediction → hyper-sensitivity to detail
- **Dyslexia**: auditory prediction abnormal

---

## 9. History

- **1958** — Helmholtz "unconscious inference" inspires
- **1999** — Rao & Ballard predictive coding for V1
- **2005** — Friston Free Energy Principle
- **2010s** — Predictive coding explains many phenomena
- **2020s** — converges with AI World Models

---

## 10. Common Pitfalls

### 10.1 Not Sole Framework

Also sparse coding, efficient coding alternatives.

### 10.2 Limited Empirical Evidence

Specific prediction-error neurons hard to localize.

### 10.3 vs Backprop

Theoretically can compute backprop; implementation differs.

### 10.4 Abstract vs Concrete

Many high-level predictive codings hard to ground in cortex.

### 10.5 Doesn't Explain Hard Problem

Only functional level, doesn't explain subjective experience.

---

## 11. Related Concepts

- **Same section**: [Spiking NN](Spiking_Neural_Networks.en.md)
- **AI**: [WM Predictive Coding](https://jeffliulab.github.io/ai-notes/01_AI/03_Frontiers/03_World_Models/WM_Predictive_Coding/)

---

## References

1. **Rao, R. P. N. & Ballard, D. H.** "Predictive coding in the visual cortex." *Nat Neurosci*, 1999.
2. **Friston, K.** "The free-energy principle." *Nat Rev Neurosci*, 2010.
3. **Lotter, W. et al.** "Deep Predictive Coding Networks (PredNet)." *ICLR*, 2017.
4. **Seth, A.** *Being You*. 2021.
