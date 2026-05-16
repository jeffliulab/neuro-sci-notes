# Vestibular System

> *The vestibular system detects head acceleration + gravity → balance, spatial orientation, gaze stabilization (VOR). Inner ear: semicircular canals (angular acceleration) + otolith organs (linear acceleration/gravity). Multisensory integration with vision/proprioception. VOR is the fastest reflex (~ 7 ms). Vertigo, motion sickness, space adaptation are its dysfunctions. Robot IMU is its engineering counterpart.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Auditory_System](Auditory_System.en.md), [Cerebellum](../01_Neuroanatomy/Cerebellum.en.md)

---

## 1. Peripheral Organs

| Organ | Detects | Count |
|---|---|---|
| **Semicircular canals** | Angular acceleration (rotation) | 3 pairs (mutually orthogonal) |
| **Utricle** | Horizontal linear accel + gravity | 1 pair |
| **Saccule** | Vertical linear accel + gravity | 1 pair |

Hair cells — homologous with cochlea, mechanically gated transduction.

---

## 2. Transduction Mechanism

- Hair cell stereocilia toward kinocilium → depolarization (excitation); opposite → hyperpolarization
- Semicircular canal: endolymph inertia → cupula deflection → angular acceleration
- Otolith organs: otolith (calcium carbonate crystal) inertia → gravity + linear acceleration
- Resting discharge → bidirectional coding (increase/decrease)

---

## 3. Central Pathway

```
Vestibular nerve (CN VIII)
   ↓
Vestibular nuclei (brainstem)
   ├→ Oculomotor nuclei (VOR — gaze stabilization)
   ├→ Spinal cord (vestibulospinal reflex — posture)
   ├→ Cerebellum (adaptation / calibration)
   ├→ Thalamus → parieto-insular vestibular cortex (PIVC, spatial orientation)
   └→ Autonomic (motion sickness → nausea)
```

---

## 4. VOR (Vestibulo-Ocular Reflex)

- Head turns → opposite equal-speed eye movement → retinal image stable
- Latency ~ 7-10 ms (one of fastest reflexes, open-loop feedforward)
- Gain ≈ 1 (adaptable: wear reversing prisms → cerebellar recalibration)
- Clinical: head impulse test measures VOR

---

## 5. PyTorch — VOR Gain Adaptation

```python
import torch

def vor_adaptation(head_velocity, gain, target_gain=1.0, lr=0.01, T=100):
    """Cerebellum-driven gain recalibration to minimize retinal slip."""
    gains = []
    for _ in range(T):
        eye_velocity = -gain * head_velocity
        retinal_slip = head_velocity + eye_velocity      # want ≈ 0
        gain += lr * retinal_slip * head_velocity        # error-driven
        gains.append(gain)
    return gains   # converges toward target_gain
```

---

## 6. Multisensory Integration

- Balance = vestibular + visual + proprioceptive fusion
- Conflict → vertigo / motion sickness (sensory mismatch theory)
- Bayesian cue combination (see [Bayesian Brain](../05_Computational_Neuroscience/Bayesian_Brain.en.md))
- Weightlessness (space) → reweighting → space adaptation syndrome

---

## 7. Pathology

- **BPPV** (benign paroxysmal positional vertigo): otolith displacement into canal (most common vertigo, Epley repositioning)
- **Ménière's disease**: endolymphatic hydrops → vertigo + tinnitus + fluctuating hearing
- **Vestibular neuritis**: acute unilateral vestibular loss
- **Motion sickness**: sensory conflict
- **Bilateral vestibulopathy**: oscillopsia, poor balance
- **Vestibular migraine**

---

## 8. Relation to Engineering / AI

- Semicircular canals + otolith ↔ **IMU** (gyro + accelerometer, see eng-notes IMU)
- VOR ↔ image stabilization (camera/drone gimbal)
- Vestibular-visual fusion ↔ Visual-Inertial Odometry (VIO)
- Sensory mismatch ↔ VR motion sickness (sensory conflict)

---

## 9. Rehabilitation

- Vestibular rehabilitation therapy (VRT): promotes central compensation + adaptation
- Epley maneuver (BPPV)
- Gaze stabilization exercises
- Vestibular implant (experimental, cochlear-implant-like)

---

## 10. Common Pitfalls

### 10.1 Vestibular Only for Balance

Also gaze stabilization (VOR), spatial orientation, autonomic (nausea).

### 10.2 Vertigo = Dizziness

Vertigo = spinning illusion (vestibular); dizziness is broader.

### 10.3 VOR Is Slow Feedback

One of fastest reflexes (~7 ms, open-loop feedforward).

### 10.4 One-Side Vestibular Loss = Permanent Imbalance

Central compensation + rehab largely recovers.

### 10.5 Unrelated to Hearing

Same inner ear + homologous hair cells + shared CN VIII.

---

## 11. Related Concepts

- **Same section**: [Auditory_System](Auditory_System.en.md), [Somatosensory](Somatosensory.en.md), [Motor_System](Motor_System.en.md)
- **Anatomy**: [Cerebellum](../01_Neuroanatomy/Cerebellum.en.md), [Brainstem](../01_Neuroanatomy/Brainstem.en.md)
- **Computational**: [Bayesian Brain](../05_Computational_Neuroscience/Bayesian_Brain.en.md)
- **Engineering**: IMU, VIO — https://jeffliulab.github.io/eng-notes/

---

## References

1. **Goldberg, J. M. et al.** *The Vestibular System: A Sixth Sense*. Oxford, 2012.
2. **Angelaki, D. E. & Cullen, K. E.** "Vestibular system: the many facets of a multimodal sense." *Annu Rev Neurosci*, 2008.
3. **Cullen, K. E.** "The vestibular system: multimodal integration and encoding of self-motion." *Trends Neurosci*, 2012.
4. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
