# Executive Function

> *Executive function is the umbrella for higher cognitive control: inhibition, working memory updating, cognitive flexibility (shifting) — Miyake's three factors. PFC (esp. dlPFC, ACC) is the substrate. Core to goal-directed behavior, planning, self-regulation. ADHD, FTD, aging impair it. A key human-vs-LLM differentiation dimension.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Working Memory](Working_Memory.en.md), [Cortex](../01_Neuroanatomy/Cortex.en.md)

---

## 1. Miyake's Three-Factor Model (2000)

| Component | Function | Task |
|---|---|---|
| **Inhibition** | Suppress prepotent/irrelevant responses | Stroop, Go/NoGo, Flanker |
| **Updating** | Working memory refresh | N-back, keep-track |
| **Shifting** | Task/rule switching | Task-switching, WCST |

→ "Unity and diversity": correlated but separable.

---

## 2. Neural Basis

- **dlPFC**: working memory, rule maintenance, shifting
- **vlPFC / IFG**: inhibitory control
- **ACC**: conflict monitoring, error detection
- **Parietal**: with PFC forms fronto-parietal control network
- **Basal ganglia**: gating updates

---

## 3. Classic Tasks

- **Stroop**: name ink color vs read word (inhibition)
- **Wisconsin Card Sorting (WCST)**: rule inference + switching (classic PFC lesion deficit)
- **Tower of London/Hanoi**: planning
- **N-back**: updating
- **Trail Making B**: shifting
- **Iowa Gambling**: decision (vmPFC)

---

## 4. PyTorch — Conflict Monitoring + Inhibition (Stroop-like)

```python
import torch

def stroop_conflict(word_signal, color_signal, control_gain=2.0):
    """ACC detects conflict; PFC control biases toward task-relevant."""
    conflict = (word_signal * color_signal).abs()        # co-activation
    # PFC top-down boosts color (task) and suppresses word (prepotent)
    response = control_gain * color_signal - 0.5 * word_signal
    rt = 1.0 + conflict                                  # RT ↑ with conflict
    return torch.sigmoid(response), rt
```

---

## 5. Development and Aging

- **Development**: EF matures with PFC to ~ 25 years (see [Neurodevelopment](../00_Foundations/Neurodevelopment.en.md))
- Early EF predicts later academics / health / income (Moffitt 2011 longitudinal)
- **Aging**: EF declines early (esp. shifting / updating); crystallized preserved
- One of the most age-sensitive cognitive domains

---

## 6. Relation to Working Memory

- WM is the core substrate of EF (updating ∝ WM)
- But EF ⊃ WM: includes inhibition, shifting, planning
- Central executive (Baddeley) ≈ EF (see [Working Memory](Working_Memory.en.md))

---

## 7. Pathology

- **ADHD**: EF deficit core (esp. inhibition, Barkley) — see [ADHD](../08_Neuro_Disorders/ADHD.en.md)
- **bvFTD**: disinhibition + planning loss (see [Frontotemporal Dementia](../08_Neuro_Disorders/Frontotemporal_Dementia.en.md))
- **PFC lesion**: dysexecutive syndrome, Phineas Gage
- **Schizophrenia / depression**: EF impaired
- **TBI**: often affects PFC → EF

---

## 8. Measurement + Limitations

- "Task impurity problem": each task mixes non-EF components
- Ecological validity: lab tasks ≠ real-life EF
- Factor structure changes with age (more "unitary" in childhood)
- Latent variable modeling (SEM) mitigates

---

## 9. AI Analogy

| EF component | AI |
|---|---|
| Inhibition | gating, attention suppressing irrelevant |
| Updating | RNN/memory write, Transformer KV |
| Shifting | task/context switching, in-context learning |
| Planning | tree search, planning module |

LLMs still weaker than human EF in multi-step planning / interference inhibition (differentiation dimension).

---

## 10. Common Pitfalls

### 10.1 EF = IQ

Correlated but separable; EF predicts outcomes independent of IQ.

### 10.2 EF = Working Memory

WM is a subcomponent; EF broader (inhibition + switching + planning).

### 10.3 Single Unified Ability

Unity + diversity: separable components (Miyake).

### 10.4 Tasks Purely Measure EF

Task impurity; need latent variable separation.

### 10.5 Training Greatly Boosts General EF

Near transfer exists, far transfer / general boost evidence weak (same brain-training debate).

---

## 11. Related Concepts

- **Same section**: [Working Memory](Working_Memory.en.md), [Decision_Making](Decision_Making.en.md), [Metacognition](Metacognition.en.md), [Attention](Attention.en.md)
- **Anatomy**: [Cortex](../01_Neuroanatomy/Cortex.en.md) (PFC)
- **Disease**: [ADHD](../08_Neuro_Disorders/ADHD.en.md), [Frontotemporal Dementia](../08_Neuro_Disorders/Frontotemporal_Dementia.en.md)

---

## References

1. **Miyake, A. et al.** "The unity and diversity of executive functions." *Cogn Psychol*, 2000.
2. **Diamond, A.** "Executive functions." *Annu Rev Psychol*, 2013.
3. **Miller, E. K. & Cohen, J. D.** "An integrative theory of prefrontal cortex function." *Annu Rev Neurosci*, 2001.
4. **Moffitt, T. E. et al.** "A gradient of childhood self-control predicts health, wealth, and public safety." *PNAS*, 2011.
