# Neurodevelopment

> *From single cell to 86-billion-neuron network: neural tube → proliferation → migration → differentiation → axon guidance → synaptogenesis → pruning. Key processes: neurogenesis, apoptosis (excess neurons die), synaptic pruning, myelination (continues to age 25). Critical periods set plasticity windows. Developmental abnormalities → autism, schizophrenia, ID.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Nervous System Overview](Nervous_System_Overview.en.md), cell biology basics

---

## 1. Major Stages

```
Neural induction (neural plate)
   ↓
Neural tube formation
   ↓
Proliferation (progenitor proliferation)
   ↓
Migration (radial migration)
   ↓
Differentiation (into specific neurons)
   ↓
Axon guidance (growth cone navigation)
   ↓
Synaptogenesis (synapse formation)
   ↓
Pruning + Myelination
```

---

## 2. Neural Induction

- Spemann organizer (1924 Nobel)
- BMP inhibition → ectoderm → neural plate
- Default model: neural is the default fate of ectoderm

---

## 3. Neurulation

- Neural plate → neural groove → neural tube
- Closure failure → spina bifida / anencephaly
- Folate prevents neural tube defects

---

## 4. Proliferation + Migration

- Ventricular zone radial glia → progenitors
- **Radial migration**: climb along radial glia (inside-out: deep layers first)
- **Tangential migration**: interneurons move laterally
- Abnormality → lissencephaly (smooth brain)

---

## 5. Axon Guidance

- Growth cone senses cues
- **Attractants / Repellents**: Netrin, Slit, Ephrin, Semaphorin
- Gradient sensing → precise wiring
- E.g.: retina → tectum retinotopic map (Sperry chemoaffinity)

---

## 6. Synaptogenesis + Overproduction

- Early synapses **overproduced**
- Synaptic density peaks at 2-3 years (above adult)
- "Blooming and pruning"

---

## 7. Apoptosis + Pruning

- ~ 50% of neurons undergo programmed death (neurotrophic factor competition — NGF)
- Synaptic pruning: use-it-or-lose-it
- Massive pruning in adolescence (esp. PFC)
- Abnormal pruning → schizophrenia (excess) / autism (insufficient) hypotheses

---

## 8. Critical Periods

- Specific windows of high plasticity
- Hubel & Wiesel: monocular deprivation → permanent amblyopia (if past window)
- Language: second language hard to reach native after ~ 7 years
- Mechanisms: GABA maturation, PV interneurons, perineuronal nets

---

## 9. Myelination

- Continues postnatally to ~ 25 years
- PFC last (explains adolescent immature decision-making)
- Experience-dependent (activity-dependent myelination)

---

## 10. PyTorch — Pruning Analogy

```python
import torch

def developmental_pruning(weights, activity, prune_ratio=0.5):
    """Use-it-or-lose-it: prune low-activity synapses."""
    importance = weights.abs() * activity  # active + strong survive
    threshold = importance.flatten().quantile(prune_ratio)
    mask = importance >= threshold
    return weights * mask  # pruned network (sparse, like adult brain)
```

→ Similar to deep learning magnitude pruning / lottery ticket.

---

## 11. Developmental Disorders

- **Autism**: synaptic / connectivity abnormality
- **Schizophrenia**: adolescent over-pruning hypothesis
- **Fragile X**: FMRP → abnormal spines
- **Rett**: MECP2
- **Fetal alcohol**: migration / apoptosis disruption
- **Microcephaly**: ZIKV, genetic → insufficient proliferation

---

## 12. Common Pitfalls

### 12.1 Neurons Never Regenerate

Adult hippocampus / olfactory bulb still have neurogenesis (small, debated).

### 12.2 More Synapses = Smarter

Pruning is a feature, not a bug; refined networks more efficient.

### 12.3 Critical Period Absolutely Closed

Can be reopened (drugs, training, environment).

### 12.4 Development = Genes Only

Experience + activity strongly shape (nature × nurture).

### 12.5 Myelination Done in Childhood

PFC myelination continues to ~ 25 years.

---

## 13. Related Concepts

- **Same section**: [Nervous System Overview](Nervous_System_Overview.en.md), [Neuron Doctrine](Neuron_Doctrine.en.md)
- **Cellular**: [Synapse](../02_Cellular_Molecular/Synapse.en.md), [LTP/LTD](../02_Cellular_Molecular/LTP_LTD.en.md)
- **Disease**: [Autism](../08_Neuro_Disorders/Autism.en.md), [Schizophrenia](../08_Neuro_Disorders/Schizophrenia.en.md)

---

## References

1. **Sanes, D. H., Reh, T. A., Harris, W. A.** *Development of the Nervous System*. 3rd ed., 2011.
2. **Hubel, D. H. & Wiesel, T. N.** "The period of susceptibility to the physiological effects of unilateral eye closure in kittens." *J Physiol*, 1970.
3. **Sperry, R. W.** "Chemoaffinity in the orderly growth of nerve fiber patterns." *PNAS*, 1963.
4. **Stiles, J. & Jernigan, T. L.** "The basics of brain development." *Neuropsychol Rev*, 2010.
