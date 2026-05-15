# Positron Emission Tomography (PET)

> *PET injects a positron-emitting radiotracer; annihilation → dual 511 keV photon coincidence detection → molecular-level functional imaging. Unique advantage: **molecular specificity** (receptors, metabolism, protein deposits). FDG-PET (glucose metabolism), amyloid/tau-PET (AD), DA-PET (PD) are clinical pillars. Medium spatial resolution (~ 4 mm), needs cyclotron + radiation.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Brain Energy Metabolism](../00_Foundations/Brain_Energy_Metabolism.en.md), [fMRI BOLD](fMRI_BOLD.en.md)

---

## 1. Principle

```
Radionuclide decay → positron (β⁺)
   ↓ travels ~1-2 mm then annihilates
e⁺ + e⁻ → 2 photons of 511 keV (opposite 180°)
   ↓
Ring detector coincidence detection
   ↓
Backprojection / iterative reconstruction → 3D tracer distribution
```

---

## 2. Common Tracers

| Tracer | Measures | Application |
|---|---|---|
| **¹⁸F-FDG** | Glucose metabolism | AD (hippocampal parietal↓), tumor, epilepsy focus |
| **¹¹C-PiB / ¹⁸F-florbetapir** | Amyloid plaque | Alzheimer diagnosis |
| **¹⁸F-flortaucipir** | tau tangles | AD staging, FTD |
| **¹⁸F-DOPA / ¹¹C-raclopride** | DA synthesis / D2 | Parkinson |
| **¹⁵O-water** | Cerebral blood flow | Activation studies (old fMRI predecessor) |
| **Receptor PET** | 5-HT/μ-opioid/GABA | Psychopharmacology |

---

## 3. Nuclides + Half-Life

| Nuclide | Half-life | Source |
|---|---|---|
| ¹⁸F | 110 min | Cyclotron (transportable) |
| ¹¹C | 20 min | On-site cyclotron |
| ¹⁵O | 2 min | On-site, instant use |
| ⁶⁸Ga | 68 min | Generator (no cyclotron) |

Short half-life → needs nearby cyclotron (cost + logistics).

---

## 4. Quantification

- **SUV** (standardized uptake value): semi-quantitative
- **Binding potential (BP)**: receptor availability (kinetic modeling)
- **Patlak / Logan plot**: graphical analysis for irreversible/reversible tracers
- Needs arterial input function or reference region

---

## 5. PyTorch — Line Of Response (LOR) Simplified

```python
import torch

def coincidence_backproject(detector_hits, n_pixels=64):
    """Each coincidence pair defines a Line Of Response; backproject."""
    img = torch.zeros(n_pixels, n_pixels)
    for (d1, d2) in detector_hits:           # detector index pairs
        # Simplified: deposit along straight line between detectors
        ys = torch.linspace(d1[0], d2[0], n_pixels).long()
        xs = torch.linspace(d1[1], d2[1], n_pixels).long()
        img[ys, xs] += 1
    return img   # iterative recon (OSEM) refines this in practice
```

---

## 6. vs fMRI / SPECT

| | PET | fMRI | SPECT |
|---|---|---|---|
| Signal | Molecular tracer | Blood oxygen | Single-photon tracer |
| Specificity | Molecular (receptor/protein) | Blood flow proxy | Molecular (coarser) |
| Spatial | ~ 4 mm | ~ 2 mm | ~ 8 mm |
| Temporal | min | sec | min |
| Radiation | Yes | No | Yes |

→ PET's irreplaceable advantage: imaging **specific molecules** (amyloid/tau/receptors).

---

## 7. AD Diagnostic Revolution

- **Amyloid-PET**: in-vivo confirmation of Aβ deposition (once autopsy-only)
- **Tau-PET**: more correlated with cognitive decline (in-vivo Braak staging)
- Combined with CSF/plasma biomarkers → AT(N) framework (see [Alzheimer](../08_Neuro_Disorders/Alzheimer.en.md))
- Anti-Aβ drug (lecanemab) efficacy monitoring

---

## 8. Limitations

- **Ionizing radiation** (limits repetition / children / healthy controls)
- Medium spatial resolution + partial volume effect
- Poor temporal resolution (minutes)
- Expensive + cyclotron-dependent
- Tracer specificity / off-target needs validation

---

## 9. Relation to AI

- Low-dose PET reconstruction (deep learning denoising → reduce radiation)
- Amyloid/tau-PET automatic quantification + disease prediction
- PET-MRI combined (same-machine multimodal)
- Accelerated tracer kinetic parameter estimation

---

## 10. Common Pitfalls

### 10.1 PET = Slow fMRI

Core is **molecular specificity** (receptor/protein), not blood-flow speed competition.

### 10.2 Amyloid-PET Positive = AD

Aβ positive can occur in asymptomatic elderly; needs clinical + tau combination.

### 10.3 SUV Is Absolute Quantification

Semi-quantitative; receptor studies need kinetic modeling (BP).

### 10.4 No Radiation Concern

Ionizing radiation; limits repetition + healthy subjects.

### 10.5 Short Half-Life Irrelevant

¹¹C/¹⁵O need on-site cyclotron (major cost/logistics).

---

## 11. Related Concepts

- **Same section**: [fMRI BOLD](fMRI_BOLD.en.md), [DTI Tractography](DTI_Tractography.en.md)
- **Foundation**: [Brain Energy Metabolism](../00_Foundations/Brain_Energy_Metabolism.en.md)
- **Disease**: [Alzheimer](../08_Neuro_Disorders/Alzheimer.en.md), [Parkinson](../08_Neuro_Disorders/Parkinson.en.md)

---

## References

1. **Phelps, M. E.** "Positron emission tomography provides molecular imaging of biological processes." *PNAS*, 2000.
2. **Klunk, W. E. et al.** "Imaging brain amyloid in Alzheimer's disease with Pittsburgh Compound-B." *Ann Neurol*, 2004.
3. **Jack, C. R. et al.** "NIA-AA Research Framework: AT(N) biomarkers." *Alzheimers Dement*, 2018.
4. **Innis, R. B. et al.** "Consensus nomenclature for in vivo imaging of reversibly binding radioligands." *J Cereb Blood Flow Metab*, 2007.
