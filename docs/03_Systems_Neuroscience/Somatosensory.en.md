# Somatosensory System

> *Somatosensory system processes touch, proprioception, temperature, pain. Skin → DRG → spinal cord → thalamus VPL → S1 cortex is the hierarchy. Penfield homunculus shows S1 has spatial mapping. Discriminative touch and pain split into two pathways. Robotic tactile sensors aim to mimic this system.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Cortex](../01_Neuroanatomy/Cortex.en.md), [Motor System](Motor_System.en.md)

---

## 1. Five Submodalities

- **Discriminative touch** (light touch, vibration, pressure)
- **Proprioception** (joint position, muscle stretch)
- **Temperature** (warm, cold)
- **Pain** (nociception)
- **Itch**

---

## 2. Skin Receptors

| Receptor | Type | Adaptation |
|---|---|---|
| Meissner corpuscle | Light touch | Fast adapting (FA) |
| Pacinian corpuscle | Vibration (high freq) | FA |
| Merkel cell | Static pressure / shape | Slow adapting (SA) |
| Ruffini ending | Stretch / slip | SA |
| Free nerve endings | Pain / temperature | Variable |

---

## 3. Pathway — Discriminative Touch (DCML)

```
Skin → DRG → ipsilateral ascending
              (Dorsal Column)
              ↓
         Medulla (gracile, cuneate)
              ↓ decussation
              ↓
         Thalamus VPL
              ↓
         S1 cortex (postcentral gyrus)
```

DCML = Dorsal Column - Medial Lemniscus.

---

## 4. Pathway — Pain (Spinothalamic)

```
Skin → DRG → spinal cord
              ↓ immediate decussation (anterior commissure)
              ↓ ascending lateral column
              ↓
         Thalamus VPL + intralaminar
              ↓
         S1 + ACC + insula
```

→ Differences from DCML: immediate crossing, slow, includes emotional component (ACC).

---

## 5. S1 Cortex

- Brodmann areas 3a, 3b, 1, 2 (postcentral gyrus)
- 4 stripes each specialize:
  - 3a: proprioception
  - 3b: cutaneous touch
  - 1: texture
  - 2: shape / grip
- Adjacent to motor cortex (M1, area 4)

---

## 6. Penfield Homunculus

- 1937 Penfield intracranial mapping
- Body parts proportional to sensory innervation:
  - Lips, hands huge
  - Trunk small
- Mirror in M1 (motor homunculus)

---

## 7. Receptor Fibers — Touch + Pain

| Fiber | Diameter | Velocity | Modality |
|---|---|---|---|
| **Aα (Ia)** | Large | 70-120 m/s | Muscle spindle, proprioception |
| **Aβ** | Medium-large | 30-70 m/s | Touch |
| **Aδ** | Small | 5-30 m/s | Sharp pain / cold |
| **C** | Tiny, unmyelinated | 0.5-2 m/s | Dull pain / warm / itch |

---

## 8. Gate Control Theory (Melzack & Wall 1965)

- "Gate" in spinal dorsal horn
- Aβ (touch) → closes gate → inhibits pain
- C (pain) → opens gate
- Explains rubbing sore spot reduces pain
- TENS, acupuncture partially based

---

## 9. PyTorch — Somatosensory Skin Sim

```python
import torch

class TactileSensor(torch.nn.Module):
    """Simulated skin patch with FA + SA receptors."""
    def __init__(self, grid_size=10):
        super().__init__()
        self.grid_size = grid_size
    
    def forward(self, pressure, dt=0.01):
        # pressure: (T, H, W)
        SA = pressure  # sustained
        FA = torch.diff(pressure, dim=0) / dt  # derivative
        FA = torch.cat([torch.zeros_like(pressure[:1]), FA])
        return SA, FA  # combined response
```

---

## 10. Pathology

- **Phantom limb**: post-amputation limb sensation (cortical re-mapping)
- **Allodynia**: light touch causes severe pain
- **Hyperalgesia**: pain sensitization
- **Tabes dorsalis**: late syphilis → DC degeneration → ataxia
- **Brown-Séquard**: half spinal cord cut → ipsilateral DCML loss, contralateral pain loss
- **Capsaicin desensitization**: chili reduces pain
- **Diabetic neuropathy**: small fiber disease

---

## 11. Robotic / AI Connection

- **e-skin**: multi-modal receptor simulation
- **GelSight** / DIGIT: visuotactile (optics + elastomer)
- **Tactile RL**: blind grasp training
- **Optimus, Figure**: finger force sensors

---

## 12. Common Pitfalls

### 12.1 Pain = Nociception

Not quite; pain has emotional + cognitive layers.

### 12.2 Homunculus Static

Brain plasticity → maps reorganize after amputation.

### 12.3 C Fiber = Pain Only

C carries itch, temperature, tickle too.

### 12.4 Gate Control Oversimplified

Melzack himself later expanded to neuromatrix theory.

### 12.5 Tactile = Touch Only

Touch + proprioception + pain + temperature all distinct.

---

## 13. Related Concepts

- **Same section**: [Visual System](Visual_System.en.md), [Motor System](Motor_System.en.md), [Auditory System](Auditory_System.en.md)
- **Anatomy**: [Cortex](../01_Neuroanatomy/Cortex.en.md)
- **AI**: Tactile sensing

---

## References

1. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
2. **Penfield, W. & Boldrey, E.** "Somatic motor and sensory representation in the cerebral cortex of man." *Brain*, 1937.
3. **Melzack, R. & Wall, P.** "Pain mechanisms: A new theory." *Science*, 1965.
4. **Johansson, R. S. & Flanagan, J. R.** "Coding and use of tactile signals from the fingertips." *Nat Rev Neurosci*, 2009.
