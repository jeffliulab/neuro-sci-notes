# Prefrontal Cortex

> *PFC = cortex anterior to frontal motor/premotor areas, largest proportion in humans. Core of executive function, working memory, decision-making, social behavior, inhibitory control. Three subregions: dlPFC (cognition), vmPFC/OFC (value emotion), ACC (conflict monitoring). Latest to mature (~25 years myelination). Phineas Gage is the historical case for its function.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Cortex](Cortex.en.md), [Executive Function](../04_Cognitive_Neuroscience/Executive_Function.en.md)

---

## 1. Definition + Subregions

| Subregion | Function |
|---|---|
| **dlPFC** (dorsolateral) | Working memory, rules, planning, shifting |
| **vmPFC / OFC** (ventromedial/orbitofrontal) | Value, emotion regulation, social decision |
| **ACC** (anterior cingulate) | Conflict monitoring, error, motivation |
| **lPFC/vlPFC** | Inhibitory control, language (left near Broca) |
| **frontopolar (BA10)** | Metacognition, multitasking, relational reasoning |

---

## 2. Connectivity

- Widely interconnected with nearly all cortex + limbic + BG + thalamus MD + brainstem neuromodulators
- "Hub": integration + top-down control
- Receives all neuromodulators (DA/NE/5-HT/ACh) → state-sensitive (inverted-U, see [ADHD](../08_Neuro_Disorders/ADHD.en.md))

---

## 3. Core Functions

- **Executive function** (see [Executive Function](../04_Cognitive_Neuroscience/Executive_Function.en.md))
- **Working memory**: dlPFC persistent activity (see [Working Memory](../04_Cognitive_Neuroscience/Working_Memory.en.md))
- **Decision/value**: vmPFC/OFC (see [Decision_Making](../04_Cognitive_Neuroscience/Decision_Making.en.md))
- **Social/moral**: vmPFC (Damasio somatic markers)
- **Inhibitory control**: vlPFC → suppress prepotent responses

---

## 4. PyTorch — PFC Persistent Activity (WM)

```python
import torch

def pfc_persistent_activity(cue, T=100, recurrent_w=1.05, decay=0.05):
    """Recurrent self-excitation maintains WM trace after cue ends."""
    h = 0.0
    trace = []
    for t in range(T):
        inp = cue if t < 10 else 0.0           # cue only early
        h = torch.tanh(torch.tensor(recurrent_w * h + inp - decay))
        trace.append(float(h))
    return trace   # activity persists in delay (attractor, see Working_Memory)
```

---

## 5. Phineas Gage (Historical Case)

- 1848 iron rod through left frontal (esp. vmPFC) → intellect/memory preserved, **personality/social decision dramatically changed**
- First PFC ↔ personality/social behavior evidence
- Modern: vmPFC damage → Iowa gambling deficit (Damasio)

---

## 6. Development (Latest to Mature)

- Synaptic pruning + myelination continue to ~ 25 years (back→front last, see [Neurodevelopment](../00_Foundations/Neurodevelopment.en.md))
- Explains adolescent risky decisions + impulsivity (PFC-limbic imbalance)
- Legal/educational implications

---

## 7. Pathology

- **bvFTD**: vmPFC/OFC degeneration → disinhibition, personality (see [Frontotemporal_Dementia](../08_Neuro_Disorders/Frontotemporal_Dementia.en.md))
- **ADHD**: PFC-striatal DA/NE dysregulation (see [ADHD](../08_Neuro_Disorders/ADHD.en.md))
- **Schizophrenia**: dlPFC hypoactivation (hypofrontality) + WM impairment
- **Depression**: dlPFC↓ / sgACC↑ (DBS target, see [Depression](../08_Neuro_Disorders/Depression.en.md))
- **TBI**: frontal often affected → executive impairment

---

## 8. Evolution

- Human PFC absolutely + relatively enlarged (esp. frontopolar)
- Connectivity complexity + neuromodulator innervation ↑
- Linked to abstraction, planning, social complexity
- But degree of "human uniqueness" debated (primate continuum)

---

## 9. Relation to AI

- PFC = "executive controller / central scheduler" ↔ controller, gating, meta-controller analogy
- Persistent activity WM ↔ RNN/working memory (see [Working Memory](../04_Cognitive_Neuroscience/Working_Memory.en.md))
- Mixed selectivity (see [Neural Population Dynamics](../05_Computational_Neuroscience/Neural_Population_Dynamics.en.md))
- LLMs lack robust multi-step planning/inhibition → PFC-like function gap

---

## 10. Common Pitfalls

### 10.1 PFC = "Rationality Center"

Also governs emotion regulation/social/value (vmPFC); not pure "cold cognition".

### 10.2 One Region One Function

Subregions divide labor but highly interact; lesion effects distributed.

### 10.3 PFC Matures in Childhood

Latest (~25 years); adolescent PFC immature.

### 10.4 Bigger = Smarter

Connectivity/neuromodulator/microstructure > volume; EQ complex.

### 10.5 Gage Completely "Lost Humanity"

Personality changed but most functions preserved; details exaggerated (historical scrutiny).

---

## 11. Related Concepts

- **Same section**: [Cortex](Cortex.en.md), [Cingulate_Cortex](Cingulate_Cortex.en.md), [Basal_Ganglia](Basal_Ganglia.en.md), [Thalamus](Thalamus.en.md)
- **Cognition**: [Executive Function](../04_Cognitive_Neuroscience/Executive_Function.en.md), [Working Memory](../04_Cognitive_Neuroscience/Working_Memory.en.md), [Decision_Making](../04_Cognitive_Neuroscience/Decision_Making.en.md)
- **Disease**: [Frontotemporal_Dementia](../08_Neuro_Disorders/Frontotemporal_Dementia.en.md), [ADHD](../08_Neuro_Disorders/ADHD.en.md)

---

## References

1. **Miller, E. K. & Cohen, J. D.** "An integrative theory of prefrontal cortex function." *Annu Rev Neurosci*, 2001.
2. **Fuster, J. M.** *The Prefrontal Cortex*. 5th ed., 2015.
3. **Damasio, A. R.** *Descartes' Error*. 1994.
4. **Arnsten, A. F. T.** "Stress signalling pathways that impair prefrontal cortex structure and function." *Nat Rev Neurosci*, 2009.
