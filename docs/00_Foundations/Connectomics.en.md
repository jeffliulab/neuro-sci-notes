# Connectomics

> *A connectome is the complete wiring diagram of a nervous system. From C. elegans 302 neurons (White 1986) to Drosophila whole brain (FlyWire 2024, ~140k neurons) to mouse cortex 1 mm³ (MICrONS 2025). EM + AI segmentation are key technologies. Debate: is the connectome sufficient to understand function (Seung "I am my connectome" vs critics).*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Research Methods](Research_Methods.en.md), [Neuron Doctrine](Neuron_Doctrine.en.md)

---

## 1. Definition

- **Connectome**: complete graph of all neurons + all synapses
- Analogy: genome is to genetics → connectome is to wiring
- Multi-scale:
  - Micro: single synapse (EM)
  - Meso: cell-type populations
  - Macro: inter-region (DTI tractography)

---

## 2. Milestones

| Year | System | Scale |
|---|---|---|
| 1986 | C. elegans (White) | 302 neurons, ~7000 synapses |
| 2013 | Mouse retina | Partial |
| 2018 | Drosophila larva | Partial |
| 2024 | Drosophila adult whole brain (FlyWire) | ~140,000 neurons, ~50M synapses |
| 2025 | Mouse visual cortex (MICrONS) | 1 mm³, ~100k neurons |
| Future | Mouse whole brain / human | Still distant |

---

## 3. Technical Pipeline

```
Tissue fixation + staining (heavy metal)
   ↓
Ultra-thin sectioning (~ 30 nm) or block-face
   ↓
Electron microscopy (nm resolution)
   ↓ TB-PB scale data
AI segmentation (3D U-Net, flood-filling)
   ↓
Synapse detection + proofreading
   ↓
Connectome graph
```

---

## 4. EM Types

- **ssTEM**: serial section TEM
- **SBEM**: serial block-face EM
- **FIB-SEM**: focused ion beam (high z-resolution)
- **ATUM**: automated section collection
- Data volume: mouse whole brain ~ exabyte scale (challenge)

---

## 5. AI in Connectomics

- **3D segmentation**: flood-filling networks (Januszewski 2018)
- **Synapse detection**: CNN
- **Proofreading**: human + AI collaboration (FlyWire crowdsourced)
- Without deep learning → connectomics infeasible (manual annotation too slow)

---

## 6. PyTorch — Connectome Graph Analysis

```python
import torch

def graph_metrics(adjacency):
    """Basic connectome graph metrics."""
    A = adjacency
    degree = A.sum(dim=1)              # out-degree
    in_degree = A.sum(dim=0)           # in-degree
    # Reciprocity: fraction of bidirectional edges
    recip = ((A > 0) & (A.t() > 0)).float().sum() / (A > 0).float().sum()
    # Hub: high-degree nodes
    hubs = torch.topk(degree, k=5).indices
    return degree, recip, hubs
```

---

## 7. Finding — C. elegans

- 302-neuron full graph known ~ 40 years
- But still **cannot fully predict behavior**!
- Reasons: connectome lacks neuromodulator (extrasynaptic), synaptic strength, dynamics
- Lesson: wiring ≠ function

---

## 8. Connectome ≠ Function Debate

| Support (Seung) | Criticism |
|---|---|
| Wiring determines computation | Lacks synaptic weights |
| Necessary foundation | Lacks neuromodulation |
| Analogy to genome | Lacks dynamics + plasticity |
| Static blueprint valuable | C. elegans counterexample |

→ Consensus: connectome necessary but not sufficient.

---

## 9. Macro Connectome (Human)

- **DTI tractography**: white matter tracts (not single synapses)
- **Human Connectome Project** (HCP): healthy + disease
- Resolution limited to mm (not neuron-level)
- "Connectopathy": psychiatric disease as connectivity abnormality

---

## 10. AI Analogy

- ANN weight matrix = artificial connectome
- But ANN fully visible + controllable; biological connectome hard to measure
- Mechanistic interpretability ↔ "reverse-engineering the connectome of an LLM"

---

## 11. Common Pitfalls

### 11.1 Connectome → Understanding Brain

C. elegans counterexample: graph for 40 years, still not fully understood.

### 11.2 EM Graph Has Weights

EM only morphology (rough synapse size); strength needs physiology.

### 11.3 DTI = Neuron Connectome

DTI is mm-scale tracts, not single synapses.

### 11.4 Static Sufficient

Lacks plasticity + neuromodulation + dynamics.

### 11.5 Human Connectome Soon Complete

Data volume (exabyte) + individual variation → still very distant.

---

## 12. Related Concepts

- **Same section**: [Research Methods](Research_Methods.en.md), [Neuron Doctrine](Neuron_Doctrine.en.md), [Levels of Analysis](Levels_of_Analysis.en.md)
- **Computational**: [Hopfield Networks](../05_Computational_Neuroscience/Hopfield_Networks.en.md)
- **Frontiers**: [Calcium Imaging](../07_Neurotech_Frontiers/Calcium_Imaging.en.md)

---

## References

1. **White, J. G. et al.** "The structure of the nervous system of the nematode C. elegans." *Phil Trans R Soc B*, 1986.
2. **Seung, S.** *Connectome*. HMH, 2012.
3. **Januszewski, M. et al.** "High-precision automated reconstruction of neurons with flood-filling networks." *Nat Methods*, 2018.
4. **Dorkenwald, S. et al.** "Neuronal wiring diagram of an adult brain (FlyWire)." *Nature*, 2024.
