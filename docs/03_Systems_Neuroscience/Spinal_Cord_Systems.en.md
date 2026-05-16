# Spinal Cord Systems

> *The spinal cord is not just a "cable": it contains reflex arcs, central pattern generators (CPG, producing rhythmic movement), sensory gating, local circuits. Ascending (sensory) + descending (motor) tracts + gray matter circuits. Injury localization (transection/hemisection/anterior horn) has characteristic syndromes. A key level for motor control + rehabilitation + neuroprostheses.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Motor_System](Motor_System.en.md), [Somatosensory](Somatosensory.en.md)

---

## 1. Structure

- 31 segments (8C+12T+5L+5S+1Co)
- **Gray matter** (central H-shape): dorsal horn (sensory), ventral horn (motor), intermediolateral column (autonomic)
- **White matter** (peripheral tracts): ascending + descending
- Rexed laminae (I-X) layering

---

## 2. Major Tracts

| Tract | Direction | Function |
|---|---|---|
| Dorsal column (DCML) | Ascending | Fine touch/proprioception |
| Spinothalamic | Ascending | Pain/temp (decussate early) |
| Corticospinal | Descending | Voluntary fine movement |
| Rubro/reticulo/vestibulospinal | Descending | Posture/gross movement |
| Spinocerebellar | Ascending | Proprioception → cerebellum |

---

## 3. Reflex Arc

- **Stretch reflex** (knee jerk): muscle spindle Ia → α motor neuron (monosynaptic)
- **Flexor withdrawal + crossed extensor**: polysynaptic protective reflex
- **Golgi tendon organ** (Ib): tension → inhibition (protective)
- Reflexes modulated by spinal + descending (variable gain)

---

## 4. Central Pattern Generator (CPG)

- Spinal intrinsic circuits produce rhythmic movement (walking/swimming) **without rhythmic input**
- Half-center model: mutual inhibition oscillation
- Isolated spinal cord + drug (e.g., L-DOPA) → still shows "fictive locomotion"
- See [Neural Circuits](../00_Foundations/Neural_Circuits.en.md) (CPG motif)

---

## 5. PyTorch — Half-Center CPG

```python
import torch

def half_center_cpg(T=300, dt=0.1, tau=5.0, w_inhib=2.0):
    """Mutual inhibition + adaptation → alternating rhythmic output."""
    a, b = 1.0, 0.0
    fa, fb = 0.0, 0.0   # fatigue/adaptation
    out = []
    for _ in range(T):
        a += dt/tau * (-a + torch.relu(torch.tensor(1.0 - w_inhib*b - fa)))
        b += dt/tau * (-b + torch.relu(torch.tensor(1.0 - w_inhib*a - fb)))
        fa += dt/tau * (-fa + 0.5*a); fb += dt/tau * (-fb + 0.5*b)
        out.append((float(a), float(b)))
    return out   # antiphase rhythm = locomotor pattern
```

---

## 6. Sensory Gating

- Dorsal horn is the pain "gate" (see [Pain_System](Pain_System.en.md))
- Presynaptic inhibition, local interneurons
- Descending modulation (PAG-RVM)
- Not a passive relay → active filtering/integration

---

## 7. Injury Syndromes (Localization)

| Injury | Presentation |
|---|---|
| Complete transection | Complete paralysis + sensory loss + autonomic below level |
| **Brown-Séquard** (hemisection) | Ipsilateral motor+proprioception loss, contralateral pain/temp loss |
| Anterior cord syndrome | Pain/temp + motor loss, proprioception spared |
| Central cord (syringomyelia) | Segmental pain/temp loss ("cape-like") |
| Spinal shock | Acute areflexia (later recovery + spasticity) |
| ALS | Upper + lower motor neuron (see [ALS](../08_Neuro_Disorders/ALS.en.md)) |

---

## 8. Clinical + Neuroprostheses

- **Spinal cord injury (SCI)**: rehab + epidural electrical stimulation (EES) → reactivate CPG → partial walking recovery (Courtine)
- **Closed-loop EES + BCI**: intent decoding → stimulation (brain-spine interface, 2023 Lancet)
- **Exoskeletons**, functional electrical stimulation (FES)
- Neural regeneration (scaffold/stem cell) under research

---

## 9. Relation to AI / Robotics

- CPG ↔ robot locomotion control (rhythm generators, quadruped/snake)
- Reflex + descending = hierarchical control (low-level fast reflex + high-level planning)
- Spinal "edge computing" ↔ distributed control
- See eng-notes robot control

---

## 10. Common Pitfalls

### 10.1 Spinal Cord Is Just a Cable

Contains reflexes + CPG + sensory gating + local computation.

### 10.2 Walking Entirely Brain-Driven

CPG generates rhythm in the spinal cord; brain initiates + modulates.

### 10.3 Reflex Gain Fixed

Plastically modulated by spinal + descending (state-dependent).

### 10.4 Spinal Shock = Permanent

Acute-phase areflexia, later recovery (often becomes spasticity).

### 10.5 SCI Completely Unrecoverable

EES + rehab can partially recover (residual pathways + CPG reactivation).

---

## 11. Related Concepts

- **Same section**: [Motor_System](Motor_System.en.md), [Somatosensory](Somatosensory.en.md), [Pain_System](Pain_System.en.md)
- **Foundation**: [Neural Circuits](../00_Foundations/Neural_Circuits.en.md) (CPG)
- **Disease**: [ALS](../08_Neuro_Disorders/ALS.en.md)
- **Frontiers**: [Closed_Loop_Neuromodulation](../07_Neurotech_Frontiers/Closed_Loop_Neuromodulation.en.md) (EES)

---

## References

1. **Grillner, S.** "Biological pattern generation: the cellular and computational logic of networks in motion." *Neuron*, 2006.
2. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
3. **Courtine, G. & Sofroniew, M. V.** "Spinal cord repair: advances in biology and technology." *Nat Med*, 2019.
4. **Lorach, H. et al.** "Walking naturally after spinal cord injury using a brain-spine interface." *Nature*, 2023.
