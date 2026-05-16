# Cognitive Maps

> *A cognitive map is the brain's internal structured representation of space (and abstract relations). Tolman 1948 proposed it behaviorally → O'Keefe place cells + Moser grid cells confirmed physiologically (see [Grid Cells](../05_Computational_Neuroscience/Grid_Cells.en.md)). The hippocampal-entorhinal system encodes not just space but generalizes to conceptual/social "spaces" (Behrens 2018). A unified framework for navigation + relational reasoning + memory.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Memory_Systems](Memory_Systems.en.md), [Grid Cells](../05_Computational_Neuroscience/Grid_Cells.en.md)

---

## 1. Origin — Tolman (1948)

- Behaviorist era: rats in mazes not pure S-R chains
- "Latent learning" + shortcuts → internal **map**
- Anti pure stimulus-response, proposed cognitive representation
- Confirmed by neurophysiology decades later

---

## 2. Spatial Cell Family

| Cell | Encodes | Region |
|---|---|---|
| Place cell | Specific location | Hippocampus (O'Keefe 1971) |
| Grid cell | Hexagonal grid | MEC (Moser 2005) |
| Head direction | Heading | presubiculum etc. |
| Border/boundary | Boundaries | subiculum/MEC |
| Speed cell | Movement speed | MEC |
| Object-vector | Relative object bearing | MEC |

→ Together form metric + topological map (see [Grid Cells](../05_Computational_Neuroscience/Grid_Cells.en.md)).

---

## 3. Map Types

- **Egocentric**: relative to body (parietal)
- **Allocentric**: world coordinates (hippocampus)
- Navigation needs both + conversion (retrosplenial cortex key)
- Path integration + landmark correction

---

## 4. PyTorch — Place from Grid Combination

```python
import torch

def place_from_grids(pos, grid_scales, grid_phases):
    """Multiple grid modules (Fourier-like basis) -> unique place code."""
    code = []
    for scale, phase in zip(grid_scales, grid_phases):
        # Hexagonal grid firing as sum of 3 cosines
        g = sum(torch.cos(2*torch.pi/scale * (pos @ d) + phase)
                for d in torch.tensor([[1.,0.],[0.5,0.87],[-0.5,0.87]]))
        code.append(torch.relu(g))
    return torch.stack(code)   # combined -> place-cell-like uniqueness
```

---

## 5. Beyond Space — Conceptual Maps

- Same hippocampal-entorhinal mechanism encodes **abstract relational space**
- Constantinescu 2016: grid-like signal in concept learning (fMRI)
- "Cognitive map of knowledge" (Behrens 2018): social, value, task structure
- Unified: hippocampus = relational memory + reasoning + generalization engine

---

## 6. Relation to Memory / Reasoning

- Cognitive maps support **inferring unexperienced relations** (transitive inference, shortcuts)
- "Schema" + rapid integration (Tse 2007)
- Imagination / planning = "simulating trajectories" on the map (see [Mental_Imagery](Mental_Imagery.en.md))
- Tightly linked to [Memory_Systems](Memory_Systems.en.md) episodic

---

## 7. Clinical

- **Alzheimer's**: entorhinal / hippocampus early damage → spatial disorientation early symptom
- **Hippocampal damage** (e.g., H.M.): spatial + relational memory deficits
- **Topographical disorientation**: RSC / parahippocampal damage
- **Aging**: grid drift → navigation decline

---

## 8. Relation to AI

- RNN trained to navigate → emerges grid-like (Banino 2018, see [Grid Cells](../05_Computational_Neuroscience/Grid_Cells.en.md))
- "World model" ↔ cognitive map (relational structure representation)
- Eichenbaum: relational memory ↔ graph / GNN
- Transformer positional encoding ↔ grid-like basis (analogy research)

---

## 9. Computational View

- Metric (grid) + topological (graph) dual
- Successor representation (Stachenfeld 2017): place encodes predictive successor → RL bridge
- Cognitive map = generalizable relational/predictive structure (not pure geometry)

---

## 10. Common Pitfalls

### 10.1 Cognitive Map = Literal Map

Structured relational representation, includes topology + prediction, not a scale map.

### 10.2 Only Encodes Physical Space

Same mechanism generalizes to conceptual / social / value spaces.

### 10.3 Place Cell = GPS Coordinate

Context-dependent, remapping, predictive (SR).

### 10.4 Tolman Refuted by Behaviorism

Later strongly confirmed by place/grid cells.

### 10.5 Grid = Place

Different regions + properties; grid combination → place (many-to-one basis).

---

## 11. Related Concepts

- **Same section**: [Memory_Systems](Memory_Systems.en.md), [Mental_Imagery](Mental_Imagery.en.md), [Reasoning_Problem_Solving](Reasoning_Problem_Solving.en.md)
- **Computational**: [Grid Cells](../05_Computational_Neuroscience/Grid_Cells.en.md), [Reinforcement Learning Brain](../05_Computational_Neuroscience/Reinforcement_Learning_Brain.en.md)
- **Systems**: [Hippocampus_Memory](../03_Systems_Neuroscience/Hippocampus_Memory.en.md)
- **AI**: world model, GNN, relational reasoning

---

## References

1. **Tolman, E. C.** "Cognitive maps in rats and men." *Psychol Rev*, 1948.
2. **O'Keefe, J. & Nadel, L.** *The Hippocampus as a Cognitive Map*. 1978.
3. **Behrens, T. E. J. et al.** "What is a cognitive map? Organizing knowledge for flexible behavior." *Neuron*, 2018.
4. **Stachenfeld, K. L. et al.** "The hippocampus as a predictive map." *Nat Neurosci*, 2017.
