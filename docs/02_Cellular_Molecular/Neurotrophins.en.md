# Neurotrophins

> *Neurotrophins (NGF, BDNF, NT-3, NT-4) are secreted proteins regulating neuron survival, growth, differentiation, synaptic plasticity. Levi-Montalcini discovered NGF (1986 Nobel). The "neurotrophic hypothesis": target-derived, limited-supply competition → developmental programmed death. BDNF/TrkB is the core molecule of LTP, learning, antidepressant action.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Neurodevelopment](../00_Foundations/Neurodevelopment.en.md), [LTP_LTD](LTP_LTD.en.md)

---

## 1. Family + Receptors

| Neurotrophin | High-affinity receptor (Trk) | Low-affinity |
|---|---|---|
| NGF | TrkA | p75NTR |
| **BDNF** | TrkB | p75NTR |
| NT-3 | TrkC (also TrkA/B) | p75NTR |
| NT-4 | TrkB | p75NTR |

- Trk: receptor tyrosine kinase → survival/growth (PI3K-Akt, MAPK, PLCγ)
- **p75NTR**: can promote apoptosis (esp. proNT form) → "yin-yang"

---

## 2. Neurotrophic Hypothesis

- Target tissue secretes **limited** NGF → neurons compete
- Sufficient → survive; insufficient → **programmed apoptosis**
- Explains ~50% neuron death in development (matches input-target)
- Hamburger & Levi-Montalcini classic (see [Neurodevelopment](../00_Foundations/Neurodevelopment.en.md))

---

## 3. Retrograde Transport Signaling

- Axon terminal takes up NGF → **retrograde** transport (dynein) → soma nucleus
- "Signaling endosome": endosome carrying activated Trk
- Long-distance survival signal (terminal → nucleus, several cm)
- See [Axonal_Transport](Axonal_Transport.en.md)

---

## 4. BDNF — Plasticity Core

- Activity-dependent expression (CREB-regulated, see [Second_Messengers](Second_Messengers.en.md))
- TrkB → promotes LTP, dendritic growth, new synapses
- Key to "synaptic + cellular consolidation" (long-term memory)
- Val66Met polymorphism → ↓ secretion → memory/mood phenotype (common in population)

---

## 5. PyTorch — Limited-Supply Competition Survival

```python
import torch

def neurotrophic_competition(n_neurons=100, ngf_supply=30.0):
    """Neurons compete for limited target NGF; insufficient -> apoptosis."""
    uptake = torch.rand(n_neurons)
    uptake = uptake / uptake.sum() * ngf_supply       # share fixed supply
    survival_threshold = 0.2
    survives = uptake > survival_threshold
    return survives.sum().item()   # only well-supplied neurons survive
```

---

## 6. proNT vs Mature ("yin-yang")

- **proBDNF** (precursor) → p75NTR → promotes apoptosis / LTD
- **mBDNF** (mature, protease-cleaved) → TrkB → survival / LTP
- Cleavage balance (plasmin/MMP) = bidirectional switch
- Explains same-molecule opposite effects

---

## 7. Clinical

- **Depression**: BDNF↓ (stress/hippocampus); antidepressants/exercise/ECT → BDNF↑ ("neurotrophic hypothesis", see [Depression](../08_Neuro_Disorders/Depression.en.md))
- **Neurodegeneration**: NGF/BDNF↓ (AD, HD, PD) → neurotrophic factor therapy trials (delivery hard, see [Gene_Therapy_CNS](../07_Neurotech_Frontiers/Gene_Therapy_CNS.en.md))
- **Pain**: NGF → hyperalgesia → anti-NGF antibody (tanezumab) analgesia
- Peripheral nerve regeneration (Schwann cell secretion)

---

## 8. Relation to Exercise / Environment

- Aerobic exercise → BDNF↑ (hippocampus) → cognitive/mood benefit (see [Cognitive_Aging](../04_Cognitive_Neuroscience/Cognitive_Aging.en.md))
- Environmental enrichment → neurotrophic factors + neurogenesis
- Molecular mediator of "exercise as medicine"

---

## 9. Relation to AI

- Limited-supply competition survival ↔ "use-driven" retention of neurons/connections (pruning + resource competition, see [Neurodevelopment](../00_Foundations/Neurodevelopment.en.md))
- Activity-dependent BDNF ↔ activity-gated plasticity (three-factor, see [Synaptic Plasticity Models](../05_Computational_Neuroscience/Synaptic_Plasticity_Models.en.md))
- Neurotrophic factor = global/local modulatory scalar

---

## 10. Common Pitfalls

### 10.1 Neurotrophins Only for Development

Lifelong regulation of plasticity/survival (BDNF core to learning/antidepressant).

### 10.2 All Promote Growth

p75NTR + proNT can promote **apoptosis** (yin-yang).

### 10.3 BDNF↑ Always Good

Excess/ectopic can cause hyperalgesia/epilepsy; needs precise regulation.

### 10.4 Neurotrophins Easy Drugs

Proteins hard to cross BBB + short half-life + side effects → major delivery challenge.

### 10.5 NGF Only Sympathetic/Sensory

Broad (basal forebrain cholinergic etc.); BDNF more widespread.

---

## 11. Related Concepts

- **Same section**: [LTP_LTD](LTP_LTD.en.md), [Second_Messengers](Second_Messengers.en.md), [Synapse](Synapse.en.md), [Axonal_Transport](Axonal_Transport.en.md)
- **Foundation**: [Neurodevelopment](../00_Foundations/Neurodevelopment.en.md), [Neuroplasticity](../00_Foundations/Neuroplasticity.en.md)
- **Disease**: [Depression](../08_Neuro_Disorders/Depression.en.md)

---

## References

1. **Levi-Montalcini, R.** "The nerve growth factor 35 years later." *Science*, 1987 (Nobel).
2. **Huang, E. J. & Reichardt, L. F.** "Neurotrophins: roles in neuronal development and function." *Annu Rev Neurosci*, 2001.
3. **Lu, B. et al.** "BDNF-based synaptic repair as a disease-modifying strategy." *Nat Rev Neurosci*, 2013.
4. **Park, H. & Poo, M.-M.** "Neurotrophin regulation of neural circuit development and function." *Nat Rev Neurosci*, 2013.
