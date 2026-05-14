# Consciousness — Neuroscience's Hardest Question

> *Consciousness is "what it's like to be me". Chalmers 1995 distinguishes **hard problem** (subjective experience) from easy problems (neural processes). Neuroscience has searched 50 years for NCC (Neural Correlates of Consciousness); candidate theories include GWT, IIT, predictive coding.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Cortex](../01_Neuroanatomy/Cortex.en.md)

---

## 1. Hard vs Easy Problem (Chalmers 1995)

- **Easy problem**: explain specific functions — attention, memory, decision... → solvable by finding neural circuits
- **Hard problem**: why do these processes accompany subjective experience? "Why is there something it's like to see red?"

Hard problem remains unsolved.

---

## 2. NCC (Neural Correlates of Consciousness)

Crick & Koch 1990s: find "necessary + sufficient" neural processes for consciousness.
Candidates:
- Late activity in cortex (vs early sensory)
- Recurrent activity
- Fronto-parietal network
- Thalamo-cortical loops

---

## 3. Main Theories

### 3.1 Global Workspace Theory (GWT, Dehaene)

- Info "broadcast" to cortical regions
- Entering global workspace = entering consciousness
- Otherwise = unconscious

### 3.2 Integrated Information Theory (IIT, Tononi)

- Consciousness = integrated information (Φ)
- Mathematically measure Φ: system minus parts info
- High Φ = high consciousness
- Predicts: plants / simple NNs also have low Φ

### 3.3 Higher-Order Theories (HOT)

- Consciousness = "I know I know"
- Meta-cognition is essence

### 3.4 Predictive Processing

- Brain is prediction machine
- Consciousness = current best prediction (Bayesian inference)
- Championed by Anil Seth et al.

---

## 4. Experimental Methods

### 4.1 Binocular Rivalry

Different images to each eye → consciousness alternates → measure NCC per period.

### 4.2 No-report Paradigms

Don't require subject report, look at brain activity → exclude motor confound.

### 4.3 Anesthesia

Measure consciousness loss ↔ Φ drop (Tononi validates IIT).

### 4.4 Coma / Vegetative

Test residual consciousness with fMRI commands ("imagine tennis") → some vegetative patients respond (Owen 2006).

---

## 5. Damage / Pathology

- **Locked-in syndrome**: consciousness fully intact but complete motor loss (only blink)
- **Persistent vegetative state**: awake but unconscious
- **Coma**: unconscious, not awake
- **Brain death**: total brain death

DBS / TMS can elicit partial response in minimally conscious patients.

---

## 6. AI Consciousness?

- LLM: no consciousness (no evidence so far; no IIT high Φ)
- But: subjective judgment hard — LLM can say "I'm conscious", how do we verify?
- Chalmers / Schwitzgebel: seriously discuss AI moral status

---

## 7. PyTorch — IIT-style Φ Concept

```python
import torch

def integrated_information(system, partition):
    """Toy Φ measure."""
    full_info = mutual_info(system)
    partition_info = sum(mutual_info(part) for part in partition)
    return full_info - partition_info
```

Real IIT computation → exponential in size, infeasible on large networks.

---

## 8. History

- **Descartes 1641**: cogito ergo sum
- **William James 1890**: stream of consciousness
- **1990s** — Crick & Koch NCC quest
- **1995** — Chalmers Hard Problem
- **2004** — Tononi IIT
- **2010s** — Dehaene GWT experimental validation
- **2020s** — AI consciousness discussions

---

## 9. Common Pitfalls

### 9.1 NCC ≠ Explanation

Knowing related regions ≠ explaining why they're accompanied by subjective experience.

### 9.2 "Default Mode Network" ≠ Consciousness

DMN relates to self-referential, but not sole consciousness substrate.

### 9.3 Anesthesia Complex

Not just consciousness off — also unconscious brain activity.

### 9.4 IIT Φ Incomputable

Real brain exact Φ NP-hard; only approximations.

### 9.5 LLM ≠ Conscious (current view)

But absence of evidence ≠ evidence of absence; stay open.

---

## 10. Related Concepts

- **Same section**: [Decision-Making](Decision_Making.en.md), [Language](Language.en.md)
- **AI ethics**: AI consciousness / moral status

---

## References

1. **Chalmers, D.** "Facing up to the problem of consciousness." *J Conscious Stud*, 1995.
2. **Crick, F. & Koch, C.** "Towards a neurobiological theory of consciousness." *Semin Neurosci*, 1990.
3. **Tononi, G.** "An information integration theory of consciousness." *BMC Neurosci*, 2004.
4. **Dehaene, S. & Naccache, L.** "Towards a cognitive neuroscience of consciousness (GWT)." *Cognition*, 2001.
5. **Seth, A.** *Being You: A New Science of Consciousness*. 2021.
