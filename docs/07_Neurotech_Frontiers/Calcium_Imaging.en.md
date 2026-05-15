# Calcium Imaging

> *Calcium imaging uses fluorescent indicators (GCaMP family) to read neural activity indirectly via Ca²⁺ changes. Action potential triggers Ca²⁺ influx → GCaMP fluorescence ↑ → microscope records. Tsien 2008 Nobel (GFP). Watch 1000+ neurons simultaneously (vs. electrophysiology's dozens). Mainstream systems neuroscience tool (Allen Brain Observatory).*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Action Potential](../02_Cellular_Molecular/Action_Potential.en.md), [Ion Channels](../02_Cellular_Molecular/Ion_Channels.en.md)

---

## 1. Principle

- AP → voltage-gated Ca²⁺ channels open → Ca²⁺ influx (100 nM → 1 μM, 10× change)
- Genetically encoded Ca²⁺ indicator (GECI):
  - GFP + calmodulin + M13 peptide
  - Ca²⁺ binding → conformation change → fluorescence ↑
- Microscope measures ΔF/F

---

## 2. GCaMP Family

| Version | Year | SNR | Speed |
|---|---|---|---|
| GCaMP1 | 2001 | Weak | Slow |
| GCaMP3 | 2009 | Medium | Medium |
| GCaMP6f/s | 2013 | High | f=fast, s=slow |
| GCaMP7 | 2019 | High | Improved |
| GCaMP8f/m/s | 2023 | Very high | Near single spike |
| jGCaMP8 | 2023 | High | Ca²⁺ peak resolution |

---

## 3. Imaging Techniques

### 3.1 Wide-field

- One CCD captures full FOV
- Fast, cheap
- Poor SNR in vivo

### 3.2 Two-photon (2P)

- IR two-photon excitation → less scattering
- ~ 500 μm depth
- Single-neuron resolution
- Mainstream in vivo

### 3.3 Three-photon (3P)

- Deeper (~ 1 mm)
- But low photon yield

### 3.4 Light-sheet

- Very fast (whole zebrafish brain 1 Hz)

### 3.5 Miniscope (head-mounted)

- 1g small scope
- Free-behaving mouse
- UCLA Miniscope open source

---

## 4. Indicator Types

- **GECI (genetically encoded)**: GCaMP, jRCaMP (red)
- **Synthetic dyes**: Oregon Green BAPTA, Fluo-4
  - Pro: high SNR
  - Con: non-specific labeling, washout

GECI delivered via viral injection + cell-type specific promoter (GCaMP6 Thy1, ChR2 Vgat).

---

## 5. Data + Pipeline

```
Microscope → tif stack (frame × time)
        ↓
Motion correction (sub-pixel)
        ↓
ROI detection (Suite2p, CaImAn)
        ↓
Trace extraction (ΔF/F)
        ↓
Spike inference (MLspike, CASCADE)
        ↓
Population analysis (PCA, dynamics)
```

---

## 6. PyTorch — Simplified Ca Trace

```python
import torch

def simulate_calcium_trace(spike_train, tau_decay=200, dt=33):
    """Spike train → Ca trace via exponential decay (Helmchen model)."""
    F = torch.zeros_like(spike_train, dtype=torch.float)
    F[0] = 0
    for t in range(1, len(spike_train)):
        F[t] = F[t-1] * torch.exp(torch.tensor(-dt / tau_decay))
        if spike_train[t]:
            F[t] += 1.0  # jump per AP
    return F
```

---

## 7. Limitations

- **Temporal resolution**: ~ 50-200 ms (slower than spikes)
- **Spike inference**: hard; GCaMP nonlinear, saturating
- **Indicator buffer**: alters Ca dynamics
- **Bleaching**: long recording hard
- **Z-axis**: single plane, 3D slow
- **Cell-type specificity**: needs transgenic / virus

---

## 8. vs Electrophysiology

| | Electrophysiology | Ca imaging |
|---|---|---|
| Temporal res | μs | 50-200 ms |
| Neurons | 1-100 | 1000+ |
| Cell identity | Hard | Easier (cre-lines) |
| Invasive | Very | Mid (craniotomy) |
| Long-term | days-month | months+ |

→ Ca imaging tradeoff: many but slow.

---

## 9. Large-Scale Experiments

- **Allen Brain Observatory**: tens of thousands neuron Ca recordings (open data)
- **Janelia MesoScope**: multi-area simultaneous
- **OASIS / CaImAn / Suite2p**: open-source pipelines
- **DeepLabCut**: behavior tracking paired with imaging

---

## 10. Robot / AI Applications

- Decode behavior from Ca traces (BCI-like)
- Predict behavior with RNNs
- Mouse neural data → train RNN → transfer to robot

---

## 11. Common Pitfalls

### 11.1 Ca = Spike

No; Ca trace is indirect; GCaMP saturation loses linearity.

### 11.2 ΔF/F Absolute

Baseline F offset → ΔF/F offset.

### 11.3 Motion Correction Optional

Muscle / heartbeat → important artifact.

### 11.4 Auto ROI Accurate

Suite2p auto ROI still 50% noise; manual curation essential.

### 11.5 GCaMP Expression Neutral

Long-term expression affects cell health, Ca dynamics.

---

## 12. Related Concepts

- **Same section**: [Optogenetics](Optogenetics.en.md), [fMRI BOLD](fMRI_BOLD.en.md), [Neuralink](Neuralink.en.md)
- **Basis**: [Action Potential](../02_Cellular_Molecular/Action_Potential.en.md)
- **Methods**: [Research Methods](../00_Foundations/Research_Methods.en.md)

---

## References

1. **Tian, L. et al.** "Imaging neural activity in worms, flies and mice with improved GCaMP calcium indicators." *Nat Methods*, 2009.
2. **Chen, T.-W. et al.** "Ultrasensitive fluorescent proteins for imaging neuronal activity." *Nature*, 2013.
3. **Pachitariu, M. et al.** "Suite2p: beyond 10,000 neurons with standard two-photon microscopy." *bioRxiv*, 2017.
4. **Helmchen, F. & Denk, W.** "Deep tissue two-photon microscopy." *Nat Methods*, 2005.
