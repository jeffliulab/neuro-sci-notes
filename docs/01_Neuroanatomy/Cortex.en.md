# Cerebral Cortex — 6-Layer Structure and Functional Areas

> *The cerebral cortex is the most recently evolved and thickest part of the mammalian brain. Human cortex is 2-4 mm thick, ~ 0.25 m² unfolded surface, containing ~ 16 billion neurons. 6-layer structure + 50+ functional areas (Brodmann).*
>
> **Difficulty**: Introduction-Intermediate
> **Prerequisites**: [Neuron](../02_Cellular_Molecular/Neuron.en.md)

---

## 1. Overall Structure

- 2-4 mm thick
- Gray matter (cell bodies), below is white matter (axons)
- 6 cell layers (neocortex); hippocampal-adjacent is allocortex (3-5 layers)
- Surface has sulci (grooves) and gyri (ridges)

---

## 2. 6 Layers

```
Layer 1: Molecular - few neurons, mainly dendritic tufts
Layer 2: External granular - small pyramidal + interneurons
Layer 3: External pyramidal - large pyramidal, outputs to other cortex
Layer 4: Internal granular - main thalamic input receiving layer
Layer 5: Internal pyramidal - large pyramidal, outputs to subcortical (BG/SC)
Layer 6: Multiform - polymorphic, feedback to thalamus
```

### 2.1 Input / Output

- **Thalamic input**: Layer 4 (except motor cortex)
- **Cortico-cortical**: Layer 2/3
- **Subcortical output**: Layer 5 → BG, brainstem, spinal cord
- **Cortico-thalamic**: Layer 6 → thalamus

---

## 3. Cortical Columns

Mountcastle 1957:
- Neurons in same column (~0.5 mm wide) encode similar stimuli
- Cortex **functional unit**
- ~10,000 neurons per column

---

## 4. Functional Areas (Brodmann)

Brodmann 1909 divided 52 areas by cytoarchitecture. Main functional areas:

### 4.1 Sensory
- **V1** (17): visual
- **A1** (41/42): auditory
- **S1** (1/2/3): somatosensory

### 4.2 Motor
- **M1** (4): primary motor
- **Premotor** (6): motor planning
- **SMA**: internally triggered actions

### 4.3 Association
- **PFC** (9/10/46): working memory, decision, personality
- **Parietal** (5/7): spatial attention, sensory fusion
- **Temporal** (20/21): object recognition, language

### 4.4 Language
- **Broca** (44/45): production
- **Wernicke** (22): comprehension

---

## 5. Four Lobes

- **Frontal**: planning, decision, motor
- **Parietal**: spatial, touch
- **Temporal**: auditory, memory, recognition
- **Occipital**: visual

---

## 6. Lateralization

- **Left**: language (most right-handed), logic, sequence
- **Right**: spatial, faces, holistic

Split-brain experiments (Sperry 1960s) — Nobel 1981.

---

## 7. vs AI

| Aspect | Cortex | CNN/Transformer |
|---|---|---|
| Layers | 6 | 10-100 |
| Internal | local columns | local attention |
| Input | thalamus → L4 | input → layer 1 |
| Output | L5 → action | output head |
| Learning | STDP / modulation | backprop |
| Neurons | 16 B | up to 1T params |

---

## 8. Pathology

- **Stroke**: focal cortex damage → loss of function
- **Cortical atrophy** (Alzheimer): gray matter reduction
- **Cortical dysplasia**: developmental abnormality → epilepsy

---

## 9. PyTorch — Simplified Cortical Column

```python
import torch, torch.nn as nn

class CorticalColumn(nn.Module):
    def __init__(self, dim=512):
        super().__init__()
        self.L4 = nn.Linear(dim, dim)
        self.L2_3 = nn.Linear(dim, dim)
        self.L5 = nn.Linear(dim, dim)
        self.L6 = nn.Linear(dim, dim)
    
    def forward(self, thal_input):
        l4 = torch.relu(self.L4(thal_input))
        l2_3 = torch.relu(self.L2_3(l4))
        l5_out = torch.tanh(self.L5(l2_3))
        l6_feedback = self.L6(l2_3)
        return l5_out, l6_feedback
```

---

## 10. Common Pitfalls

### 10.1 Brodmann ≠ Function

Brodmann is cytoarchitectonic; doesn't perfectly match fine functional boundaries.

### 10.2 Column Reality

Clear in some species (rat/cat/monkey); less clear in humans.

### 10.3 Lateralization Oversimplified

"Right brain creative" etc. are pop misconceptions.

### 10.4 Cortex ≠ Sole Intelligence

Subcortical (hippocampus / BG / cerebellum) equally critical.

### 10.5 Allocortex vs Neocortex

Hippocampus / olfactory are allocortex, only 3 layers.

---

## 11. Related Concepts

- **Same section**: [Hippocampus Anatomy](Hippocampus_Anatomy.en.md), [Cerebellum](Cerebellum.en.md), [Basal Ganglia](Basal_Ganglia.en.md)
- **AI comparison**: https://jeffliulab.github.io/ai-notes/02_Deep_Learning/03_Computer_Vision/CNN/

---

## References

1. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
2. **Brodmann, K.** *Vergleichende Lokalisationslehre der Großhirnrinde*. 1909.
3. **Mountcastle, V. B.** "Modality and topographic properties of single neurons of cat's somatic sensory cortex." *J Neurophysiol*, 1957.
4. **Sperry, R. W.** "Cerebral Organization and Behavior." *Science*, 1961.
