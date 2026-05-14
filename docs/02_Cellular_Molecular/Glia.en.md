# Glia — Beyond "Support Cells"

> *Glia were long thought to be just "glue" for neurons, but since the 1990s research found glia actively participate in synapses / immunity / metabolism / plasticity. Brain glia:neuron ≈ 1:1. This article covers 4 main glia types + functions + disease links.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Neuron](Neuron.en.md), [Synapse](Synapse.en.md)

---

## 1. Four Main Glia Types

### 1.1 Astrocytes

Most numerous; star-shaped; surround neurons + blood vessels:
- Maintain K+, Glu concentrations
- Form **Blood-Brain Barrier (BBB)**
- Release ATP, glutamate ("gliotransmission")
- Participate in LTP

### 1.2 Oligodendrocytes

Produce **myelin** in CNS — one oligo wraps multiple axons.
- MS (multiple sclerosis) attacks oligos
- Myelin accelerates signals 10-100×

### 1.3 Microglia

CNS immune cells:
- Patrol + phagocytose debris
- Participate in synaptic pruning (development)
- Over-activated in Alzheimer / Parkinson → inflammation

### 1.4 Schwann Cells

PNS myelin + nerve repair.
1 Schwann wraps 1 axon (vs Oligo 1 wraps many).

---

## 2. Astrocyte Detailed Functions

### 2.1 Tripartite Synapse

Classic synapse = pre + post; new finding **astrocyte wrapping** forms tripartite:

```
[Pre]───glutamate───[Post]
   \                /
    \─── astrocyte ─┘ (uptake Glu, release ATP)
```

### 2.2 BBB

Astrocyte end-feet surround brain blood vessels + tight junctions → block macromolecules / drugs from entering brain.
Drug design often requires BBB crossing.

### 2.3 K+ Siphoning

Astrocytes absorb high K+ after spikes, distribute via gap junction network → maintain homeostasis.

### 2.4 Glymphatic System

During sleep, CSF flows through brain tissue via astrocyte aquaporin 4 → clears metabolic waste (β-amyloid etc.).

---

## 3. Microglia Detail

### 3.1 Surveillance

Normal ramified shape → long processes patrolling.
Damage → ameboid activated → phagocytose.

### 3.2 Synaptic Pruning

During development microglia eat weak synapses → improve circuit efficiency.
Abnormal → autism hypothesis.

### 3.3 Neuroinflammation

Over-activation releases TNF-α, IL-6 → chronic inflammation → neurodegeneration (AD, PD, ALS).

---

## 4. Numbers

- Glia : neuron = 1:1 (old 10:1 view corrected, Azevedo 2009)
- Varies by region:
  - Cerebellum: 4:1 (more neurons)
  - Cortex: 3:1 (more glia)
- White matter has more oligos; gray matter more astrocytes + microglia

---

## 5. Glia and Disease

| Disease | Glia Role |
|---|---|
| Multiple Sclerosis | Oligo attacked by immunity → demyelination |
| ALS | Astrocyte dysfunction, microglia inflammation |
| Alzheimer | Microglia can't clear amyloid + inflammation |
| Parkinson | α-synuclein triggers microglia |
| Glioma | Glia cancer (most common brain tumor) |
| Charcot-Marie-Tooth | Schwann cell disease |
| Autism | Microglia synaptic pruning abnormal |

---

## 6. Modern Research

### 6.1 Single-cell RNA-seq

Found ~30 astrocyte subtypes, ~20 microglia states.

### 6.2 Optogenetic Glia

ChR2 in astrocytes → light-controlled gliotransmission → behavior effects.

### 6.3 iPSC-derived Glia

Use stem cells to differentiate astrocytes for testing human disease.

### 6.4 Reactive Astrogliosis Detection

GFAP biomarker — astrocyte response post stroke / trauma.

---

## 7. PyTorch — Tripartite Synapse Concept

```python
import torch

class TripartiteSynapse:
    def __init__(self, w=0.5, glia_capacity=10):
        self.w = w
        self.glu = 0
        self.glia_glu = 0
        self.capacity = glia_capacity
    
    def step(self, pre_spike, post_state, dt=1):
        if pre_spike:
            self.glu += 1.0
        uptake = min(self.glu * 0.5, self.capacity)
        self.glu -= uptake * dt
        self.glia_glu += uptake * dt
        atp_release = self.glia_glu * 0.01
        self.glia_glu *= (1 - 0.01 * dt)
        epsp = self.w * self.glu + 0.1 * atp_release
        return epsp
```

---

## 8. Experimental Tools

- **GFAP staining**: astrocyte marker
- **Iba1 staining**: microglia marker
- **MBP staining**: oligo / myelin
- **Calcium imaging**: astrocytes also have Ca²⁺ signals

---

## 9. History

- **1858** — Rudolf Virchow named "neuroglia" ("nerve glue")
- **1890s** — Ramón y Cajal classified neurons vs glia
- **1990s** — gliotransmission concept
- **2000s** — synaptic pruning discovered
- **2010s** — glymphatic system (Nedergaard 2012)
- **2020s** — single-cell glia diversity

---

## 10. Common Pitfalls

### 10.1 Glia Are Not Passive

For 50 years glia were underestimated; modern research shows active participation.

### 10.2 Single Astrocyte Misconception

At least 30 subtypes with very different functions.

### 10.3 Microglia Blind Spot

Most CNS research focuses on neurons; microglia influence underestimated.

### 10.4 BBB Complexity

Astrocytes are part of BBB, but endothelial cells + pericytes also are.

### 10.5 In Vitro vs In Vivo

Cultured astrocytes have very different morphology + gene expression.

---

## 11. Related Concepts

- **Same section**: [Neuron](Neuron.en.md), [Synapse](Synapse.en.md), [Neurotransmitters](Neurotransmitters.en.md)

---

## References

1. **Verkhratsky, A. & Butt, A.** *Glial Physiology and Pathophysiology*. 2nd ed., 2013.
2. **Allen, N. J. & Lyons, D. A.** "Glia as architects of central nervous system formation and function." *Science*, 2018.
3. **Nedergaard, M.** "Garbage truck of the brain (glymphatic system)." *Science*, 2013.
4. **Azevedo, F. et al.** *J Comp Neurol*, 2009.
5. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
