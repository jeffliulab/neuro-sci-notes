# Olfactory System

> *Olfaction is the only sense that bypasses thalamus to reach cortex directly. Buck & Axel 1991 discovered ~ 350 odorant receptors (2004 Nobel). Most ancient sense, deeply linked with emotion + memory (Proust effect). Olfactory bulb → piriform → entorhinal → hippocampus. AI olfaction (electronic nose) attempts to simulate.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Cortex](../01_Neuroanatomy/Cortex.en.md), [Hippocampus Anatomy](../01_Neuroanatomy/Hippocampus_Anatomy.en.md)

---

## 1. Receptors + First Level

- **Olfactory epithelium**: nasal roof, ~ 5 cm²
- **Olfactory receptor neurons (ORN)**: ~ 6 million, 3-4 week lifespan with regeneration
- Each ORN expresses 1 odorant receptor (1OR-1ORN rule)
- 350 OR genes (Buck & Axel) → combinatorial coding
- Cilia + mucus capture odorant molecules

---

## 2. Pathway

```
Odorant molecules (air)
  ↓
Olfactory epithelium (ORN cilia)
  ↓ (cribriform plate)
Olfactory bulb (glomeruli)
  ↓
  ├──→ Piriform cortex (primary)
  ├──→ Amygdala
  ├──→ Entorhinal → Hippocampus
  └──→ OFC (secondary)
```

→ Direct to cortex, not via thalamus (other senses go through thalamus).

---

## 3. Olfactory Bulb

- **Glomeruli** ~ 2000, each glomerulus collects ORN axons from same OR
- Convergence: thousands of ORN → 1 glomerulus
- Mitral / tufted cells = output
- Granule cells = inhibitory interneurons → lateral inhibition

---

## 4. Combinatorial Coding

- 1 odorant activates many OR (non-specific)
- 1 OR responds to many odorants
- $350 \times 350 \times ... \times 350$ combinatorial → trillions of distinguishable odors
- Like NN distributed representation

---

## 5. Piriform Cortex

- Paleocortex (3 layers, not 6)
- Unlike V1 retinotopy, piriform has no odotopy (mixed input)
- Like a flat NN doing odor classification
- Tightly linked with hippocampus + amygdala → olfactory memory + emotion

---

## 6. Proust Effect

- Proust's madeleine: smell → strong episodic memory
- Explanation: olfaction enters amygdala + hippocampus directly, no thalamus gate
- Among modalities: olfactory memory most robust long-term

---

## 7. PyTorch — Olfactory Combinatorial Code

```python
import torch
import torch.nn as nn

class OlfactoryEncoder(nn.Module):
    """350 receptors → odor identity classifier."""
    def __init__(self, n_receptors=350, n_odors=1000):
        super().__init__()
        self.receptor_affinity = nn.Parameter(torch.randn(n_receptors, n_odors))
    
    def forward(self, odor_concentration):
        """odor_concentration: (B, n_odors) — composition vector."""
        response = self.receptor_affinity @ odor_concentration.t()  # (n_receptors, B)
        response = torch.sigmoid(response)  # saturation
        return response  # neural code
```

---

## 8. Human vs Animal

- **Human**: ~ 350 functional OR genes
- **Dog**: ~ 800
- **Mouse**: ~ 1000
- But absolute olfactory acuity in humans is stronger than legend (McGann 2017)

---

## 9. Pathology

- **Anosmia**: loss of smell
  - COVID-19 key symptom
  - Parkinson early marker (damages ORN / bulb)
  - Alzheimer early
- **Parosmia**: distorted smell (post-infection, chemo)
- **Phantosmia**: phantom smell (temporal seizure)
- **Kallmann syndrome**: olfactory + sex development co-disorder

---

## 10. AI Olfaction

- **Electronic nose**: gas sensor array → ML classification
- **Aryballe**: commercial e-nose
- **Osmo (Google AI)**: deep learning + smell → description
- **Princeton 2024**: GNN predicts odor from molecular structure
- Olfaction is hardest AI modality to replicate

---

## 11. Pheromones

- **Vomeronasal organ (VNO)**: rodents have a second olfactory system
- Human VNO is vestigial, but MHC olfaction still influences mate choice
- Pheromone products (Axe) are marketing

---

## 12. Common Pitfalls

### 12.1 Human Olfaction Weak

McGann 2017: human olfaction not behind dog in many aspects.

### 12.2 1 Odor = 1 Receptor

Wrong; combinatorial coding.

### 12.3 Bulb No Plasticity

Actually: olfactory bulb is the only brain region with ongoing adult neurogenesis.

### 12.4 Anosmia Not Severe

Actually: depression risk ↑, food safety ↓, social difficulty.

### 12.5 Olfaction = Smell Only

Also includes flavor (retronasal olfaction during eating).

---

## 13. Related Concepts

- **Same section**: [Visual System](Visual_System.en.md), [Auditory System](Auditory_System.en.md), [Somatosensory](Somatosensory.en.md)
- **Anatomy**: [Hippocampus Anatomy](../01_Neuroanatomy/Hippocampus_Anatomy.en.md), [Amygdala](../01_Neuroanatomy/Amygdala.en.md)
- **Disease**: Alzheimer early anosmia

---

## References

1. **Buck, L. & Axel, R.** "A novel multigene family may encode odorant receptors." *Cell*, 1991.
2. **Mori, K. & Yoshihara, Y.** "Molecular recognition and olfactory processing." *Prog Neurobiol*, 1995.
3. **McGann, J. P.** "Poor human olfaction is a 19th-century myth." *Science*, 2017.
4. **Lee, B. K. et al.** "A principal odor map unifies diverse tasks in olfactory perception." *Science*, 2023.
