# Memory Systems Taxonomy

> *Memory is not unitary: multiple dissociable systems. Declarative (hippocampus-dependent: episodic + semantic) vs non-declarative (procedural, priming, conditioning, habit). Squire's taxonomy + patient H.M. (lost episodic, preserved procedural) are foundational evidence. Understanding the taxonomy is a framework for clinical use + AI continual learning.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Hippocampus_Memory](../03_Systems_Neuroscience/Hippocampus_Memory.en.md), [Working Memory](Working_Memory.en.md)

---

## 1. Squire's Taxonomy

```
Memory
├── Declarative (explicit, conscious, hippocampus-dependent)
│   ├── Episodic (events + spatiotemporal context)
│   └── Semantic (facts / knowledge)
└── Non-declarative (implicit, unconscious)
    ├── Procedural (skills, striatum/cerebellum)
    ├── Priming (cortex)
    ├── Classical conditioning (amygdala/cerebellum)
    └── Non-associative (habituation/sensitization)
```

---

## 2. Temporal Dimension

| Type | Duration | Substrate |
|---|---|---|
| Sensory memory | < 1 sec | Sensory cortex |
| Working memory | sec | PFC (see [Working Memory](Working_Memory.en.md)) |
| Short-term | min | Early hippocampus |
| Long-term | days-lifetime | Cortex (consolidation) |

---

## 3. Patient Evidence (Double Dissociation)

- **H.M.** (bilateral MTL resection): can't form new episodic, but procedural (mirror drawing) normal → declarative ≠ procedural
- **Amnesia**: episodic impaired, semantic partly preserved
- **Parkinson/HD** (striatum): procedural impaired, declarative relatively preserved
- Double dissociation = strong evidence for independent systems

---

## 4. Episodic vs Semantic

| | Episodic | Semantic |
|---|---|---|
| Content | "What I did yesterday" | "Paris is France's capital" |
| Context | Spatiotemporal self | Decontextualized |
| Subjective | autonoetic (mental time travel) | noetic |
| Hippocampus-dependent | Strong (esp. recent) | Weaker after consolidation |
| Tulving proposed | ✓ | ✓ |

---

## 5. Consolidation

- **Synaptic consolidation** (hours): LTP → synaptic stabilization
- **Systems consolidation** (months-years): hippocampus → neocortex transfer
- Standard model vs Multiple Trace Theory (whether remote episodic still needs hippocampus, debated)
- Sleep + replay critical (see [Hippocampus_Memory](../03_Systems_Neuroscience/Hippocampus_Memory.en.md))

---

## 6. PyTorch — Complementary Learning Systems (CLS)

```python
import torch

class ComplementaryLearningSystems(torch.nn.Module):
    """McClelland 1995: fast hippocampus + slow neocortex."""
    def __init__(self, dim=64):
        super().__init__()
        self.hpc = torch.nn.Linear(dim, dim)   # fast, sparse, episodic
        self.ctx = torch.nn.Linear(dim, dim)   # slow, distributed, semantic
    def forward(self, x, mode='recall'):
        if mode == 'encode':
            return torch.relu(self.hpc(x))     # rapid one-shot
        return torch.tanh(self.ctx(x))         # consolidated, generalized
```

→ Explains why dual systems needed: avoid catastrophic interference (same AI continual learning problem).

---

## 7. Relation to AI Continual Learning

- **Catastrophic forgetting**: NN learning new overwrites old (McCloskey 1989)
- Brain uses CLS (fast hippocampus + slow cortex + replay) to avoid
- AI borrows: experience replay (DQN), EWC, generative replay
- Hippocampal replay ↔ DQN replay buffer (direct inspiration)

---

## 8. Amnesia Types

- **Anterograde**: can't form new memories (H.M.)
- **Retrograde**: lose old memories (often temporal gradient, Ribot's law)
- **Transient global amnesia**: brief
- **Psychogenic**: no organic damage (debated)
- **Confabulation**: fabricated filling (Korsakoff)

---

## 9. Distinction from Working Memory

- WM: online maintenance + manipulation (seconds, PFC)
- Long-term memory: storage (hippocampus → cortex)
- Different systems; WM impairment ≠ LTM impairment (double dissociation)

---

## 10. Common Pitfalls

### 10.1 Memory Is a Single Store

Multiple dissociable systems (declarative vs procedural double dissociation).

### 10.2 Memory Like Video Replay

Reconstructive, prone to schema / suggestion distortion (false memory).

### 10.3 Hippocampus Stores All Long-Term Memory

Systems consolidation → remote memory moves to cortex (details debated).

### 10.4 Episodic = Semantic

Tulving distinction: autonoetic vs noetic; separable impairment.

### 10.5 Forgetting = Storage Loss

Mostly retrieval failure / interference, not full deletion.

---

## 11. Related Concepts

- **Same section**: [Working Memory](Working_Memory.en.md), [Executive Function](Executive_Function.en.md)
- **Systems**: [Hippocampus_Memory](../03_Systems_Neuroscience/Hippocampus_Memory.en.md), [Sleep_Wake](../03_Systems_Neuroscience/Sleep_Wake.en.md)
- **Cellular**: [LTP_LTD](../02_Cellular_Molecular/LTP_LTD.en.md)
- **AI**: continual learning, experience replay

---

## References

1. **Squire, L. R.** "Memory systems of the brain: A brief history and current perspective." *Neurobiol Learn Mem*, 2004.
2. **Tulving, E.** "Episodic and semantic memory." 1972.
3. **Scoville, W. B. & Milner, B.** "Loss of recent memory after bilateral hippocampal lesions (H.M.)." *J Neurol Neurosurg Psychiatry*, 1957.
4. **McClelland, J. L. et al.** "Why there are complementary learning systems in the hippocampus and neocortex." *Psychol Rev*, 1995.
