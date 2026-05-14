# Cerebellum — "Little Brain", Motor Control + Learning

> *Though small (10% of brain volume), the cerebellum contains 80% of neurons (~70 billion). Function goes beyond "motor coordination" — modern research shows it participates in motor learning, cognition, emotion. Marr-Albus (1969) theory was an early computational neuroscience milestone.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Neuron](../02_Cellular_Molecular/Neuron.en.md), [Cortex](Cortex.en.md)

---

## 1. Anatomy

Located behind/below the brain, 3 parts:
- **Vermis** (middle)
- **Hemispheres** (left/right)
- **Flocculus** (vestibular-related)

3-layer cortex (vs cortex 6 layers):
- **Molecular layer**: parallel fibers + interneurons
- **Purkinje layer**: single row of Purkinje cell bodies
- **Granular layer**: granule cells (most numerous)

---

## 2. Main Cell Types

### 2.1 Purkinje Cell

- **Sole output** neuron
- Planar fan-shaped dendritic tree (200k+ synapses)
- GABAergic (inhibitory)
- Projects to deep cerebellar nuclei

### 2.2 Granule Cells

- Most numerous (~50 billion in humans)
- Receive mossy fibers
- Axons → parallel fibers, traverse molecular layer

### 2.3 Climbing Fibers

- From inferior olive
- 1 climbing fiber wraps 1 Purkinje (strong 1:1)
- Each activation produces complex spike

### 2.4 Mossy Fibers

- From spinal cord, pontine nuclei, vestibular
- Form mossy synapses on granule cells

---

## 3. Marr-Albus Learning Theory

```
Mossy fibers → Granule → Parallel fibers (~100k inputs / Purkinje)
                                    ↓
Climbing fiber (error/teacher signal) → Purkinje
                                    ↓
Long-term modify parallel fiber → Purkinje synapse (LTD)
```

Theory:
- Mossy fibers carry "state" information
- Climbing fiber is "error signal"
- LTD at parallel fiber → Purkinje implements supervised learning

Marr 1969 + Albus 1971 published independently. Modern evidence confirms many predictions.

---

## 4. Relation to Perceptron

Cerebellum = giant perceptron:
- Input: parallel fibers (100k)
- Output: Purkinje
- Learning: gradient descent via climbing fiber error

→ Cerebellum is the biological example of supervised learning.

---

## 5. Functions

### 5.1 Motor Control (Classic)

- Timing, fluency, coordination
- Cerebellar lesion → ataxia (motor incoordination)
- Fast skill learning (typing, piano)

### 5.2 Motor Learning

- Vestibulo-ocular reflex (VOR) adaptation: prism glasses → eye movements adjust
- Classical cerebellar plasticity test

### 5.3 Cognitive

- Last 20 years: cerebellum participates in language, thought, emotion
- "Cerebellar Cognitive Affective Syndrome" (Schmahmann)
- Cerebellar cortex has closed loops with prefrontal cortex

---

## 6. Pathology

- **Stroke (cerebellar)**: ataxia + dysarthria
- **Friedreich's ataxia**: genetic degeneration
- **Alcohol cerebellar degeneration**: chronic alcoholism
- **Autism**: cerebellar abnormalities

---

## 7. PyTorch — Cerebellum-Perceptron

```python
import torch
import torch.nn as nn

class CerebellumPerceptron(nn.Module):
    def __init__(self, n_parallel=100000):
        super().__init__()
        self.parallel_to_purkinje = nn.Linear(n_parallel, 1, bias=False)
        self.granule_sparsity = 0.05
    
    def forward(self, mossy_input):
        granule = (mossy_input > torch.quantile(mossy_input, 1 - self.granule_sparsity)).float()
        purkinje = self.parallel_to_purkinje(granule)
        return -purkinje
    
    def learn(self, mossy_input, climbing_signal, lr=1e-4):
        granule = (mossy_input > torch.quantile(mossy_input, 1 - self.granule_sparsity)).float()
        with torch.no_grad():
            self.parallel_to_purkinje.weight -= lr * climbing_signal * granule
```

---

## 8. Numbers

- Volume: 10% of brain
- Neurons: ~70 billion (80% of total brain)
- Purkinje cells: ~15 million
- Granule cells: ~50 billion
- Synapses: 100 trillion+

---

## 9. Common Pitfalls

### 9.1 "Just Motor" Misconception

Modern view includes cognition / emotion.

### 9.2 Simple Analogy to Cortex

3 vs 6 layers, very different wiring.

### 9.3 Marr-Albus Completeness

LTD isn't sole plasticity (LTP also); climbing fiber isn't only error.

### 9.4 Lesion Localization

Cerebellar lesion effects differ from cortex (more compensation).

### 9.5 Granule Cell Recording Hard

So small + numerous, experimentally very challenging.

---

## 10. Related Concepts

- **Same section**: [Cortex](Cortex.en.md), [Hippocampus Anatomy](Hippocampus_Anatomy.en.md), [Basal Ganglia](Basal_Ganglia.en.md)
- **Cellular**: [LTP/LTD](../02_Cellular_Molecular/LTP_LTD.en.md)
- **AI comparison**: Perceptron / SVM — https://jeffliulab.github.io/ai-notes/01_AI/02_Machine_Learning/

---

## References

1. **Marr, D.** "A theory of cerebellar cortex." *J Physiol*, 1969.
2. **Albus, J. S.** "A theory of cerebellar function." *Math Biosci*, 1971.
3. **Schmahmann, J. D.** "Cerebellar cognitive affective syndrome." *Brain*, 1998.
4. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
