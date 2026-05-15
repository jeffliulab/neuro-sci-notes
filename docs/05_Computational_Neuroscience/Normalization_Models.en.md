# Divisive Normalization

> *Divisive normalization (Heeger 1992, Carandini & Heeger 2012) is the brain's "canonical computation": a neuron's response is divided by the summed activity of a nearby population. Explains V1 contrast saturation, cross-orientation suppression, attention, multisensory integration, value coding. One of few operations universal across brain regions — like deep learning's LayerNorm/softmax.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Visual System](../03_Systems_Neuroscience/Visual_System.en.md), [Neural Circuits](../00_Foundations/Neural_Circuits.en.md)

---

## 1. Formula

$$R_i = \frac{D_i^n}{\sigma^n + \sum_j w_{ij} D_j^n}$$

- $D_i$: driving input of neuron i
- Denominator: normalization pool
- $\sigma$: semi-saturation constant
- $n$: exponent (~ 2)

→ Response is "divided" by population activity.

---

## 2. Phenomena Explained

| Phenomenon | Mechanism |
|---|---|
| Contrast saturation | High contrast → large denominator → saturation |
| Cross-orientation suppression | Orthogonal grating enters norm pool |
| Surround suppression | Surround enters pool |
| Attention | Adjust normalization weight / gain |
| Redundancy reduction | Decorrelation |
| Adaptation | Dynamic σ |

---

## 3. Canonical Computation

- Carandini & Heeger 2012: normalization appears in
  - Retina, LGN, V1, MT, IT
  - Auditory, olfactory
  - Multisensory, value (LIP, OFC)
- → A **universal computational primitive** across brain regions

---

## 4. Relation to Attention

- Reynolds & Heeger 2009 normalization model of attention
- Attention adjusts normalization → contrast gain vs response gain
- Unified explanation of spatial / feature attention effects

---

## 5. PyTorch — Divisive Normalization

```python
import torch
import torch.nn.functional as F

def divisive_normalization(drive, sigma=0.5, n=2.0):
    """drive: (B, C, H, W). Normalize by local pool."""
    d_n = drive.abs() ** n
    # Normalization pool: sum over channels (cross-feature)
    pool = d_n.sum(dim=1, keepdim=True)
    return d_n / (sigma**n + pool)

# Compare with softmax / LayerNorm — same divisive motif
```

---

## 6. Relation to Deep Learning

| Brain norm | DL |
|---|---|
| Divisive normalization | LayerNorm / softmax |
| Cross-orientation suppression | channel competition |
| Gain control | BatchNorm gain |
| Sigmoid saturation | activation saturation |
| Attention via norm | softmax attention |

Normalization is a core motif shared by brain and DL.

---

## 7. Mechanistic Implementation

- Shunting inhibition (→ division)
- Feedback inhibition pool
- Synaptic depression
- Multiple biological mechanisms can implement the same normative operation

---

## 8. Value Normalization (Economic Decision)

- Louie & Glimcher: LIP/OFC value coding is normalized
- → context-dependent choice, IIA violation
- "My value for a burger depends on other menu options"
- Explains decoy effect and other behavioral economics anomalies

---

## 9. Adaptive Advantages

- Extends dynamic range (limited spikes → encode large range)
- Decorrelation → efficient coding (see [Efficient Coding](Efficient_Coding.en.md))
- Invariance (contrast-invariant orientation tuning)

---

## 10. Common Pitfalls

### 10.1 Vision Only

Cross-modal + value + attention universal.

### 10.2 Single Mechanism

Multiple biological mechanisms (shunting, feedback, depression) implement same operation.

### 10.3 Norm Pool = All Neurons

Pool has selective weights (not all equal weight).

### 10.4 Linear Operation

Fundamentally nonlinear (division + exponent).

### 10.5 Unrelated to Attention

Attention works precisely by modulating normalization (Reynolds-Heeger).

---

## 11. Related Concepts

- **Same section**: [Efficient Coding](Efficient_Coding.en.md), [Predictive Coding](Predictive_Coding.en.md)
- **Foundation**: [Neural Circuits](../00_Foundations/Neural_Circuits.en.md)
- **Systems**: [Visual System](../03_Systems_Neuroscience/Visual_System.en.md)
- **Cognition**: [Attention](../04_Cognitive_Neuroscience/Attention.en.md), [Decision Making](../04_Cognitive_Neuroscience/Decision_Making.en.md)
- **AI**: LayerNorm, softmax attention

---

## References

1. **Heeger, D. J.** "Normalization of cell responses in cat striate cortex." *Vis Neurosci*, 1992.
2. **Carandini, M. & Heeger, D. J.** "Normalization as a canonical neural computation." *Nat Rev Neurosci*, 2012.
3. **Reynolds, J. H. & Heeger, D. J.** "The normalization model of attention." *Neuron*, 2009.
4. **Louie, K. & Glimcher, P. W.** "Efficient coding and the neural representation of value." *Ann NY Acad Sci*, 2012.
