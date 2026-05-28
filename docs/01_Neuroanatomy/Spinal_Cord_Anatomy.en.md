# Spinal Cord Anatomy

> *Spinal cord: continuation of medulla to L1-2 (adult), 31 segments. Cross-section: central butterfly gray matter (dorsal/lateral/ventral horns) + peripheral white matter (dorsal/lateral/ventral columns). Ascending (sensory) + descending (motor) tracts have precise spatial arrangement → injury localization. This article focuses on **structure**; circuits/CPG in [Spinal_Cord_Systems](../03_Systems_Neuroscience/Spinal_Cord_Systems.en.md).*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Brainstem](Brainstem.en.md), [Nervous System Overview](../00_Foundations/Nervous_System_Overview.en.md)

---

## 1. Gross Anatomy

- Continues from medulla, ends at **conus** (adult L1-2) → filum terminale + cauda equina
- 31 segments: 8C+12T+5L+5S+1Co
- Two enlargements: cervical (upper limb), lumbosacral (lower limb)
- Coverings: spinal meninges (same three meningeal layers) + epidural space (fat/venous plexus)

---

## 2. Cross-Section

```
        Dorsal horn (sensory afferent)
       /  Rexed laminae I-X
Dorsal col ─ Gray matter (butterfly/H) ─ Lateral col
       \ Ventral horn (motor neurons → muscle)
        Central canal (ependyma, CSF)
Periphery: white matter (myelinated tracts)
```

Gray matter: dorsal horn (sensory), intermediolateral column (T1-L2 sympathetic), ventral horn (α/γ motor neurons, somatotopically arranged).

---

## 3. Major Tracts (Spatial Arrangement)

| Tract | Location | Direction/function |
|---|---|---|
| Dorsal columns (gracile/cuneate) | Dorsal column | Ascending: fine touch/proprioception (ipsilateral, decussate in medulla) |
| Spinothalamic | Anterolateral | Ascending: pain/temp (decussate in spinal cord) |
| Corticospinal (lateral) | Lateral column | Descending: voluntary movement (decussated in medulla) |
| Spinocerebellar | Around lateral column | Ascending: proprioception → cerebellum |

→ Precise arrangement → injury pattern localization (see [Spinal_Cord_Systems](../03_Systems_Neuroscience/Spinal_Cord_Systems.en.md)).

---

## 4. PyTorch — Lesion Level Localization

```python
def localize_lesion(deficit):
    rules = {
     "ipsilateral motor+proprioception, contralateral pain/temp": "Brown-Sequard (hemisection)",
     "bilateral pain/temp loss, motor spared (cape)": "central cord (syrinx)",
     "bilateral motor + pain/temp below, proprioception spared": "anterior cord",
     "all modalities below a level": "complete transection",
    }
    return rules.get(deficit, "see tract layout")
```

---

## 5. Spinal Nerves + Segments

- Each segment: dorsal root (sensory, has dorsal root ganglion) + ventral root (motor) → combine into spinal nerve
- **Dermatome**: each segment's sensory skin area (clinical localization of zoster/injury)
- **Myotome**: muscle group innervated per segment
- Nerve plexuses: cervical/brachial/lumbosacral (recombination)

---

## 6. Vertebral Mismatch

- Spinal cord shorter than vertebral canal (developmental speed difference) → segment ≠ same-numbered vertebra (especially lower)
- Cauda equina: only nerve roots below L2 → lumbar puncture safe zone (see [Meninges_Ventricles](Meninges_Ventricles.en.md))
- Disc herniation → compress nerve root (radicular pain/weakness)

---

## 7. Reflex Arc Anatomy

- Monosynaptic (stretch): Ia → ventral horn α motor neuron
- Polysynaptic (flexor withdrawal)
- Intra-segmental + inter-segmental (association tracts)
- See [Spinal_Cord_Systems](../03_Systems_Neuroscience/Spinal_Cord_Systems.en.md) (function)

---

## 8. Clinical Injury Syndromes

| Injury | Anatomy | Presentation |
|---|---|---|
| Complete transection | All severed | Complete loss + autonomic below level |
| Brown-Séquard | Hemisection | Ipsilateral motor/proprioception, contralateral pain/temp |
| Anterior cord | Anterior 2/3 | Motor+pain/temp loss, proprioception spared |
| Central cord (syrinx) | Around central canal | Segmental pain/temp ("cape") |
| Posterior cord | Dorsal column | Proprioception/vibration loss (sensory ataxia) |
| ALS | Anterior horn+corticospinal | Upper+lower motor neuron (see [ALS](../08_Neuro_Disorders/ALS.en.md)) |

---

## 9. Relation to AI / Engineering

- Precise tract arrangement = modular wiring + topological diagnosis (injury pattern → localization)
- Gray/white separation = computation (gray) vs communication (white) separation
- Spinal cord = brain's "peripheral bus + edge controller"

---

## 10. Common Pitfalls

### 10.1 Spinal Cord Extends to Sacrum

Adult ends L1-2; below is cauda equina (basis of lumbar puncture safety).

### 10.2 Segment = Same-Numbered Vertebra

Spinal cord shorter than canal; lower segments markedly offset.

### 10.3 Pain/Temp and Proprioception Ascend Same Side

Pain/temp decussate in spinal cord; proprioception decussate in medulla (key to injury patterns).

### 10.4 Cross-Section Tracts Random

Precise spatial arrangement → highly stereotyped syndromes.

### 10.5 Spinal Cord Only Conducts (Anatomically)

Gray matter contains motor neurons/interneurons/autonomic columns (computation + reflex).

---

## 11. Related Concepts

- **Same section**: [Brainstem](Brainstem.en.md), [White_Matter_Tracts](White_Matter_Tracts.en.md), [Cerebral_Vasculature](Cerebral_Vasculature.en.md)
- **Systems**: [Spinal_Cord_Systems](../03_Systems_Neuroscience/Spinal_Cord_Systems.en.md), [Somatosensory](../03_Systems_Neuroscience/Somatosensory.en.md), [Motor_System](../03_Systems_Neuroscience/Motor_System.en.md)
- **Disease**: [ALS](../08_Neuro_Disorders/ALS.en.md)

---

## References

1. **Standring, S.** *Gray's Anatomy*. 42nd ed., 2020.
2. **Blumenfeld, H.** *Neuroanatomy through Clinical Cases*. 2nd ed., 2010.
3. **Watson, C. et al.** *The Spinal Cord: A Christopher and Dana Reeve Foundation Text and Atlas*. 2009.
4. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
