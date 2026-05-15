# Deep Learning vs Brain

> *Goal-driven deep networks have become the best predictive models of sensory cortex (Yamins & DiCarlo 2014: CNN mid-layers ↔ IT firing r > 0.7). But similarity is at the representational level; mechanisms differ entirely (backprop vs local rules; static vs recurrent + feedback). Understanding where they're similar vs different is core to NeuroAI.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Visual System](../03_Systems_Neuroscience/Visual_System.en.md), [Neural Population Dynamics](Neural_Population_Dynamics.en.md), deep learning basics

---

## 1. Goal-Driven Modeling

- Train CNN on ImageNet → mid-layer representations **predict IT/V4 firing**
- Yamins & DiCarlo 2014: performance ↑ → brain predictivity ↑
- No need to fit neural data; task optimization naturally yields brain-like representations
- Brain-Score benchmark quantifies

---

## 2. Representational Similarity Measures

| Method | Idea |
|---|---|
| **RSA** (Repr. Similarity Analysis) | Compare representational dissimilarity matrices |
| **Linear encoding** | Linearly predict neural from model features |
| **CKA** | Centered kernel alignment |
| **Brain-Score** | Integrative benchmark |

---

## 3. Where Similar

- Ventral stream ↔ CNN hierarchy (V1→V4→IT vs conv layers)
- Auditory cortex ↔ audio CNN
- Language areas ↔ LLM representations (Schrimpf 2021: GPT predicts language cortex)
- Grid-like code emerges in RNN (see [Grid Cells](Grid_Cells.en.md))

---

## 4. Where Different (Mechanism)

| Dimension | Brain | DL |
|---|---|---|
| Learning | local rule, no backprop | backprop |
| Structure | heavy recurrent + feedback | mostly feedforward |
| Neuron | spiking, dendritic compute | scalar ReLU |
| Energy | ~ 20 W | GPU kW |
| Data | few-shot | massive |
| Time | continuous dynamics | discrete layers |

---

## 5. PyTorch — RSA

```python
import torch

def rsa(features_a, features_b):
    """Representational Similarity Analysis between two systems."""
    def rdm(X):  # representational dissimilarity matrix
        X = X - X.mean(0)
        sim = X @ X.t()
        d = 1 - sim / (sim.diag().outer(sim.diag()).sqrt() + 1e-8)
        return d
    ra, rb = rdm(features_a), rdm(features_b)
    iu = torch.triu_indices(ra.size(0), ra.size(0), 1)
    va, vb = ra[iu[0], iu[1]], rb[iu[0], iu[1]]
    # Spearman-like correlation of RDMs
    return torch.corrcoef(torch.stack([va, vb]))[0, 1]
```

---

## 6. Backprop's Bio Plausibility

- Problems: weight transport, global error, bidirectional, non-local
- Alternatives: feedback alignment, predictive coding, equilibrium prop, target prop
- See [Synaptic Plasticity Models](Synaptic_Plasticity_Models.en.md)
- Lillicrap 2020: brain may approximate backprop (debated)

---

## 7. Bidirectional Inspiration History

- CNN ← Hubel-Wiesel V1 (1959), Neocognitron (Fukushima 1980)
- RL ← dopamine RPE (see [RL Brain](Reinforcement_Learning_Brain.en.md))
- Attention ← visual attention
- Hopfield/Boltzmann ← statistical physics + neuro (2024 Nobel)
- Feedback: CNN → understand IT; RNN → understand PFC dynamics

---

## 8. NeuroAI Agenda

- Use DL as image-computable models of the brain
- Use neuroscience to improve AI (robustness, efficiency, continual learning)
- "Embodied Turing test" (Zador 2023)
- Foundation models of neural data

---

## 9. Caveats

- High predictivity ≠ same mechanism (possible degenerate solutions)
- Untrained networks also partially predict (architectural prior)
- Benchmark gaming risk
- Similarity metric choice sensitive

---

## 10. Common Pitfalls

### 10.1 CNN = Visual Cortex

Only approximate; lacks recurrence, feedback, attention, adaptation.

### 10.2 High r → Same Algorithm

Representational similarity ≠ same mechanism.

### 10.3 Backprop Impossible in Brain

May have approximations; unresolved (active research).

### 10.4 LLM = Language Brain

Some representational similarity; but grounding, development, energy vastly differ.

### 10.5 NeuroAI = Fitting DL to Brain

Bidirectional: also use neuro principles to improve AI.

---

## 11. Related Concepts

- **Same section**: [Neural Population Dynamics](Neural_Population_Dynamics.en.md), [Synaptic Plasticity Models](Synaptic_Plasticity_Models.en.md), [Grid Cells](Grid_Cells.en.md)
- **Systems**: [Visual System](../03_Systems_Neuroscience/Visual_System.en.md)
- **Foundation**: [Levels of Analysis](../00_Foundations/Levels_of_Analysis.en.md)
- **AI**: CNN/Transformer — https://jeffliulab.github.io/ai-notes/02_Deep_Learning/

---

## References

1. **Yamins, D. L. K. & DiCarlo, J. J.** "Using goal-driven deep learning models to understand sensory cortex." *Nat Neurosci*, 2016.
2. **Schrimpf, M. et al.** "The neural architecture of language: Integrative modeling converges on predictive processing." *PNAS*, 2021.
3. **Lillicrap, T. P. et al.** "Backpropagation and the brain." *Nat Rev Neurosci*, 2020.
4. **Zador, A. et al.** "Catalyzing next-generation artificial intelligence through NeuroAI." *Nat Commun*, 2023.
