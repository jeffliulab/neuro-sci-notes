# Social Cognition

> *Social cognition is the brain's ability to understand others' mental states: Theory of Mind (ToM), empathy, face recognition, social hierarchy. Mirror neurons (1992 Rizzolatti) + mPFC + TPJ + STS are key. Autism strongly linked with social cognition deficits. Key human-vs-LLM differentiator.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Cortex](../01_Neuroanatomy/Cortex.en.md), [Emotion](Emotion.en.md)

---

## 1. Core Concepts

- **Theory of Mind (ToM)**: inferring others' belief/desire/intention
- **Empathy**: affective + cognitive
- **Mentalizing**: actively model others' mental state
- **Joint attention**: shared attention focus
- **Face perception**: FFA etc. recognize identity + expression

---

## 2. Neural Basis

```
       mPFC (mentalizing)
      /
TPJ ─── inferring others' intent
      \
       STS (action observation)

PCC + precuneus (self vs other)
Amygdala (emotion + threat detection)
Insula (interoception, empathy)
Mirror neurons (premotor + parietal)
```

---

## 3. Mirror Neuron System

- Rizzolatti 1992 in macaque F5
- Fire when monkey performs action **AND** sees others perform same
- Human inferior frontal gyrus + inferior parietal lobule have mirror system
- Hypothesized: imitation, language evolution, empathy basis (still debated)

---

## 4. Theory of Mind Tests

- **False Belief Task** (Sally-Anne, Wimmer & Perner 1983):
  - Sally puts ball in basket, leaves
  - Anne moves ball to box
  - Ask child: where will Sally look when she returns?
  - Before 4: "box" (can't model others' belief)
  - 4-5 yo: develops ToM, says "basket"
- LLM (GPT-4) passes ToM tests, but whether truly has ToM is debated

---

## 5. Two Empathy Types

- **Affective empathy**: feel others' emotion (insula, ACC)
- **Cognitive empathy**: understand others' perspective (mPFC, TPJ)
- Psychopathy: affective ↓, cognitive normal or ↑

---

## 6. Pathology

- **Autism Spectrum Disorder (ASD)**: ToM, joint attention deficits
- **Psychopathy**: missing affective empathy, intact cognitive
- **Frontotemporal Dementia (FTD)**: social cognition decline
- **Williams Syndrome**: hyper-social, but other cognitive deficits

---

## 7. PyTorch — Simplified ToM Model

```python
import torch

class TheoryOfMindModel(torch.nn.Module):
    """Predict another agent's belief based on observation."""
    def __init__(self, state_dim=10, hidden=64):
        super().__init__()
        self.observer = torch.nn.GRU(state_dim, hidden, batch_first=True)
        self.belief_head = torch.nn.Linear(hidden, state_dim)
    
    def forward(self, other_observations):
        """Given what 'other' saw, infer their belief."""
        h, _ = self.observer(other_observations)
        belief = self.belief_head(h[:, -1])
        return belief
```

---

## 8. Evolutionary View

- Dunbar (1992): primate neocortex size ∝ social group size
- "Social brain hypothesis": large brains evolved for social complexity
- Humans ~ 150 (Dunbar number) stable social relationships

---

## 9. AI / LLM ↔ ToM

- GPT-4 passes Sally-Anne and other ToM benchmarks
- But controversy: is LLM doing statistical pattern matching or true mental state modeling?
- Multi-agent reasoning still LLM weakness

---

## 10. Common Pitfalls

### 10.1 Mirror Neuron Mystique

Not "empathy cells"; functions far more complex.

### 10.2 ToM ≠ Empathy

ToM is cognitive ability; empathy involves feeling.

### 10.3 Autism Doesn't Lack Empathy

ASD is social communication challenge; empathy may be normal or atypical.

### 10.4 LLM ToM Test

Passing ≠ true understanding; may exploit training-data patterns.

### 10.5 Mentalizing Network Beyond Social

Also engaged in self-reflection, autobiographical memory.

---

## 11. Related Concepts

- **Same section**: [Emotion](Emotion.en.md), [Consciousness](Consciousness.en.md)
- **Anatomy**: [Cortex](../01_Neuroanatomy/Cortex.en.md)
- **Disorder**: Autism

---

## References

1. **Premack & Woodruff** "Does the chimpanzee have a theory of mind?" *BBS*, 1978.
2. **Rizzolatti, G. et al.** "Premotor cortex and the recognition of motor actions." *Cogn Brain Res*, 1996.
3. **Frith, U. & Frith, C. D.** "Mechanisms of social cognition." *Annu Rev Psychol*, 2012.
4. **Dunbar, R. I. M.** "The social brain hypothesis." *Evol Anthropol*, 1998.
