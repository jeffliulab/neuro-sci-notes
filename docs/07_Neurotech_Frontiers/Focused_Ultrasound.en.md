# Focused Ultrasound Neuromodulation

> *Focused ultrasound (FUS) is the only technology offering **non-invasive + deep + high spatial precision** brain modulation. Low intensity → reversible neuromodulation (mechanosensitive ion channels); high intensity → thermal ablation (MRgFUS, FDA-approved for essential tremor). + microbubbles → temporarily open BBB for drug delivery. One of the most promising neuromodulation frontiers.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [TMS](TMS.en.md), [Blood Brain Barrier](../00_Foundations/Blood_Brain_Barrier.en.md)

---

## 1. Why Unique

| Technique | Deep | Spatial precision | Non-invasive |
|---|---|---|---|
| TMS / tDCS | ✗ (cortex only) | Poor-medium | ✓ |
| DBS | ✓ | High | ✗ (implant) |
| **FUS** | ✓ | **High (mm)** | ✓ |

→ FUS fills the "non-invasive deep precise" gap.

---

## 2. Intensity Grades

| Intensity | Effect | Application |
|---|---|---|
| Low (LIFU) | Reversible neuromodulation | Research, depression, pain |
| Moderate + microbubbles | Temporary BBB opening | Drug delivery (chemo, gene) |
| High (HIFU) | Thermal ablation (irreversible) | Essential tremor, PD tremor (MRgFUS) |

---

## 3. Neuromodulation Mechanism (LIFU)

- Mainly **mechanical** (not thermal): acoustic radiation force + cavitation
- Activates **mechanosensitive ion channels** (Piezo1, TRP, K2P)
- Bidirectional (parameter-dependent): excitation or inhibition
- Mechanism still partly unclear (controversy: direct vs indirect auditory confound)

---

## 4. MRgFUS — Clinical Ablation

- MRI-guided + real-time thermometry
- Hundreds-thousands transducer array focused through skull (phase correction for skull distortion)
- FDA-approved: essential tremor (Vim thalamus), PD tremor
- No incision, no implant → alternative to some DBS / lesioning

---

## 5. BBB Opening (FUS + Microbubbles)

- IV microbubbles + FUS → microbubble oscillation mechanically loosens tight junctions
- Temporary (hours) + local + reversible BBB opening
- Delivery: chemo, antibodies, AAV, ASO into brain
- AD, brain tumor clinical trials ongoing (see [Blood Brain Barrier](../00_Foundations/Blood_Brain_Barrier.en.md))

---

## 6. PyTorch — Acoustic Field Focusing (simplified)

```python
import numpy as np

def fus_focus(N=128, focal_depth=50, wavelength=2.0):
    """Phased array: per-element delay to focus at depth (skull-naive)."""
    elements = np.linspace(-30, 30, N)              # transducer positions
    dist = np.sqrt(elements**2 + focal_depth**2)
    delays = (dist.max() - dist) / wavelength       # phase delays to align
    # Constructive interference at focal point -> mm-scale spot
    return elements, delays
```

---

## 7. Advantages + Limitations

| Pro | Limitation |
|---|---|
| Non-invasive + deep + mm precision | Skull attenuation/distortion (needs phase correction) |
| Reversible (LIFU) or permanent (HIFU) selectable | LIFU mechanism debated |
| Can open BBB for delivery | Device expensive, technically complex |
| No implant infection risk | Auditory confound (animals) |

---

## 8. Application Prospects

- Neuromodulation: depression, OCD, pain, addiction (deep targets, non-invasive)
- Ablation: tremor, PD, refractory epilepsy
- Drug delivery: brain tumor, AD (Aβ clearance + antibody delivery)
- "Sonogenetics": + engineered mechanosensitive proteins → optogenetics-like but non-invasive

---

## 9. Safety

- Thermal damage (HIFU intentional; LIFU must avoid)
- Hemorrhage (excessive BBB opening → microbleeds)
- Cavitation uncontrolled
- Skull heating
- Still needs long-term safety data (esp. repeated LIFU)

---

## 10. Common Pitfalls

### 10.1 FUS = Ablation Only

Low-intensity reversible neuromodulation + BBB opening are major frontiers.

### 10.2 LIFU Mechanism Clear

Mechanosensitive hypothesis mainstream but still debated (auditory confound).

### 10.3 BBB Opening Risk-Free

Excessive → microbleeds / edema; parameters + microbubble dose critical.

### 10.4 Skull No Effect

Skull strongly attenuates + distorts; phase correction (CT-based) essential.

### 10.5 Replaces DBS

Complementary: FUS ablation irreversible, non-adjustable; DBS adjustable, reversible.

---

## 11. Related Concepts

- **Same section**: [DBS](DBS.en.md), [TMS](TMS.en.md), [Optogenetics](Optogenetics.en.md)
- **Foundation**: [Blood Brain Barrier](../00_Foundations/Blood_Brain_Barrier.en.md)
- **Disease**: [Parkinson](../08_Neuro_Disorders/Parkinson.en.md), [Alzheimer](../08_Neuro_Disorders/Alzheimer.en.md)

---

## References

1. **Tyler, W. J. et al.** "Ultrasonic modulation of neural circuit activity." *Neuron*, 2008.
2. **Elias, W. J. et al.** "A randomized trial of focused ultrasound thalamotomy for essential tremor." *NEJM*, 2016.
3. **Hynynen, K. et al.** "Noninvasive MR imaging-guided focal opening of the blood-brain barrier." *Radiology*, 2001.
4. **Blackmore, J. et al.** "Ultrasound neuromodulation: a review of results, mechanisms and safety." *Ultrasound Med Biol*, 2019.
