# Brain Organoids

> *A brain organoid is 3D miniature brain tissue self-organized from iPSC/ESC (mm scale, with layered cortical structure, multiple neurons + glia). Lancaster 2013 first cerebral organoid. Used for development, disease modeling (microcephaly, ASD, ZIKV), drug screening, organoid intelligence. Raises sentience ethics debate (see [Neuroscience Ethics](../00_Foundations/Neuroscience_Ethics.en.md)).*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Neurodevelopment](../00_Foundations/Neurodevelopment.en.md), stem cell basics

---

## 1. What It Is

- iPSC / ESC → neural induction → 3D self-organizing culture
- Forms: neuroepithelium → layered cortical-like structure + multiple neuron types + glia
- Size: ~ 1-4 mm (diffusion limit → core necrosis)
- Not a "mini complete brain": no vasculature, no sensory input, disorganized tissue

---

## 2. Types

| Type | Models |
|---|---|
| Cerebral organoid | Cortex (undirected) |
| Region-specific | Forebrain / midbrain (DA) / hippocampus / optic cup |
| **Assembloid** | Fused multiple regions (e.g., cortex+thalamus, circuit study) |
| Vascularized | + endothelium (improves core survival) |
| Sliced (ALI-CO) | Sliced → improved oxygen + axon outgrowth |

---

## 3. Key Applications

- **Development**: human-specific cortical expansion mechanism (vs mouse)
- **Disease modeling**:
  - Microcephaly (CDK5RAP2) — Lancaster's original finding
  - ZIKV → microcephaly mechanism (2016 rapid application)
  - ASD, SCZ, Rett (MECP2), Timothy syndrome
- **Drug screening / toxicology**: neurodevelopmental toxicity
- **Transplantation**: organoid into animal → integration (Pasca 2022 rat cortex integration + affects behavior)

---

## 4. PyTorch — Self-Organizing Polarization (toy)

```python
import torch

def organoid_self_org(N=50, T=100, diffusion=0.1):
    """Toy reaction-diffusion: morphogen gradient -> regional identity."""
    field = torch.zeros(N)
    field[0] = 1.0                       # morphogen source (e.g., SHH)
    for _ in range(T):
        lap = torch.zeros(N)
        lap[1:-1] = field[2:] - 2*field[1:-1] + field[:-2]
        field = field + diffusion * lap
        field = field.clamp(0, 1)
    # Threshold -> regional fate (gradient -> patterning)
    fate = (field > 0.5).long()
    return field, fate
```

---

## 5. Organoid Intelligence (OI)

- Organoid + MEA (microelectrode array) → read/write activity
- "DishBrain" (Kagan 2022): organoid learns to play Pong (controversial claim)
- "Wetware computing" vision
- Very early + high over-interpretation risk

---

## 6. vs Animal Models

| | Organoid | Animal model |
|---|---|---|
| Human-specific | ✓ | ✗ (species difference) |
| Complete circuit | ✗ (no input/output) | ✓ |
| Behavioral readout | ✗ | ✓ |
| Ethics | Lower (but sentience debate) | Higher |
| Maturity | Early fetal level | Complete |

---

## 7. Limitations

- **No vasculature** → core hypoxic necrosis (limits size + maturation)
- Maturity only ~ early fetal (no complete myelination / adult network)
- Large batch variability (reproducibility)
- No sensory input / motor output (no "experience")
- Tissue structure incompletely ordered

---

## 8. Ethics — Sentience?

- Organoids have coordinated electrical activity (oscillations — Trujillo 2019) → raises "consciousness?" question
- Consensus: currently **extremely unlikely to have sentience** (no sensation/experience/complete structure/scale)
- But proactive governance needed (chimera, transplantation, OI)
- See [Neuroscience Ethics](../00_Foundations/Neuroscience_Ethics.en.md)

---

## 9. Frontier Directions

- Vascularized / perfused → larger more mature
- Assembloid → circuits + disease (neuro-muscular, cortico-striatal)
- Transplant integration (human neurons mature in animals — Pasca)
- Multi-omics + long culture (year-scale)
- Personalized (patient iPSC → precision medicine)

---

## 10. Common Pitfalls

### 10.1 = Mini Brain

No vasculature / input / complete structure; a developmental fragment, not a "small brain."

### 10.2 Conscious

Currently extremely unlikely (scale/structure/experience missing); don't over-interpret.

### 10.3 Reproducible and Stable

Batch variability significant; needs standardization + QC.

### 10.4 Mature Like Adult Brain

Only early fetal level; long culture still doesn't reach adult network.

### 10.5 DishBrain = Organoid Thinking

Claim highly controversial; early + media-amplified.

---

## 11. Related Concepts

- **Same section**: [Calcium Imaging](Calcium_Imaging.en.md), [Neuropixels](Neuropixels.en.md)
- **Foundation**: [Neurodevelopment](../00_Foundations/Neurodevelopment.en.md), [Neuroscience Ethics](../00_Foundations/Neuroscience_Ethics.en.md)
- **Disease**: [Autism](../08_Neuro_Disorders/Autism.en.md), [Schizophrenia](../08_Neuro_Disorders/Schizophrenia.en.md)

---

## References

1. **Lancaster, M. A. et al.** "Cerebral organoids model human brain development and microcephaly." *Nature*, 2013.
2. **Qian, X. et al.** "Brain-region-specific organoids using mini-bioreactors for modeling ZIKV exposure." *Cell*, 2016.
3. **Pasca, S. P. et al.** "Maturation and circuit integration of transplanted human cortical organoids." *Nature*, 2022.
4. **Trujillo, C. A. et al.** "Complex oscillatory waves emerging from cortical organoids." *Cell Stem Cell*, 2019.
