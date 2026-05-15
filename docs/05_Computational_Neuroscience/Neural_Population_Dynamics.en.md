# Neural Population Dynamics

> *A modern systems neuroscience paradigm shift: from single-neuron tuning to population state space + low-dimensional manifold + dynamics. Churchland 2012: M1 doesn't encode movement parameters but is a dynamical system generating muscle commands. PCA/jPCA/dPCA + dynamical systems are tools. RNN is the theoretical model.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Neural Coding](../00_Foundations/Neural_Coding.en.md), [Attractor Networks](Attractor_Networks.en.md), linear algebra

---

## 1. Paradigm Shift

| Old (single-neuron) | New (population) |
|---|---|
| Each neuron encodes a variable | Population state on low-dim manifold |
| Tuning curve | Neural trajectory |
| Mean firing rate | Dynamics (how it evolves) |
| Reductionist | Emergent collective computation |

---

## 2. State Space

- N neurons → N-dim space, one point per moment
- Population activity = trajectory
- Actual dimensionality ≪ N (low-dimensional manifold)
- Dimensionality reduction: PCA, Factor Analysis, jPCA, dPCA, UMAP

---

## 3. Low-Dimensional Manifold

- M1 hundreds of neurons, but ~ 6-10 dims capture most variance
- "Neural manifold" stable across trials / time
- Learning = within-manifold vs outside (Sadtler 2014: within-manifold easy, outside hard)

---

## 4. Churchland 2012 — Rotational Dynamics

- M1 preparation → execution: population activity shows **rotation** (revealed by jPCA)
- Single neurons look messy, population level is regular
- M1 is not representational (encoding direction), it's dynamical (engine generating patterns)
- "Neural population as a machine, not a code"

---

## 5. Dynamical Systems View

$$\dot{\mathbf{x}} = F(\mathbf{x}, \mathbf{u})$$

- $\mathbf{x}$: low-dim latent state
- Computation = trajectory evolution (not instantaneous mapping)
- Fixed points, line attractor, rotation motifs
- Same framework as [Attractor Networks](Attractor_Networks.en.md)

---

## 6. PyTorch — jPCA Idea (Rotational Component)

```python
import torch

def find_rotational_dynamics(X):
    """X: (T, N) population trajectory. Find skew-symmetric M: dX ≈ X M."""
    dX = X[1:] - X[:-1]
    Xc = X[:-1]
    # Solve least squares dX = Xc M, constrain M skew-symmetric
    M = torch.linalg.lstsq(Xc, dX).solution
    M_skew = 0.5 * (M - M.t())          # rotational part
    eigvals = torch.linalg.eigvals(M_skew)
    return M_skew, eigvals  # imaginary eigvals → rotation freq
```

---

## 7. dPCA — Demixed PCA

- Standard PCA mixes task variables
- dPCA (Kobak 2016): decomposes variance into stimulus / decision / time components
- Explains mixed selectivity (single neuron encodes multiple variables)

---

## 8. Mixed Selectivity

- PFC neurons often nonlinearly mix-encode multiple variables
- Rigotti 2013: high-dimensional mixed selectivity → any combination linearly readable
- Similar to kernel trick / reservoir
- Function only appears at population level

---

## 9. RNN as Model

- Train RNN to solve task → compare to neural data (same dynamics?)
- Mante 2013: context-dependent computation, RNN consistent with PFC dynamics
- Reverse-engineer RNN (fixed points) → understand brain
- "Computation through dynamics" (Vyas 2020 review)

---

## 10. Common Pitfalls

### 10.1 Single-Neuron Tuning Sufficient

Population dynamics reveals structure invisible in single neurons.

### 10.2 Low-Dim = Simple

Low-dim manifold can have complex nonlinear dynamics.

### 10.3 PCA Components Have Biological Meaning

PCA axes are statistical, not necessarily specific mechanisms.

### 10.4 Manifold Fixed

Learning / task can change manifold (though within-manifold easier).

### 10.5 Representation vs Dynamics Either-Or

Complementary views; modern leans dynamics but representation still useful.

---

## 11. Related Concepts

- **Same section**: [Attractor Networks](Attractor_Networks.en.md), [Hopfield Networks](Hopfield_Networks.en.md), [Reinforcement Learning Brain](Reinforcement_Learning_Brain.en.md)
- **Foundation**: [Neural Coding](../00_Foundations/Neural_Coding.en.md), [Levels of Analysis](../00_Foundations/Levels_of_Analysis.en.md)
- **Systems**: [Motor System](../03_Systems_Neuroscience/Motor_System.en.md)
- **AI**: RNN dynamics

---

## References

1. **Churchland, M. M. et al.** "Neural population dynamics during reaching." *Nature*, 2012.
2. **Mante, V. et al.** "Context-dependent computation by recurrent dynamics in prefrontal cortex." *Nature*, 2013.
3. **Rigotti, M. et al.** "The importance of mixed selectivity in complex cognitive tasks." *Nature*, 2013.
4. **Vyas, S. et al.** "Computation through neural population dynamics." *Annu Rev Neurosci*, 2020.
