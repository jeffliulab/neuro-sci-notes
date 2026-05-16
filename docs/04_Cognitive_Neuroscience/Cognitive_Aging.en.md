# Cognitive Aging

> *Normal aging ≠ dementia. Fluid abilities (processing speed, WM, episodic, EF) decline with age; crystallized (vocabulary, knowledge) preserved or even improves. Mechanisms: PFC/hippocampus atrophy, white matter degradation, dopamine↓, dedifferentiation. Cognitive reserve + compensation (PASA, HAROLD) explain individual differences. Distinguishing normal aging vs MCI/AD is clinical core.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Executive Function](Executive_Function.en.md), [Memory_Systems](Memory_Systems.en.md)

---

## 1. Dual Track: Fluid vs Crystallized

| | Fluid | Crystallized |
|---|---|---|
| Content | Processing speed, WM, reasoning, episodic, EF | Vocabulary, general knowledge, expertise |
| Trajectory | Gradual decline after adulthood (~20s onward) | Stable / rising to 60s+ |
| Example | Digit symbol, Raven | Vocabulary test |

→ "Aging is not global decline" (domain differentiation).

---

## 2. Most Affected Domains

- **Processing speed** (Salthouse: speed↓ mediates most decline)
- **Working memory / EF** (esp. updating, shifting, see [Executive Function](Executive_Function.en.md))
- **Episodic memory** (esp. free recall, source memory; recognition more preserved)
- Relatively preserved: semantic, procedural, emotion regulation ("positivity effect")

---

## 3. Neural Mechanisms

- **Structure**: PFC + hippocampus atrophy most prominent; white matter integrity↓ (see [DTI Tractography](../07_Neurotech_Frontiers/DTI_Tractography.en.md))
- **Dopamine**: D2 receptor ~ -10% per decade → affects WM/EF/speed
- **Dedifferentiation**: neural representation specificity↓ (broader tuning)
- Default network connectivity↓; synaptic/dendritic changes

---

## 4. Compensation Models

| Model | Description |
|---|---|
| **HAROLD** | Older adults bilateral PFC activation (de-lateralization compensation) |
| **PASA** | Posterior→anterior shift (occipital↓ frontal↑) |
| **CRUNCH** | Over-activation at low load, resources exhausted at high load |
| **Scaffolding (STAC)** | Compensatory scaffolding against neural degradation |

---

## 5. PyTorch — Dedifferentiation Simulation

```python
import torch

def neural_tuning(stimulus, age_factor=0.0):
    """Aging broadens tuning curves -> less distinct representations."""
    preferred = torch.linspace(0, 1, 10)
    sigma = 0.1 + 0.3 * age_factor          # older -> wider tuning
    resp = torch.exp(-((stimulus - preferred) ** 2) / (2 * sigma ** 2))
    return resp   # high age_factor -> overlapping (dedifferentiated) codes
```

---

## 6. Cognitive Reserve

- High education / cognitive activity / bilingualism / social → better function at equal pathology
- "Reserve" buffers (doesn't prevent) degradation
- Explains why brain pathology dissociates from symptoms (AD pathology yet no symptoms)
- Emphasizes modifiable factors (exercise, cognitive + social engagement)

---

## 7. Normal Aging vs Pathology

| | Normal aging | MCI | AD dementia |
|---|---|---|---|
| Episodic | Mild decline (recall) | Marked objective impairment | Severe + progressive |
| Function | Preserved | Largely preserved | Impaired |
| Progression | Slow stable | May convert to AD | Progressive |

Differentiation is clinical core (see [Alzheimer](../08_Neuro_Disorders/Alzheimer.en.md)).

---

## 8. Protective / Risk Factors

- **Protective**: aerobic exercise (strongest evidence), education, social, Mediterranean diet, sleep, cognitive engagement
- **Risk**: hypertension/diabetes (vascular), smoking, depression, loneliness, hearing loss (modifiable!)
- Lancet Commission: ~ 40% of dementia risk attributable to modifiable factors

---

## 9. Relation to AI

- Dedifferentiation ↔ representation collapse / capacity decay
- Cognitive reserve ↔ redundancy / robustness / model capacity
- Aging trajectory modeling ↔ longitudinal ML (disease progression prediction)
- But AI has no biological aging; analogy limited

---

## 10. Common Pitfalls

### 10.1 Aging = Global Cognitive Decline

Crystallized preserved/rising; domain differentiation.

### 10.2 Aging = Dementia

Normal aging mild, stable, function preserved; dementia is pathological.

### 10.3 Brain Atrophy = Definitely Symptoms

Cognitive reserve dissociates pathology and symptoms.

### 10.4 Compensatory Activation = Better

Compensation can be effective or not (CRUNCH: exhausted at high load).

### 10.5 Aging Not Modifiable

~40% of dementia risk modifiable (exercise/hearing/vascular etc.).

---

## 11. Related Concepts

- **Same section**: [Executive Function](Executive_Function.en.md), [Memory_Systems](Memory_Systems.en.md), [Working Memory](Working_Memory.en.md)
- **Disease**: [Alzheimer](../08_Neuro_Disorders/Alzheimer.en.md)
- **Frontiers**: [DTI Tractography](../07_Neurotech_Frontiers/DTI_Tractography.en.md)
- **Foundation**: [Neuroplasticity](../00_Foundations/Neuroplasticity.en.md)

---

## References

1. **Salthouse, T. A.** "The processing-speed theory of adult age differences in cognition." *Psychol Rev*, 1996.
2. **Cabeza, R.** "Hemispheric asymmetry reduction in older adults: the HAROLD model." *Psychol Aging*, 2002.
3. **Park, D. C. & Reuter-Lorenz, P.** "The adaptive brain: aging and neurocognitive scaffolding." *Annu Rev Psychol*, 2009.
4. **Livingston, G. et al.** "Dementia prevention, intervention, and care (Lancet Commission)." *Lancet*, 2020.
