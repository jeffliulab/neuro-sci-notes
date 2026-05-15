# Working Memory

> *Working memory is the brain's short-term hold + manipulate buffer ($7 \pm 2$ items, Miller 1956). Baddeley 1974 model: central executive + phonological loop + visuospatial sketchpad + episodic buffer. PFC + parietal are the substrate. Key to reasoning, decision, language.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Cortex](../01_Neuroanatomy/Cortex.en.md), [Hippocampus + Memory](../03_Systems_Neuroscience/Hippocampus_Memory.en.md)

---

## 1. Difference from Short-Term Memory

- **Short-term memory**: passive hold (e.g., digit span)
- **Working memory**: active manipulation (rearrange, transform)
- WM = STM + control

---

## 2. Baddeley Model (1974)

```
        Central Executive
       /      |        \
Phonological  Visuospatial  Episodic
Loop          Sketchpad     Buffer (added 2000)
(verbal)      (visual)      (multimodal)
```

- **Phonological loop**: auditory / verbal (~ 2 sec)
- **Visuospatial sketchpad**: visual / spatial
- **Episodic buffer**: interface with long-term memory
- **Central executive**: attention + control

---

## 3. Neural Basis

- **DLPFC**: primary WM region
- **PPC** (parietal): visual / spatial WM
- **Broca's area**: phonological loop (overlap with language)
- **Hippocampus**: long-term storage interface

---

## 4. Persistent Activity

WM neurons show sustained firing during delay:
- Goldman-Rakic 1990s monkey experiments
- DLPFC neurons hold item info for seconds
- Similar to "attractor network" states

---

## 5. Capacity Limits

- Miller 1956: $7 \pm 2$ items (but simplistic; modern: ~ 4)
- Chunking: 1234567 → "1-2-3" "4-5-6" "7" → 3 chunks
- Expert chunking (chess masters) appears to exceed capacity

---

## 6. Decay

- ~ 18 sec without rehearsal (Peterson & Peterson 1959)
- Interference > decay (main cause of forgetting)
- Sleep deprivation severely affects WM

---

## 7. PyTorch — Sustained Activity Model

```python
import torch

class WorkingMemoryNet(torch.nn.Module):
    """Attractor network for WM."""
    def __init__(self, dim=64, n_attractors=4):
        super().__init__()
        # Recurrent weights from Hebbian learning
        self.W = torch.nn.Parameter(torch.zeros(dim, dim))
        self.dim = dim
    
    def forward(self, x, steps=20):
        h = x
        for _ in range(steps):
            h = torch.tanh(self.W @ h + x * 0.1)  # sustained input + recurrence
        return h  # converged attractor
    
    def store(self, pattern):
        """Hebbian: W += outer(p, p) / dim."""
        with torch.no_grad():
            self.W += torch.outer(pattern, pattern) / self.dim
```

---

## 8. Training WM

- N-back task (1-back, 2-back, 3-back...)
- Dual N-back: visual + auditory N-back
- Brain training (transfer is controversial)
- Stimulant medication (ADHD) gives temporary boost

---

## 9. Pathology

- **ADHD**: WM capacity / regulation dysregulated
- **Schizophrenia**: severe WM impairment
- **Alzheimer's**: WM affected early
- **Aging**: WM capacity decline (but expertise compensates)
- **Depression**: rumination occupies WM resources

---

## 10. WM ↔ AI

- **Transformer context window**: ≈ short-term WM (but capacity much larger)
- **RNN hidden state**: approximates sustained activity
- **Attention mechanism**: similar to central executive
- LLM "in-context learning" has functional overlap with human WM

---

## 11. Common Pitfalls

### 11.1 7 ± 2 Misread

Not universal capacity; depends on content + chunking.

### 11.2 STM = WM Error

WM includes active manipulation; STM is passive hold.

### 11.3 Brain Training Transfer

Improving N-back ≠ improving general intelligence (controversial).

### 11.4 WM Capacity ≠ IQ

Correlated but distinct constructs.

### 11.5 LLM Context ≠ Brain WM

LLM capacity 200k+ tokens; brain ~ 4-7. Completely different substrate.

---

## 12. Related Concepts

- **Same section**: [Attention](Attention.en.md), [Decision_Making](Decision_Making.en.md), [Language](Language.en.md)
- **Anatomy**: [Cortex](../01_Neuroanatomy/Cortex.en.md)
- **Systems**: [Hippocampus + Memory](../03_Systems_Neuroscience/Hippocampus_Memory.en.md)
- **AI**: Transformer attention — https://jeffliulab.github.io/ai-notes/02_Deep_Learning/05_Transformers/

---

## References

1. **Baddeley, A.** *Working Memory*. Oxford, 1986.
2. **Miller, G. A.** "The magical number seven." *Psychol Rev*, 1956.
3. **Goldman-Rakic, P. S.** "Cellular basis of working memory." *Neuron*, 1995.
4. **D'Esposito, M. & Postle, B. R.** "The cognitive neuroscience of working memory." *Annu Rev Psychol*, 2015.
