# Cingulate Cortex

> *The cingulate gyrus arcs around the corpus callosum, a limbic core. Anterior cingulate (ACC): conflict monitoring, error, pain "unpleasantness", motivation. Posterior cingulate (PCC): DMN hub, self-reference. Subgenual ACC (sgACC, Brodmann 25) = treatment-resistant depression DBS target. Papez circuit member. Function spans cognition-emotion-autonomic.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Cortex](Cortex.en.md), [Limbic_System](../03_Systems_Neuroscience/Limbic_System.en.md)

---

## 1. Subdivisions

| Region | Function |
|---|---|
| **dACC** (dorsal anterior) | Conflict monitoring, error detection, cognitive control, pain affect |
| **sgACC/BA25** (subgenual) | Emotion, depression (DBS target) |
| **PCC** (posterior) | DMN, self-reference, memory retrieval |
| **RSC** (retrosplenial) | Spatial navigation, egocentric↔allocentric |

---

## 2. ACC — Conflict/Error

- **Error-Related Negativity (ERN)**: ~ 50-100 ms post-error scalp potential (ACC source)
- Conflict monitoring theory (Botvinick): dACC detects response conflict → recruits dlPFC control
- Pain "unpleasantness" (affective) in ACC (sensory intensity in S1)
- Motivation/effort allocation

---

## 3. PyTorch — Conflict Monitoring → Control Recruitment

```python
import torch

def acc_conflict_control(resp_a, resp_b, dlpfc_gain=1.0):
    """dACC detects co-active competing responses -> recruit dlPFC control."""
    conflict = resp_a * resp_b                       # both active = conflict
    control_signal = dlpfc_gain * torch.sigmoid(conflict - 0.3)
    # Next trial: heightened control reduces conflict (adaptation)
    return conflict, control_signal
```

---

## 4. PCC + Default Mode Network

- PCC/precuneus = DMN core hub (high metabolism)
- Self-reference, autobiographical memory, mind-wandering (see [Self_and_Agency](../04_Cognitive_Neuroscience/Self_and_Agency.en.md))
- Suppressed during task; DMN-task network anticorrelated
- Anesthesia/reduced consciousness → PCC activity/connectivity changes (consciousness marker)

---

## 5. sgACC (BA25) — Depression DBS

- Treatment-resistant depression: sgACC hyperactivity/abnormal connectivity
- Mayberg DBS target (white matter tract) → partial relief (mixed trial results)
- sgACC normalizes after antidepressant/CBT
- See [Depression](../08_Neuro_Disorders/Depression.en.md), [DBS](../07_Neurotech_Frontiers/DBS.en.md)

---

## 6. Connectivity + Integration

- ACC connects PFC + limbic + autonomic (hypothalamus/brainstem) + motor
- "Cognition-emotion-autonomic" interface
- Von Economo neurons (large, fast-projecting, human/ape/whale; ACC + anterior insula) — selectively vulnerable in bvFTD (see [Frontotemporal_Dementia](../08_Neuro_Disorders/Frontotemporal_Dementia.en.md))

---

## 7. Dual Dimensions of Pain

- **Sensory-discriminative** (where/how strong): S1/S2
- **Affective-motivational** (how unpleasant): ACC + insula (see [Pain_System](../03_Systems_Neuroscience/Pain_System.en.md))
- Cingulotomy → reduces pain "unpleasantness" not sensation (historical + intractable pain)

---

## 8. Pathology

- **Depression**: sgACC abnormality
- **OCD**: ACC hyperactivity (CSTC, see [OCD](../08_Neuro_Disorders/OCD.en.md))
- **Pain chronification**: ACC plasticity
- **Disorders of consciousness**: PCC marker
- **bvFTD**: ACC von Economo loss

---

## 9. Relation to AI

- Conflict monitoring → control recruitment ↔ uncertainty triggers extra computation / "think more" (adaptive computation-like)
- Error signal (ERN) ↔ internal error monitor / self-evaluation
- DMN ↔ "offline" simulation / default generation (see [Creativity](../04_Cognitive_Neuroscience/Creativity.en.md))

---

## 10. Common Pitfalls

### 10.1 Cingulate Single Function

ACC vs PCC vs sgACC vs RSC functions distinct (subdivisions).

### 10.2 ACC = Pain Center

Encodes pain **affective** dimension, not sensory intensity (S1).

### 10.3 DMN = Functionless Idle

Active self/memory/simulation network.

### 10.4 sgACC DBS Definitely Effective

Mixed trial results (individualized target + white matter tract key).

### 10.5 ERN Only "Making Errors"

Also reflects conflict / feedback / reinforcement learning signals.

---

## 11. Related Concepts

- **Same section**: [Cortex](Cortex.en.md), [Prefrontal_Cortex](Prefrontal_Cortex.en.md), [Amygdala](Amygdala.en.md)
- **Systems**: [Limbic_System](../03_Systems_Neuroscience/Limbic_System.en.md), [Pain_System](../03_Systems_Neuroscience/Pain_System.en.md)
- **Cognition**: [Decision_Making](../04_Cognitive_Neuroscience/Decision_Making.en.md), [Self_and_Agency](../04_Cognitive_Neuroscience/Self_and_Agency.en.md)
- **Disease**: [Depression](../08_Neuro_Disorders/Depression.en.md), [OCD](../08_Neuro_Disorders/OCD.en.md)

---

## References

1. **Botvinick, M. M. et al.** "Conflict monitoring and cognitive control." *Psychol Rev*, 2001.
2. **Shackman, A. J. et al.** "The integration of negative affect, pain and cognitive control in the cingulate cortex." *Nat Rev Neurosci*, 2011.
3. **Mayberg, H. S. et al.** "Deep brain stimulation for treatment-resistant depression." *Neuron*, 2005.
4. **Vogt, B. A.** "Pain and emotion interactions in subregions of the cingulate gyrus." *Nat Rev Neurosci*, 2005.
