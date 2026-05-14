# Hippocampus and Memory Systems

> *The hippocampus is core to declarative memory (episodic + semantic). After patient H.M.'s 1957 hippocampal resection lost the ability to form new long-term memories, the memory systems neuroscience field began. This article covers: multi-memory systems, hippocampal consolidation, replay, relation to AI.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Hippocampus Anatomy](../01_Neuroanatomy/Hippocampus_Anatomy.en.md), [LTP/LTD](../02_Cellular_Molecular/LTP_LTD.en.md)

---

## 1. Memory System Classification

```
Memory
├── Declarative (explicit) — hippocampus + medial temporal lobe
│   ├── Episodic (personal experience)
│   └── Semantic (conceptual knowledge)
└── Non-declarative (implicit) — other regions
    ├── Procedural (skills) — BG + cerebellum
    ├── Priming — neocortex
    └── Conditioning — amygdala / cerebellum
```

H.M. lost declarative but preserved procedural / motor learning → multi-system evidence.

---

## 2. Hippocampus in Episodic Memory

### 2.1 Encoding

Event → cortex multimodal input → EC → DG → CA3 → CA1 → output.
LTP occurs simultaneously at multiple synapses → forms episode trace.

### 2.2 Storage

CA3 recurrent → auto-associative memory:
- Pattern separation (DG): distinguish similar episodes
- Pattern completion (CA3): partial cue → full episode

### 2.3 Retrieval

Cue → partial EC input → CA3 reactivate → CA1 → cortex reconstruction.

---

## 3. Consolidation

### 3.1 Time Dimensions

- **Short-term**: sec-min, hippocampus active
- **Long-term**: months-years, **gradually transferred to cortex**
- **Remote memory**: years later, hippocampus may no longer needed

### 3.2 Standard Consolidation Theory

Squire 1990s: hippocampus is temporary storage, info gradually transfers to cortex.

### 3.3 Multiple Trace Theory

Nadel & Moscovitch: certain episodic memories always need hippocampus (especially vivid details).

### 3.4 Sleep Replay

REM + slow-wave sleep:
- Place cell sequences replayed during sleep (Wilson & McNaughton 1994)
- Same sequences as daytime behavior, accelerates consolidation
- Biological basis of "AI replay buffer"

---

## 4. Memory Engrams

Tonegawa 2015+: use optogenetics to label + activate specific memories:
- Fear conditioning → label engram cells in DG
- Later light activation of same engram → reenact fear behavior (no stimulus)
- Proves memory has **specific cellular substrate**

---

## 5. Spatial Memory

### 5.1 Place Cells (CA1/CA3)

- Fire at specific spatial locations
- O'Keefe 1971
- A mouse running a maze, ~50% of hippocampal pyramidals are place cells

### 5.2 Grid Cells (EC)

- Moser 2005
- Hexagonal grid pattern
- Provides metric for path integration

### 5.3 London Taxi Driver

Maguire 2000: posterior hippocampus enlarged → training shapes hippocampus.

---

## 6. Hippocampus vs AI Memory

### 6.1 Episodic Memory in AI

- DNC (Differentiable Neural Computer, Graves 2016): explicit memory bank
- Memory-augmented Transformer
- RAG (Retrieval-Augmented Generation): LLM + external KB

### 6.2 Replay Buffer (RL)

- DQN: experience replay
- Analog to hippocampal sleep replay

### 6.3 In-Context Learning

- LLM short-term context ≈ hippocampus + working memory
- Long context (Claude 200k+) mimics "long-term"

---

## 7. PyTorch — Episodic Memory + Pattern Completion

```python
import torch

class EpisodicMemory:
    """Auto-associative memory like CA3."""
    def __init__(self, dim=128):
        self.dim = dim
        self.memories = []
    
    def store(self, episode):
        self.memories.append(episode.clone())
    
    def recall(self, cue, mask=None):
        if mask is not None:
            distances = [(m - cue)[mask].norm() for m in self.memories]
        else:
            distances = [(m - cue).norm() for m in self.memories]
        best_idx = torch.argmin(torch.tensor(distances))
        return self.memories[best_idx]

mem = EpisodicMemory()
mem.store(torch.tensor([1., 1, 0, 1, 0, 1]))
mem.store(torch.tensor([0., 1, 1, 0, 1, 1]))
cue = torch.tensor([1., 1, 0, 0, 0, 0])
mask = torch.tensor([True, True, True, False, False, False])
recalled = mem.recall(cue, mask)
```

---

## 8. Pathology

- **H.M.**: bilateral hippocampectomy → complete anterograde amnesia
- **Alzheimer**: hippocampus is early damage area → memory loss
- **Stress / PTSD**: cortisol damages hippocampus, reduces volume
- **Aging**: hippocampal function decline + reduced neurogenesis
- **Temporal lobe epilepsy**: hippocampal sclerosis common

---

## 9. Treatment / Enhancement

- **Memantine** (NMDA modulator) — Alzheimer
- **DBS on entorhinal** (experimental): improves memory in elderly
- **Cognitive training**: doesn't change hippocampal volume but improves connectivity
- **Physical exercise**: increases hippocampal neurogenesis

---

## 10. Common Pitfalls

### 10.1 Hippocampus ≠ All Memory

Also involves amygdala (emotional), cerebellum (motor), BG (habit).

### 10.2 Place Cell ≠ Pure Spatial

Also encodes time, social relations.

### 10.3 Replay ≠ Exact Replay

Sleep replay is compressed + shuffled.

### 10.4 H.M. Clinically Specific

Can't generalize to all amnesia.

### 10.5 fMRI Hippocampal Signal

Easily contaminated by cortex; needs careful ROI.

---

## 11. Related Concepts

- **Anatomy**: [Hippocampus Anatomy](../01_Neuroanatomy/Hippocampus_Anatomy.en.md)
- **Cellular**: [LTP/LTD](../02_Cellular_Molecular/LTP_LTD.en.md)
- **AI comparison**: [RAG](https://jeffliulab.github.io/ai-notes/06_AI_Engineering/06_RAG/), Memory-augmented networks

---

## References

1. **Scoville & Milner.** "Loss of recent memory after bilateral hippocampal lesions." 1957.
2. **Squire, L. R.** "Memory systems of the brain." *Neurobiol Learn Mem*, 2004.
3. **Wilson, M. A. & McNaughton, B. L.** "Reactivation of hippocampal ensemble memories during sleep." *Science*, 1994.
4. **Tonegawa, S. et al.** "Memory engram cells." *Cell*, 2015.
5. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
