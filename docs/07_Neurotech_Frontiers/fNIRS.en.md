# Functional Near-Infrared Spectroscopy (fNIRS)

> *fNIRS uses near-infrared light (~ 700-900 nm) to measure cortical blood oxygenation changes (like fMRI but portable, cheap, motion-tolerant). Based on oxy/deoxy-Hb absorption difference. Penetrates ~ 1-3 cm (cortex only). Advantages: natural settings, infants, wearable, BCI; limitations: shallow, low spatial resolution. Between EEG and fMRI.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [fMRI BOLD](fMRI_BOLD.en.md), [Brain Energy Metabolism](../00_Foundations/Brain_Energy_Metabolism.en.md)

---

## 1. Principle

- Near-infrared "optical window" (~ 700-900 nm): low water/tissue absorption, Hb absorption dominant
- **oxy-Hb** and **deoxy-Hb** have different absorption spectra
- Neural activity → local blood flow ↑ → oxy-Hb ↑ / deoxy-Hb ↓ (same basis as BOLD)
- Source + detector pair (~ 3 cm spacing) → banana-shaped light path probes cortex

---

## 2. Modified Beer-Lambert Law

$$\Delta A = \varepsilon \cdot \Delta c \cdot d \cdot \text{DPF}$$

- $\Delta A$: absorbance change
- $\varepsilon$: extinction coefficient (oxy/deoxy differ)
- DPF: differential pathlength factor (scattering lengthens path)
- Dual wavelength → solve oxy/deoxy-Hb concentration changes

---

## 3. vs fMRI / EEG

| | fNIRS | fMRI | EEG |
|---|---|---|---|
| Signal | Blood oxygen (optical) | Blood oxygen (magnetic) | Electric |
| Time | ~ sec | ~ sec | ms |
| Space | cm (shallow) | mm (whole brain) | cm |
| Portable | ✓ | ✗ | ✓ |
| Motion-tolerant | Better | Poor | Medium |
| Deep | ✗ (~ 3 cm) | ✓ | ✗ |
| Cost | Low | Very high | Low |

---

## 4. PyTorch — Dual-Wavelength Hb Solve

```python
import torch

def fnirs_hb(dA_lambda1, dA_lambda2, eps, dpf, d):
    """Solve Δ[HbO], Δ[HbR] from two wavelengths (modified Beer-Lambert)."""
    # eps: 2x2 extinction matrix [[e_HbO_l1, e_HbR_l1],[e_HbO_l2, e_HbR_l2]]
    dA = torch.tensor([dA_lambda1, dA_lambda2]) / (dpf * d)
    dC = torch.linalg.solve(eps, dA)   # [ΔHbO, ΔHbR]
    return dC
```

---

## 5. Advantageous Use Cases

- **Infants/children**: fMRI hard to cooperate, fNIRS works
- **Natural behavior**: walking, social, exercise (fMRI impossible)
- **Bedside / clinical**: neonatal monitoring, intraoperative
- **BCI**: hemodynamic BCI (slow but noise-tolerant)
- **Hyperscanning**: two-person simultaneous (social neuroscience)

---

## 6. Signal Processing

- **Motion artifacts**: though better than fMRI still needs correction (spline, wavelet)
- **Physiological noise**: heartbeat, respiration, Mayer waves → filter / short-channel regression
- **Short-separation channel**: measures scalp blood flow → regress out superficial contamination
- GLM (like fMRI) analysis

---

## 7. Limitations

- Cortex only (~ 1-3 cm) → deep inaccessible
- Low spatial resolution (light scattering)
- Hair / skin tone affect signal (light attenuation) — device equity issue
- Superficial (scalp blood flow) contamination
- Hard absolute quantification (DPF individual variation)

---

## 8. Device Types

- **CW** (continuous wave): most common, measures intensity change
- **Frequency-domain**: + phase → partial absolute quantification
- **Time-domain**: photon time-of-flight → best depth discrimination (expensive)
- **Wearable / high-density DOT**: approaching imaging (diffuse optical tomography)

---

## 9. Relation to AI

- BCI decoding (slow hemodynamic + ML)
- Motion artifact removal (deep learning)
- Hyperscanning social synchrony analysis
- Wearable + natural settings → real-world neuroscience

---

## 10. Common Pitfalls

### 10.1 fNIRS = Portable fMRI

Cortex only + low spatial; not whole-brain equivalent.

### 10.2 Directly Measures Neural Activity

Indirect (blood oxygen), ~ sec delay, same BOLD limitation.

### 10.3 Fully Motion-Proof

Better than fMRI but still needs motion correction.

### 10.4 Signal Purely Brain Source

Scalp blood flow contamination significant; needs short-channel regression.

### 10.5 Skin Tone/Hair No Effect

Significantly affect signal quality → device inclusivity issue (recent concern).

---

## 11. Related Concepts

- **Same section**: [fMRI BOLD](fMRI_BOLD.en.md), [EEG](EEG.en.md)
- **Foundation**: [Brain Energy Metabolism](../00_Foundations/Brain_Energy_Metabolism.en.md)
- **BCI**: [BCI overview](../06_Brain_Computer_Interface/index.md)

---

## References

1. **Jöbsis, F. F.** "Noninvasive, infrared monitoring of cerebral and myocardial oxygen sufficiency." *Science*, 1977.
2. **Scholkmann, F. et al.** "A review on continuous wave functional NIRS." *NeuroImage*, 2014.
3. **Pinti, P. et al.** "The present and future use of functional near-infrared spectroscopy (fNIRS) for cognitive neuroscience." *Ann NY Acad Sci*, 2020.
4. **Kwasa, J. et al.** "Demographic reporting and phenotypic exclusion in fNIRS." *Front Neurosci*, 2023.
