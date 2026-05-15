# Neural Coding

> *Neural coding studies how neurons represent information with spikes. Rate coding (Adrian 1926) vs temporal coding vs population coding vs sparse coding. Understanding coding bridges single-neuron and behavior, and is core to brain-AI comparison. Decoding (reconstructing stimulus from spikes) is the basis of BCI.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Action Potential](../02_Cellular_Molecular/Action_Potential.en.md), probability basics

---

## 1. Coding Schemes

| Scheme | Information carrier |
|---|---|
| **Rate coding** | spike frequency |
| **Temporal coding** | precise spike timing |
| **Population coding** | population activity pattern |
| **Sparse coding** | few neurons active |
| **Phase coding** | phase relative to oscillation |

---

## 2. Rate Coding (Adrian 1926)

- Information = firing rate (spikes / sec)
- Strong stimulus → high rate
- Simple, robust
- But slow (needs integration window ~ 100 ms)

$$r = \frac{n_{\text{spikes}}}{\Delta T}$$

---

## 3. Temporal Coding

- Precise spike timing carries information
- E.g.: auditory phase locking (< 1 ms precision)
- Fast (single spike = information)
- Spike-timing-dependent plasticity (STDP) exploits this

---

## 4. Population Coding

- Single neuron noisy → population average
- **Population vector** (Georgopoulos 1986): M1 movement direction
$$\vec{P} = \sum_i r_i \vec{c}_i$$
- Redundant + robust + fast

---

## 5. Sparse Coding

- At any moment only few neurons active
- Energy efficient + high capacity
- Olshausen & Field 1996: sparse coding naturally emerges V1 Gabor-like filters
- Linked to autoencoder sparsity

---

## 6. Tuning Curve

- Neuron response vs stimulus parameter
- E.g.: V1 orientation tuning (bell-shaped)
- $r(\theta) = r_{\max} \exp\left(-\frac{(\theta - \theta_{pref})^2}{2\sigma^2}\right)$

---

## 7. PyTorch — Population Decoding

```python
import torch

def population_decode(rates, preferred_dirs):
    """Population vector decoding (Georgopoulos)."""
    pop_vector = (rates.unsqueeze(1) * preferred_dirs).sum(dim=0)
    decoded_angle = torch.atan2(pop_vector[1], pop_vector[0])
    return decoded_angle

N = 100
preferred = torch.randn(N, 2)
preferred = preferred / preferred.norm(dim=1, keepdim=True)
true_dir = torch.tensor([1.0, 0.0])
rates = torch.relu((preferred @ true_dir)) * 10  # cosine tuning
print(population_decode(rates, preferred))  # ≈ 0 rad
```

---

## 8. Information Theory

- **Mutual information**: $I(S; R) = H(R) - H(R|S)$
- Measures how much stimulus info a spike train carries
- **Fisher information**: lower bound on coding precision (Cramér-Rao)

---

## 9. Noise + Variability

- Spike count often Poisson-like (variance ≈ mean)
- **Fano factor** = var/mean ≈ 1
- Noise correlation affects population coding efficiency
- Trial-to-trial variability is active research

---

## 10. AI Correspondence

| Brain | AI |
|---|---|
| Rate coding | ANN continuous activation |
| Temporal coding | SNN spike timing |
| Population coding | distributed representation |
| Sparse coding | sparse autoencoder, L1 |
| Tuning curve | receptive field / feature detector |

---

## 11. Common Pitfalls

### 11.1 Rate vs Temporal Either-Or

Brain actually mixes them, task-dependent.

### 11.2 Single Neuron = Single Concept ("grandmother cell")

Limited evidence (Quiroga concept cells), but mostly distributed.

### 11.3 High Firing = Important

In sparse coding, silence is also information.

### 11.4 Poisson Assumption Universal

Many neurons sub/super-Poisson; assumption needs verification.

### 11.5 Decoding = What Brain Does

Decoder is an observer's tool; doesn't mean downstream neurons use the same method.

---

## 12. Related Concepts

- **Same section**: [Levels of Analysis](Levels_of_Analysis.en.md), [Research Methods](Research_Methods.en.md)
- **Basis**: [Action Potential](../02_Cellular_Molecular/Action_Potential.en.md)
- **Computational**: [SNN](../05_Computational_Neuroscience/Spiking_Neural_Networks.en.md), [Grid Cells](../05_Computational_Neuroscience/Grid_Cells.en.md)
- **Systems**: [Motor System](../03_Systems_Neuroscience/Motor_System.en.md)

---

## References

1. **Adrian, E. D.** "The impulses produced by sensory nerve endings." *J Physiol*, 1926.
2. **Georgopoulos, A. P. et al.** "Neuronal population coding of movement direction." *Science*, 1986.
3. **Olshausen, B. A. & Field, D. J.** "Emergence of simple-cell receptive field properties by learning a sparse code." *Nature*, 1996.
4. **Dayan, P. & Abbott, L. F.** *Theoretical Neuroscience*. MIT Press, 2001.
