# Nervous System Overview

> *The nervous system divides into CNS (brain + spinal cord) and PNS (cranial + spinal nerves + autonomic). CNS ~ 86 billion neurons. PNS splits into somatic (voluntary) + autonomic (sympathetic / parasympathetic / enteric). This is the "map" of neuroscience, the aerial view needed before any specific system.*
>
> **Difficulty**: Beginner
> **Prerequisites**: none

---

## 1. Top-Level Division

```
Nervous System
├── CNS (Central)
│   ├── Brain
│   └── Spinal cord
└── PNS (Peripheral)
    ├── Somatic (voluntary)
    └── Autonomic (involuntary)
        ├── Sympathetic ("fight or flight")
        ├── Parasympathetic ("rest and digest")
        └── Enteric ("gut brain")
```

---

## 2. CNS — Brain Major Regions

| Region | Function |
|---|---|
| Cerebrum (cortex) | Higher cognition, sensory, motor |
| Cerebellum | Motor coordination, timing, learning |
| Brainstem | Vital centers (breathing, heartbeat) |
| Diencephalon (thalamus, hypothalamus) | Relay, homeostasis |
| Limbic system | Emotion, memory |
| Basal ganglia | Motor selection, habits |

---

## 3. CNS — Spinal Cord

- 31 segments (8C + 12T + 5L + 5S + 1Co)
- Gray matter (central H) + white matter (peripheral axon tracts)
- Reflex arc bypasses brain
- Ascending (sensory) + descending (motor) tracts

---

## 4. PNS — Somatic

- Controls skeletal muscle (voluntary)
- Sensory (afferent) + motor (efferent)
- 12 pairs cranial nerves + 31 pairs spinal nerves

---

## 5. PNS — Autonomic

| | Sympathetic | Parasympathetic |
|---|---|---|
| Function | Fight/flight | Rest/digest |
| Origin | Thoracolumbar | Craniosacral |
| NT | NE (post) | ACh |
| Heart rate | ↑ | ↓ |
| Pupil | Dilate | Constrict |
| Digestion | Inhibit | Promote |

**Enteric**: gut nervous system, ~ 500 million neurons, semi-autonomous ("second brain").

---

## 6. Orders of Magnitude

| Item | Count |
|---|---|
| Brain neurons | ~ 86 billion |
| Glia | ~ same order as neurons |
| Synapses | ~ 10^14-10^15 |
| Cortex neurons | ~ 16 billion |
| Cerebellum neurons | ~ 69 billion (most!) |
| Spinal cord neurons | ~ 1 billion |

---

## 7. Directional Terms

- **Rostral / Caudal**: head / tail
- **Dorsal / Ventral**: back / belly
- **Medial / Lateral**: middle / side
- **Superior / Inferior**: above / below
- **Afferent / Efferent**: incoming / outgoing
- **Ipsilateral / Contralateral**: same side / opposite side

---

## 8. PyTorch — System Hierarchy Tree

```python
nervous_system = {
    "CNS": {
        "brain": ["cortex", "cerebellum", "brainstem", "thalamus", "limbic", "BG"],
        "spinal_cord": ["cervical", "thoracic", "lumbar", "sacral"]
    },
    "PNS": {
        "somatic": ["cranial_nerves", "spinal_nerves"],
        "autonomic": ["sympathetic", "parasympathetic", "enteric"]
    }
}

def count_leaves(d):
    if isinstance(d, list): return len(d)
    return sum(count_leaves(v) for v in d.values())

print(count_leaves(nervous_system))  # number of system components
```

---

## 9. Developmental Origin

- Neural tube → CNS
- Neural crest → PNS + some others
- 3 primary vesicles → 5 secondary → adult brain
  - Prosencephalon → telencephalon + diencephalon
  - Mesencephalon → midbrain
  - Rhombencephalon → pons/cerebellum + medulla

---

## 10. Common Pitfalls

### 10.1 Cerebrum Has Most Neurons

Wrong; cerebellum ~ 69B, far more than cortex ~ 16B.

### 10.2 Autonomic = Uncontrollable

Partially trainable (biofeedback, breathing modulates heart rate).

### 10.3 Spinal Cord Only Conducts

Has reflexes + local pattern generators (motor rhythm).

### 10.4 Enteric Controlled by Brain

Largely autonomous; vagus only modulates.

### 10.5 Left Brain Controls Left Body

Wrong; mostly contralateral (left brain → right body).

---

## 11. Related Concepts

- **Same section**: [Neuroscience History](Neuroscience_History.en.md), [Research Methods](Research_Methods.en.md)
- **Anatomy**: [Cortex](../01_Neuroanatomy/Cortex.en.md), [Brainstem](../01_Neuroanatomy/Brainstem.en.md), [Cerebellum](../01_Neuroanatomy/Cerebellum.en.md), [Thalamus](../01_Neuroanatomy/Thalamus.en.md)

---

## References

1. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
2. **Purves, D. et al.** *Neuroscience*. 6th ed., 2018.
3. **Herculano-Houzel, S.** "The human brain in numbers." *Front Hum Neurosci*, 2009.
4. **Gray, H.** *Gray's Anatomy*. 42nd ed., 2020.
