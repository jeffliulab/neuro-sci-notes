# Motor System — M1, Premotor, BG, Cerebellum Coordination

> *The motor system extends from cortex M1 → spinal cord → muscle, but is far more than simple motor command. Premotor / SMA plans, BG selects, cerebellum calibrates, PPC integrates sensation. This article covers components + population coding + AI comparison.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Cortex](../01_Neuroanatomy/Cortex.en.md), [Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.en.md), [Cerebellum](../01_Neuroanatomy/Cerebellum.en.md)

---

## 1. System Components

```
PFC → Premotor / SMA (planning)
       ↓
    M1 (execution)
       ↓
  Brainstem / Spinal cord → motor neurons → muscles
       ↑                       ↑
  Cerebellum (timing)    BG (action selection)
       ↑
  PPC + S1 (sensory feedback)
```

---

## 2. M1 (Primary Motor Cortex)

### 2.1 Motor Homunculus

Penfield 1937: M1 surface has body map:
- Large areas: hand / face (fine control)
- Small areas: trunk / leg

### 2.2 Output

Layer 5 pyramidals → corticospinal tract → spinal motor neurons.

### 2.3 Population Vector

Georgopoulos 1986: single neurons have preferred directions; population vector encodes movement direction.

Inspired BCI: decode intended movement from M1 population.

---

## 3. Premotor / SMA

- **Premotor**: receives PPC + visual input, plans reach trajectory
- **SMA**: internally triggered actions, motor sequence
- **dlPFC**: high-level goals

---

## 4. Cerebellum

Participates in:
- Timing (smooth movement)
- Motor learning (adaptation)
- Forward model: predicts sensory consequences of motor command
- Damage → ataxia

---

## 5. Basal Ganglia

- Action selection
- Habit formation
- Reward-based learning

---

## 6. Feedback Control

```
Motor command → muscle → movement → sensors (proprioception, vision)
                                              ↓
                       Compared to predicted (forward model)
                                              ↓
                       Error → corrective adjustment
```

→ Internal Model Theory (Kawato 1999).

---

## 7. BCI Applications

Decode intended movement from M1 → control robotic arm / cursor:
- Utah array (96 channels) 1990s
- BrainGate clinical 2006+
- Neuralink 1024 channels 2024

---

## 8. PyTorch — Population Coding

```python
import torch

class M1Population:
    def __init__(self, n_neurons=100, preferred_directions=None):
        self.n = n_neurons
        if preferred_directions is None:
            self.pds = torch.linspace(0, 2 * torch.pi, n_neurons + 1)[:-1]
        else:
            self.pds = preferred_directions
    
    def encode(self, direction, noise=0.1):
        responses = torch.cos(direction - self.pds)
        responses = torch.relu(responses) + noise * torch.randn(self.n)
        return responses
    
    def decode(self, responses):
        x = (responses * torch.cos(self.pds)).sum()
        y = (responses * torch.sin(self.pds)).sum()
        return torch.atan2(y, x)
```

---

## 9. Pathology

- **Stroke (M1)**: contralateral paralysis
- **ALS**: motor neuron degeneration
- **Spinal cord injury**: cuts cortex → muscle path
- **Parkinson**: BG degeneration → difficulty initiating
- **Cerebellar ataxia**: incoordination

---

## 10. Common Pitfalls

### 10.1 M1 ≠ All Motor

Subcortical (BG, cerebellum, brainstem) essential.

### 10.2 Population Coding Not Unique

Rate, timing, dynamics all participate.

### 10.3 Reflex vs Voluntary

Knee jerk reflex doesn't go through cortex; voluntary motion does.

### 10.4 Open-loop vs Closed-loop

Fine motion heavily depends on feedback; fast motions (piano playing) partly open-loop.

### 10.5 BCI Output Hard

Hand reach decoding ~90%, but fine finger / speech difficult.

---

## 11. Related Concepts

- **Same section**: [Visual System](Visual_System.en.md), [Hippocampus + Memory](Hippocampus_Memory.en.md)
- **Anatomy**: [Cortex](../01_Neuroanatomy/Cortex.en.md), [Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.en.md), [Cerebellum](../01_Neuroanatomy/Cerebellum.en.md)

---

## References

1. **Georgopoulos, A. P. et al.** "Neural coding of movement direction." *Science*, 1986.
2. **Penfield, W. & Boldrey, E.** "Somatic motor and sensory representation in the cerebral cortex." *Brain*, 1937.
3. **Kawato, M.** "Internal models for motor control and trajectory planning." *Curr Opin Neurobiol*, 1999.
4. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
