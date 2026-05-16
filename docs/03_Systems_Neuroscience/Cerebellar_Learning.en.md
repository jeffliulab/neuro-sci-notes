# Cerebellar Learning System

> *The cerebellum does more than coordinate movement: it performs **supervised error learning**. Marr-Albus-Ito theory: parallel fiber-Purkinje synapses adjusted via climbing fiber "error signal" LTD. Forward/internal models → prediction + correction (VOR adaptation, saccade adaptation). Recently: cerebellum also involved in cognition/emotion. A biological exemplar of supervised learning.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Cerebellum](../01_Neuroanatomy/Cerebellum.en.md), [Motor_System](Motor_System.en.md)

---

## 1. Circuit (review + learning view)

```
Mossy fiber → Granule cell (massive, expansion coding)
   ↓ Parallel fiber (~200,000/Purkinje)
Purkinje cell ←── Climbing fiber (inferior olive, 1/PC, "error/teaching signal")
   ↓ (sole output, GABA)
Deep nuclei → thalamus/brainstem
```

Granule cell layer = high-dimensional sparse expansion (reservoir/SVM-kernel-like, see [Reservoir Computing](../05_Computational_Neuroscience/Reservoir_Computing.en.md)).

---

## 2. Marr-Albus-Ito Learning

- **Marr 1969 / Albus 1971**: parallel fiber-Purkinje synapse plastic; climbing fiber = teaching signal
- **Ito**: discovered **PF-PC LTD** (climbing fiber + parallel fiber co-activation → long-term depression)
- → Cerebellum = supervised learner (error-driven, perceptron-like)

---

## 3. Supervised vs Three-System Comparison

| System | Learning type | Teaching signal |
|---|---|---|
| **Cerebellum** | Supervised | Climbing fiber (error) |
| Basal ganglia | Reinforcement | Dopamine (RPE) |
| Cortex/hippocampus | Un/self-supervised | Hebbian/statistical |

→ Brain's three learning systems division of labor (Doya 2000).

---

## 4. Internal Models (Forward/Inverse)

- **Forward model**: predicts sensory result from motor command (faster than real feedback → real-time control)
- **Inverse model**: computes required command from goal
- Cerebellum = where internal models stored/learned
- Explains: can't tickle yourself (prediction attenuation, see [Self_and_Agency](../04_Cognitive_Neuroscience/Self_and_Agency.en.md))

---

## 5. PyTorch — Cerebellar Perceptron (error-driven LTD)

```python
import torch

def cerebellar_perceptron(granule_act, weights, climbing_error, lr=0.01):
    """PF-PC LTD: parallel fiber weights decrease when paired with CF error."""
    purkinje_out = (weights * granule_act).sum()         # PC firing
    # Climbing fiber error -> LTD at co-active PF synapses
    weights = weights - lr * climbing_error * granule_act
    return purkinje_out, weights   # supervised, error-corrected
```

---

## 6. Classic Adaptation Paradigms

- **VOR adaptation**: magnifying/reversing glasses → gain recalibration (see [Vestibular_System](Vestibular_System.en.md))
- **Saccade adaptation**: target jump → eye movement amplitude correction
- **Prism adaptation**: prism offset → pointing recalibration (aftereffect after removal)
- **Eyeblink conditioning**: CS-US pairing (cerebellar HVI/interpositus, classic)

---

## 7. Beyond Motor — Cognitive Cerebellum

- Posterolateral cerebellum ↔ PFC (closed loop) → cognitive/language/working memory involvement
- "Cerebellar cognitive affective syndrome" (Schmahmann)
- Hypothesis: cerebellum does "coordination + error correction + internal model" for **cognition** too (unified computation)
- Emotion/social cognition also involved

---

## 8. Clinical

- **Cerebellar damage**: ataxia, dysmetria, intention tremor, dysarthria
- **VOR/saccade adaptation deficits** (adaptation learning impaired)
- **CCAS**: executive/visuospatial/language/emotional blunting
- Degeneration (SCA), stroke, tumor (medulloblastoma)
- Linked to autism (cerebellar developmental abnormality)

---

## 9. Relation to AI

- Marr-Albus = early perceptron biological prototype
- Granule layer expansion ↔ kernel / reservoir / random features
- Supervised error learning ↔ supervised learning (but local rule, not backprop, see [Synaptic Plasticity Models](../05_Computational_Neuroscience/Synaptic_Plasticity_Models.en.md))
- Forward model ↔ world model / model-based control (see ai-notes world models)

---

## 10. Common Pitfalls

### 10.1 Cerebellum Only for Movement

Also cognition/language/emotion (CCAS; unified computation hypothesis).

### 10.2 Learning Only via PF-PC LTD

Also LTP, deep nuclei plasticity, multi-site (more complex).

### 10.3 Climbing Fiber = Motor Command

It's an **error/teaching** signal (inferior olive), not command.

### 10.4 = Backprop

It's **local** error-driven (climbing fiber), not global gradient backprop.

### 10.5 Cerebellar Damage = Paralysis

It's coordination/accuracy/learning impairment (ataxia), not paralysis.

---

## 11. Related Concepts

- **Same section**: [Motor_System](Motor_System.en.md), [Vestibular_System](Vestibular_System.en.md)
- **Anatomy**: [Cerebellum](../01_Neuroanatomy/Cerebellum.en.md)
- **Computational**: [Reservoir Computing](../05_Computational_Neuroscience/Reservoir_Computing.en.md), [Synaptic Plasticity Models](../05_Computational_Neuroscience/Synaptic_Plasticity_Models.en.md)
- **Cognition**: [Self_and_Agency](../04_Cognitive_Neuroscience/Self_and_Agency.en.md)
- **AI**: supervised learning, world model

---

## References

1. **Marr, D.** "A theory of cerebellar cortex." *J Physiol*, 1969.
2. **Albus, J. S.** "A theory of cerebellar function." *Math Biosci*, 1971.
3. **Ito, M.** "Cerebellar long-term depression: characterization, signal transduction, and functional roles." *Physiol Rev*, 2001.
4. **Schmahmann, J. D.** "The cerebellum and cognition." *Neurosci Lett*, 2019.
