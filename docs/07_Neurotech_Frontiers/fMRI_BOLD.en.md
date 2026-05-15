# fMRI and BOLD Signal

> *fMRI (functional MRI) is the most mainstream cognitive neuroscience tool today. Uses **BOLD (Blood-Oxygen-Level Dependent) signal** to indirectly measure brain activity. Boom since 1992; Logothetis et al. revealed BOLD ↔ neural activity relationships.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Cortex](../01_Neuroanatomy/Cortex.en.md)

---

## 1. fMRI Principle

### 1.1 MRI Basics

Strong magnetic field + RF pulse → measure tissue magnetization relaxation. Different tissues / states have different contrasts.

### 1.2 BOLD Signal

Neural activity → local oxygen demand ↑ → blood flow increases → **excess oxygen supply** → local deoxyhemoglobin ↓ → MRI T2* signal increases.

→ BOLD is a **vascular signal**, indirectly reflecting neural activity.

---

## 2. Spatiotemporal Resolution

- **Spatial**: 1-3 mm (typical), 0.5 mm (7T)
- **Temporal**: 1-3 sec (Hemodynamic response peaks at 5-7 sec)

→ Better space than EEG/MEG, worse time.

---

## 3. BOLD ↔ Neural Activity

Logothetis 2001 simultaneous measurement:
- **LFP (local field potential)** ↔ BOLD r > 0.8
- **MUA (multi-unit activity)** ↔ BOLD r ~ 0.5
- BOLD reflects mainly **input + processing** (synaptic activity), not output spikes

---

## 4. Main Paradigms

### 4.1 Block Design

Task A vs B alternating 30s blocks → compare averages.

### 4.2 Event-related

Single trials → reconstruct hemodynamic response.

### 4.3 Resting-state

At rest → measure networks (DMN, salience, etc.).

### 4.4 Task-based

Specific cognitive tasks (Stroop, n-back, etc.).

### 4.5 MVPA (Multi-Voxel Pattern Analysis)

Not just averaged activation, decoding voxel patterns.

---

## 5. Analysis Pipeline

```
Raw scans
   ↓
Motion correction
   ↓
Spatial normalization (MNI / Talairach)
   ↓
Spatial smoothing (8 mm FWHM Gaussian)
   ↓
GLM (General Linear Model) fitting
   ↓
Statistical maps (t-test / F-test per voxel)
   ↓
Multiple comparison correction (FWE / FDR)
   ↓
Visualization
```

---

## 6. Tools

- **SPM** (London)
- **FSL** (Oxford)
- **AFNI** (NIMH)
- **fMRIPrep** (preprocessing pipeline)
- **Nilearn** (Python)

---

## 7. PyTorch / Python — Simplified GLM

```python
import numpy as np

def hrf(t, peak=6, undershoot=16):
    return t**peak * np.exp(-t) - 0.35 * t**undershoot * np.exp(-t) / 50

def fit_glm(voxel_timeseries, task_design):
    t = np.arange(0, 30, 1)
    hrf_canonical = hrf(t)
    predicted = np.convolve(task_design, hrf_canonical, mode='same')
    X = np.column_stack([predicted, np.ones(len(predicted))])
    beta, *_ = np.linalg.lstsq(X, voxel_timeseries, rcond=None)
    return beta
```

---

## 8. Famous Studies

- **Cognitive networks (Raichle 2001)**: Default Mode Network
- **MVPA decoding (Kamitani 2005)**: decode visual content from V1
- **Reading brain (Talairach atlas)**
- **fMRI lie detection** (controversial)
- **Connectome (HCP)**: 1000+ subjects resting-state

---

## 9. Limitations

### 9.1 Poor Temporal Resolution

BOLD slow 5-7 sec; can't resolve < 1s events.

### 9.2 Indirect Measurement

BOLD ≠ neural firing directly.

### 9.3 Multiple Comparison

Per voxel test → 100k+ tests → strict control needed.

### 9.4 Reverse Inference

"X region active = X processes task Y" is wrong inference (causality not possible).

### 9.5 Subject Variability

Individual anatomy differences, need normalization.

---

## 10. Modern Developments

- **7T MRI**: sub-mm resolution
- **Real-time fMRI**: BCI / neurofeedback
- **fMRI + DTI**: structure + function
- **Multi-modal**: fMRI + EEG / MEG
- **AI decoding**: deep learning + fMRI

---

## 11. Common Pitfalls

### 11.1 "Activity" ≠ Causality

fMRI is correlation; needs lesion / TMS for causality.

### 11.2 Subliminal Effects

Weak signals may be random noise.

### 11.3 Salt Fish fMRI (classic joke)

Dead fish + multi-comparison + no correction → fake activation.

### 11.4 ROI Selection

Can't fish for significance.

### 11.5 Publication Bias

Negative results not published; effect sizes exaggerated.

---

## 12. Related Concepts

- **Same section**: [Optogenetics](Optogenetics.en.md), [TMS](TMS.en.md)
- **Anatomy**: [Cortex](../01_Neuroanatomy/Cortex.en.md)

---

## References

1. **Ogawa, S. et al.** "Brain magnetic resonance imaging with contrast dependent on blood oxygenation (BOLD)." *PNAS*, 1990.
2. **Logothetis, N. K.** "What we can do and what we cannot do with fMRI." *Nature*, 2008.
3. **Raichle, M. E. et al.** "A default mode of brain function." *PNAS*, 2001.
4. **Poldrack, R. A.** "Inferring mental states from neuroimaging data." *NeuroImage*, 2011.
