# Neuroplasticity

> *Neuroplasticity is the brain's lifelong ability to change structure + function. From synaptic (LTP/LTD) to structural (spine, axon sprouting) to systems (cortical remapping) to neurogenesis. It's the basis of learning, memory, recovery. "Neurons that fire together wire together" (Hebb). Also the principle of rehabilitation and BCI training.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [LTP/LTD](../02_Cellular_Molecular/LTP_LTD.en.md), [Neurodevelopment](Neurodevelopment.en.md)

---

## 1. Plasticity Levels

| Level | Example | Timescale |
|---|---|---|
| Synaptic | LTP / LTD | ms-hours |
| Structural | spine gain/loss, axon sprouting | hours-days |
| Intrinsic | ion channel density change | hours-days |
| Systems | cortical map remapping | days-months |
| Neurogenesis | new neurons (hippocampal DG) | weeks-months |

---

## 2. Hebbian Principle

> "Cells that fire together wire together" — Hebb 1949

$$\Delta w_{ij} = \eta \, x_i \, x_j$$

- Co-activation → synapse strengthens
- Pure Hebbian unstable → needs normalization / BCM rule
- STDP is the spike-timing version

---

## 3. Synaptic Plasticity

- **LTP**: long-term potentiation (NMDA → Ca²⁺ → AMPA insertion)
- **LTD**: long-term depression
- **Homeostatic plasticity**: synaptic scaling (keeps stable)
- See [LTP/LTD](../02_Cellular_Molecular/LTP_LTD.en.md)

---

## 4. Structural Plasticity

- **Spine turnover**: learning → new spine formation
- **Axon sprouting**: collateral growth after injury
- **Dendritic remodeling**
- Two-photon in vivo imaging directly observes

---

## 5. Cortical Remapping

- **Amputation**: adjacent cortical areas invade (phantom limb)
- **Blind**: visual cortex recruited for touch/language (cross-modal)
- **Musicians**: finger representation expanded
- **London taxi**: posterior hippocampus enlarged (Maguire 2000)

---

## 6. Critical Period vs Lifelong Plasticity

- Critical period: childhood-specific window strongest
- But adults still have it (reduced but present)
- Reopening mechanisms: environmental enrichment, drugs (fluoxetine), training

---

## 7. PyTorch — Hebbian + Oja Rule

```python
import torch

def oja_update(W, x, lr=0.01):
    """Oja's rule: Hebbian + normalization (stable)."""
    y = W @ x                          # output
    # ΔW = lr * y * (x - y * W)  — prevents unbounded growth
    dW = lr * (torch.outer(y, x) - torch.outer(y, y) @ W)
    return W + dW
```

→ Pure Hebbian diverges; Oja adds normalization, converges to principal component.

---

## 8. Clinical Applications

- **Stroke rehab**: constraint-induced movement therapy (force use of affected limb)
- **BCI training**: user learns control → cortical plasticity
- **Tinnitus / phantom pain**: maladaptive plasticity → mirror therapy
- **Antidepressant**: SSRI increases plasticity (BDNF)
- **Stroke + brain training**

---

## 9. Negative Plasticity ("Maladaptive")

- Phantom limb pain
- Chronic pain centralization
- Addiction (reward circuit remodeling)
- PTSD (fear memory strengthening)
- Dystonia (excessive representation overlap)

---

## 10. AI Analogy

- **Backprop ≠ Hebbian**: biology uses local rules; backprop not bio-plausible
- **Continual learning**: AI catastrophic forgetting vs brain continuous learning
- **Transfer learning** ↔ experience transfer
- **Lottery ticket / pruning** ↔ developmental pruning

---

## 11. Common Pitfalls

### 11.1 Adult Brain Unchangeable

Lifelong plasticity; just weaker after critical period.

### 11.2 Plasticity Always Good

Maladaptive plasticity (pain, addiction) also exists.

### 11.3 Hebbian = Backprop

Backprop not biologically plausible (weight transport problem).

### 11.4 Neurogenesis Widespread

Adult mainly limited to hippocampal DG (and debated in humans); not whole brain.

### 11.5 More Use = Definitely Stronger

Needs correct + meaningful training; useless repetition may not help.

---

## 12. Related Concepts

- **Same section**: [Neurodevelopment](Neurodevelopment.en.md), [Neural Coding](Neural_Coding.en.md)
- **Cellular**: [LTP/LTD](../02_Cellular_Molecular/LTP_LTD.en.md), [Synapse](../02_Cellular_Molecular/Synapse.en.md), [Dendrites](../02_Cellular_Molecular/Dendrites.en.md)
- **Systems**: [Hippocampus Memory](../03_Systems_Neuroscience/Hippocampus_Memory.en.md)

---

## References

1. **Hebb, D. O.** *The Organization of Behavior*. Wiley, 1949.
2. **Pascual-Leone, A. et al.** "The plastic human brain cortex." *Annu Rev Neurosci*, 2005.
3. **Maguire, E. A. et al.** "Navigation-related structural change in the hippocampi of taxi drivers." *PNAS*, 2000.
4. **Holtmaat, A. & Svoboda, K.** "Experience-dependent structural synaptic plasticity in the mammalian brain." *Nat Rev Neurosci*, 2009.
