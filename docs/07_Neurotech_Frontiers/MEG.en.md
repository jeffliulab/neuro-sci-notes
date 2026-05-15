# Magnetoencephalography (MEG)

> *MEG measures the extremely weak magnetic fields from neural currents (~ fT, a billionth of geomagnetic). Advantages: ms temporal resolution + better spatial localization than EEG (magnetic fields not distorted by skull). Needs SQUID + magnetic shielding (expensive). OPM-MEG (optically pumped magnetometer, 2018+) is a wearable revolution. Mainstay for source localization + oscillation research.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [EEG](EEG.en.md), [Brain Rhythms](../00_Foundations/Brain_Rhythms.en.md)

---

## 1. Principle

- Neuronal postsynaptic currents → weak magnetic field (Biot-Savart)
- Main source: pyramidal cells in **sulcal walls** (tangential currents, magnetically measurable)
- Magnitude ~ 10-1000 fT (femtoTesla)
- Detectors: SQUID (superconducting quantum interference device, needs liquid helium) or OPM

---

## 2. MEG vs EEG

| | MEG | EEG |
|---|---|---|
| Measures | Magnetic field | Electric potential |
| Skull effect | Almost none (magnetically transparent) | Strong distortion/spread |
| Spatial localization | Better (~ mm-cm) | Poorer |
| Sensitive source | Tangential (sulcal) | Tangential+radial |
| Reference electrode | Not needed (absolute) | Needed (relative) |
| Portable | Traditionally no (OPM yes) | Yes |

---

## 3. Inverse Problem (Source Localization)

- Inferring intracranial current sources from surface fields = **ill-posed inverse problem** (no unique solution)
- Methods:
  - **Dipole fitting** (equivalent current dipole)
  - **Beamforming** (LCMV)
  - **Minimum norm estimate (MNE)**
  - **MUSIC**
- Needs head model (MRI co-registration) + prior constraints

---

## 4. PyTorch — Minimum Norm Inverse (simplified)

```python
import torch

def minimum_norm_estimate(B, leadfield, reg=1e-3):
    """B: (n_sensors,) measured field; L: (n_sensors, n_sources) leadfield."""
    L = leadfield
    # MNE: J = L^T (L L^T + reg I)^-1 B  (Tikhonov-regularized)
    LLt = L @ L.t()
    inv = torch.linalg.solve(LLt + reg * torch.eye(LLt.size(0)), B)
    J = L.t() @ inv          # estimated source currents
    return J
```

---

## 5. OPM-MEG Revolution

- **Optically Pumped Magnetometer**: no liquid helium needed, room temperature
- Can be placed on scalp (closer to source → ↑ signal)
- Wearable + allows head movement → natural behavior, children, clinical
- Rapid development since 2018 (trend to replace SQUID)

---

## 6. Applications

- **Epilepsy**: epileptogenic focus localization (presurgical) — main clinical use
- **Cognitive neuroscience**: language, attention, memory spatiotemporal dynamics
- **Oscillations**: gamma/beta (motor), theta — better localization than EEG
- **MEG-fMRI fusion**: time (MEG) + space (fMRI)

---

## 7. Advantages Summary

- ms temporal resolution (same as EEG)
- Spatial localization better than EEG (fields not tissue-distorted)
- No reference electrode, no electrode gel
- Directly measures neural currents (unlike fMRI's indirect blood oxygen)

---

## 8. Limitations

- Device very expensive + magnetic shielding room (traditional SQUID)
- Insensitive to **radial** sources (gyral crown vs sulcal wall)
- Weak signal for deep sources (rapid decay with distance)
- Inverse problem non-unique (needs constraints + assumptions)
- Motion-sensitive (traditional; OPM improves)

---

## 9. Relation to AI

- Source localization = ill-posed inverse problem → deep learning reconstruction (like MRI reconstruction)
- MEG decoding (decoding perception/language) + Transformer
- Real-time BCI (OPM wearable → new possibilities)

---

## 10. Common Pitfalls

### 10.1 MEG = Magnetic EEG

Complementary but sensitive to different sources (MEG tangential, EEG also radial).

### 10.2 Spatial Localization Precise and Unique

Inverse problem non-unique; depends on head model + priors.

### 10.3 Measures All Neurons

Mainly tangential sulcal pyramidal cell synchronous activity; deep weak.

### 10.4 SQUID Is the Only Way

OPM-MEG room-temperature wearable is replacing it.

### 10.5 No MRI Needed

Source localization needs MRI anatomical co-registration (head model).

---

## 11. Related Concepts

- **Same section**: [EEG](EEG.en.md), [fMRI BOLD](fMRI_BOLD.en.md), [TMS](TMS.en.md)
- **Foundation**: [Brain Rhythms](../00_Foundations/Brain_Rhythms.en.md), [Research Methods](../00_Foundations/Research_Methods.en.md)
- **Disease**: [Epilepsy](../08_Neuro_Disorders/Epilepsy.en.md)

---

## References

1. **Hämäläinen, M. et al.** "Magnetoencephalography—theory, instrumentation, and applications." *Rev Mod Phys*, 1993.
2. **Baillet, S.** "Magnetoencephalography for brain electrophysiology and imaging." *Nat Neurosci*, 2017.
3. **Boto, E. et al.** "Moving magnetoencephalography towards real-world applications with a wearable system (OPM)." *Nature*, 2018.
4. **Gross, J. et al.** "Good practice for conducting and reporting MEG research." *NeuroImage*, 2013.
