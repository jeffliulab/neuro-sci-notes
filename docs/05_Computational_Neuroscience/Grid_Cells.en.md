# Grid Cells — Neural Spatial Map

> *Grid cells are entorhinal cortex (EC) neurons that fire when an animal is at vertices of a hexagonal grid. Discovered by Moser & Moser 2005 (Nobel 2014). They cooperate with hippocampal place cells → spatial map. Grids provide metric (distance / direction); place cells provide episodic memory. Banino 2018 showed RNNs naturally emerge grid-like patterns, deepening AI-brain link.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Hippocampus Anatomy](../01_Neuroanatomy/Hippocampus_Anatomy.en.md), [Hippocampus Memory](../03_Systems_Neuroscience/Hippocampus_Memory.en.md)

---

## 1. Discovery

- O'Keefe 1971: hippocampus place cells
- Moser & Moser 2005: medial entorhinal cortex (MEC) grid cells
- 2014 Nobel Prize in Physiology
- Same lab found head direction cells, border cells, speed cells

---

## 2. Grid Pattern

```
Rat in 1 m × 1 m arena, one MEC neuron firing:

. . X . . X . . X .
. X . . X . . X . .
X . . X . . X . . X
. . X . . X . . X .
. X . . X . . X . .

(60° spaced equilateral triangle grid)
```

- Period ~ 30 cm-1 m
- Multiple spatial frequencies (MEC dorsal-ventral increase)
- Same module: same frequency, different phase

---

## 3. Neural Basis

- Layer II MEC stellate cells
- Continuous attractor network (CAN) model
- Inhibitory ring + path integration
- Border cells correct drift

---

## 4. Modular Organization

- **Modules** ~ 5-10
- Within module: same period; ~ √2 ratio between modules
- Like digit code (high / low bits)
- Theoretically can encode large space

---

## 5. Path Integration

- Use self-motion signal (velocity + direction) to update position
- Grid cells maintain pattern without visual cue (dark room)
- Same as dead reckoning

---

## 6. PyTorch — Grid-like Pattern from RNN

```python
import torch
import torch.nn as nn

class PathIntegrationRNN(nn.Module):
    """Banino 2018-like setup."""
    def __init__(self, hidden=128, n_grid=512):
        super().__init__()
        self.rnn = nn.LSTM(input_size=2, hidden_size=hidden, batch_first=True)
        self.grid_head = nn.Linear(hidden, n_grid)
        self.pos_head = nn.Linear(hidden, 2)
    
    def forward(self, velocities):
        # velocities: (B, T, 2)
        h, _ = self.rnn(velocities)
        positions = self.pos_head(h)
        grid_act = self.grid_head(h)
        return positions, grid_act
```

Training this RNN to predict position → hidden units naturally show grid-like firing!

---

## 7. Grid ↔ Place Relationship

- MEC grid → Hippocampus CA3 place
- Multiple grid modules combined → unique place (Fourier-basis-like)
- Reverse: place cell can generate / replay sequences

---

## 8. Abstract Grid Uses

Grids aren't limited to space:
- **Concept space**: 2D task space (Constantinescu 2016)
- **Time**: temporal grid cells
- **Social space**: friend count + status?
- LLM internal grid-like representation? (under exploration)

---

## 9. Pathology

- **Alzheimer's**: EC affected early → spatial disorientation early symptom
- **Aging**: grid drift
- **3xTg-AD mouse**: grid modules degrade

---

## 10. AI / Banino 2018

- DeepMind RNN trained to do path integration
- Hidden units emerge hexagonal grid pattern
- Like the brain
- But follow-up shows architecture choice matters; not all RNNs

---

## 11. Common Pitfalls

### 11.1 Grid = Unique Position

No; one grid period maps to many positions → multi-module decode.

### 11.2 Grid Only Spatial

No; abstract concept space also shows grids.

### 11.3 Path Integration Perfect

In practice drifts; needs visual / border cues.

### 11.4 RNN Training Auto-emerges Grid

Architecture matters; not any RNN.

### 11.5 Grid = Place

Different brain regions, different properties (periodic vs unimodal).

---

## 12. Related Concepts

- **Same section**: [Hopfield Networks](Hopfield_Networks.en.md), [Predictive Coding](Predictive_Coding.en.md)
- **Anatomy**: [Hippocampus Anatomy](../01_Neuroanatomy/Hippocampus_Anatomy.en.md)
- **Systems**: [Hippocampus Memory](../03_Systems_Neuroscience/Hippocampus_Memory.en.md)
- **AI**: RNN spatial reasoning

---

## References

1. **Hafting, T. et al.** "Microstructure of a spatial map in the entorhinal cortex." *Nature*, 2005.
2. **Moser, E. I. et al.** "Grid cells and cortical representation." *Nat Rev Neurosci*, 2014.
3. **Banino, A. et al.** "Vector-based navigation using grid-like representations in artificial agents." *Nature*, 2018.
4. **Constantinescu, A. O., O'Reilly, J. X., Behrens, T. E. J.** "Organizing conceptual knowledge in humans with a gridlike code." *Science*, 2016.
