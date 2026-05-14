# Hippocampus Anatomy

> *The hippocampus is a C-shaped structure in the temporal lobe, central to episodic memory + spatial navigation. Patient H.M. (1957 bilateral resection) made hippocampus the focus of memory research. This article covers anatomy + circuit + function.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Cortex](Cortex.en.md), [Neuron](../02_Cellular_Molecular/Neuron.en.md)

---

## 1. Anatomical Structure

```
Entorhinal Cortex (EC) — entry
    ↓ perforant path
Dentate Gyrus (DG) — granule cells
    ↓ mossy fiber
CA3 — large pyramidal, recurrent
    ↓ Schaffer collateral
CA1 — main output to EC
    ↓
Subiculum → output
```

3 main regions: DG, CA3, CA1. CA = Cornu Ammonis.

---

## 2. Trisynaptic Circuit

Classical hippocampal information flow:

1. **EC layer 2 → DG**: perforant path
2. **DG granule cells → CA3**: mossy fibers
3. **CA3 pyramidal → CA1**: Schaffer collaterals + recurrent CA3 collaterals

Each synapse is a classic LTP study site.

---

## 3. CA3 Recurrent Network

CA3 pyramidals have ~3% interconnection probability → strong recurrence.
Function:
- **Pattern completion**: partial cue → full recall (Marr 1971 hypothesis)
- **Auto-associative memory**: similar to Hopfield network

---

## 4. CA1

Main "output" — pyramidals project to subiculum + EC layer 5.
- Widely connected with cortex
- Place cells strongly active in CA1

---

## 5. Place Cells & Grid Cells

### 5.1 Place Cells (O'Keefe 1971)

CA1 / CA3 pyramidals fire at specific spatial locations (place fields).
**Hippocampus is brain's GPS**. Nobel 2014.

### 5.2 Grid Cells (Moser 2005)

EC layer 2 (Medial EC), grid-like firing — multiple hexagonal place fields.
Provides metric for path integration.

### 5.3 Head-direction, Border, Speed Cells

EC + presubiculum also have direction, border, speed cells.

---

## 6. Relation to Cortex

Hippocampus receives **multimodal integrated input** (via EC):
- Visual, auditory, olfactory, somatosensory
- Whole-brain info highly integrated → hippocampus
- Output back to neocortex (memory consolidation)

---

## 7. Role in Memory

### 7.1 Episodic Memory

- Personal experiences (where, when, what)
- Hippocampus essential (H.M. case)

### 7.2 Memory Consolidation

- Hippocampus is **temporary store**
- Days-years: memory gradually transferred to cortex
- Sleep accelerates consolidation via hippocampal replay

### 7.3 Spatial Navigation

- Hippocampus + EC work together for navigation
- London taxi driver hippocampus larger (Maguire 2000)

---

## 8. Pathology

- **H.M.** (1957 bilateral surgery): anterograde amnesia
- **Alzheimer**: hippocampus is early damage area
- **Temporal lobe epilepsy**: hippocampal sclerosis common
- **PTSD**: reduced hippocampal volume
- **Depression**: reduced hippocampal LTP + neurogenesis

---

## 9. PyTorch — Hopfield Network (CA3 abstraction)

```python
import torch

class HopfieldNetwork:
    def __init__(self, N=100):
        self.W = torch.zeros(N, N)
        self.N = N
    
    def store(self, patterns):
        for p in patterns:
            self.W += torch.outer(p, p) / self.N
        self.W.fill_diagonal_(0)
    
    def recall(self, initial, steps=10):
        s = initial.clone()
        for _ in range(steps):
            s = torch.sign(self.W @ s)
        return s

net = HopfieldNetwork(N=64)
patterns = [torch.sign(torch.randn(64)) for _ in range(5)]
net.store(patterns)
partial_cue = patterns[0].clone()
partial_cue[20:30] = 0
recalled = net.recall(partial_cue)
```

---

## 10. Measurement

- **fMRI**: hippocampal activation in memory tasks
- **In vivo recording**: place cells via tetrode / Neuropixels
- **Imaging**: hippocampal volume (MRI) for AD biomarker
- **Optogenetics**: activate / suppress specific cells (engram)

---

## 11. Common Pitfalls

### 11.1 Hippocampus ≠ All Memory

Only declarative memory; procedural / motor memory by cerebellum / BG.

### 11.2 H.M. Could Still Learn Motor Skills

Proves memory is multi-system.

### 11.3 Place Cells Aren't Just Navigation

Also encode temporal sequences, social relations etc.

### 11.4 Hopfield ≠ Real CA3

Hopfield is oversimplified; real CA3 has inhibitory interneurons + plasticity + spike timing.

### 11.5 Hippocampus Size ≠ Intelligence

Human hippocampus is small; dolphins / elephants have larger. What matters is connectivity with cortex.

---

## 12. Related Concepts

- **Same section**: [Cortex](Cortex.en.md), [Cerebellum](Cerebellum.en.md), [Basal Ganglia](Basal_Ganglia.en.md)
- **Cellular**: [LTP/LTD](../02_Cellular_Molecular/LTP_LTD.en.md)
- **AI comparison**: Hopfield → https://jeffliulab.github.io/ai-notes/01_AI/03_Frontiers/03_World_Models/RSSM_PlaNet/

---

## References

1. **Scoville, W. B. & Milner, B.** "Loss of recent memory after bilateral hippocampal lesions (H.M.)." *J Neurol Neurosurg Psychiatry*, 1957.
2. **O'Keefe, J. & Dostrovsky, J.** "The hippocampus as a spatial map." *Brain Res*, 1971.
3. **Hafting, T. et al.** "Microstructure of a spatial map in the entorhinal cortex (Grid cells)." *Nature*, 2005.
4. **Marr, D.** "Simple memory: a theory for archicortex." *Phil Trans R Soc*, 1971.
5. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
