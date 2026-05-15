# Neuroscience Research Methods

> *Neuroscience is multi-level + multi-method. Lesion (Broca) → electrophysiology (intracellular, EEG) → fMRI / optical → optogenetics → connectomics. Each method trades off resolution × invasiveness × cost. Understanding methods is understanding the epistemic basis of scientific claims.*
>
> **Difficulty**: Beginner
> **Prerequisites**: none (Foundations entry)

---

## 1. Methods (Resolution × Invasiveness)

| Method | Spatio-temporal res | Invasive | Human/animal |
|---|---|---|---|
| Lesion | whole-brain | extreme | both |
| Single-unit | μm / ms | high | mainly animal |
| Patch clamp | single-cell / sub-ms | high | animal |
| MEA / Neuropixels | μm-mm / ms | high | animal |
| EEG | cm / ms | none | mainly human |
| ECoG | mm / ms | semi (intracranial) | human (epilepsy) |
| fMRI BOLD | mm / sec | none | mainly human |
| MEG | cm / ms | none | mainly human |
| 2-photon Ca²⁺ | μm / 10s ms | semi (craniotomy) | mainly animal |
| fNIRS | cm / sec | none | human |
| TMS | cm / 10s ms | none | human |
| Optogenetics | single-cell / ms | high (gene + fiber) | mainly animal |
| Connectomics (EM) | nm / static | postmortem | mainly animal |

---

## 2. Lesion Studies

- **Broca (1861)**: patient Tan, lesion → IFG → speech production area
- **Wernicke (1874)**: posterior temporal → comprehension
- **H.M. (Scoville 1957)**: bilateral MTL resection → episodic memory loss
- Modern: stroke / TBI / tumor patient + structural MRI

---

## 3. Electrophysiology

### 3.1 Intracellular

- Patch clamp (Neher & Sakmann, Nobel 1991)
- Single channel currents recordable
- Full action potential waveform

### 3.2 Extracellular

- Single-unit recording (Hubel & Wiesel)
- Tetrode, silicon probe
- **Neuropixels 1.0** (2017): 384 channels
- **Neuropixels 2.0** (2021): 5,120 channels

### 3.3 EEG / MEG

- EEG: surface electrodes, ~ 32-256 channels
- MEG: quantum magnetic field detection
- ms time resolution, poor spatial

---

## 4. fMRI / PET

- fMRI BOLD: blood oxygen → indirect neural activity
- T1/T2 weighted, DTI, resting state
- Logothetis 2001: BOLD ~ LFP > spike
- PET: inject radioisotope, good for receptor binding (DA, 5-HT)

---

## 5. Optical

### 5.1 2-photon imaging

- IR light → less scattering
- Ca²⁺ indicator (GCaMP) for real-time firing
- Single-cell + hundreds simultaneously

### 5.2 Voltage imaging

- Direct membrane voltage (no Ca delay)
- Lower SNR
- Active development

### 5.3 Miniscope

- 1g head-mount mouse microscope
- Free-behaving Ca²⁺ imaging

---

## 6. Optogenetics + Chemogenetics

- **Optogenetics**: gene express ChR2 / NpHR → light control
- **Chemogenetics (DREADD)**: drug control
- Causal inference tool (beyond lesion)

---

## 7. Connectomics

- C. elegans (302 neurons) full connectome (White 1986)
- Drosophila full connectome (FlyEM 2024)
- Mouse cortex MICrONS (2025): 1 mm³ EM
- Human: only partial (Allen Brain Atlas, Human Connectome)

---

## 8. PyTorch — Simple Spike Sorter

```python
import torch

class SimpleSpikeSorter:
    """KMeans on waveform features."""
    def __init__(self, n_clusters=4):
        self.n_clusters = n_clusters
        self.centers = None
    
    def fit(self, waveforms, iter=20):
        """waveforms: (N, T) extracted spikes."""
        idx = torch.randperm(waveforms.shape[0])[:self.n_clusters]
        self.centers = waveforms[idx].clone()
        for _ in range(iter):
            dist = torch.cdist(waveforms, self.centers)
            labels = dist.argmin(dim=1)
            for k in range(self.n_clusters):
                mask = labels == k
                if mask.sum() > 0:
                    self.centers[k] = waveforms[mask].mean(dim=0)
        return labels
```

---

## 9. Behavioral Paradigms

- **Maze**: Morris water maze, radial arm
- **Operant**: Skinner box
- **Working memory**: delayed response, N-back
- **Decision**: 2AFC, Iowa gambling
- **Social**: 3-chamber test
- **Closed-loop**: real-time stim modulating behavior

---

## 10. Common Pitfalls

### 10.1 fMRI Measures Spikes

BOLD is indirect signal, ~ seconds delay.

### 10.2 EEG Localization Precise

EEG spatial resolution is cm-level, can't localize single neuron.

### 10.3 Correlation = Causation

fMRI activation ≠ region necessary; need lesion / optogenetic verification.

### 10.4 Optogenetics Perfect

Optogenetics expression uneven, heating + other confounds.

### 10.5 Connectome = Function

Knowing connections ≠ knowing function (need dynamics + plasticity).

---

## 11. Related Concepts

- **Same section**: [Neuroscience History](Neuroscience_History.en.md)
- **Cellular/molecular**: [Action Potential](../02_Cellular_Molecular/Action_Potential.en.md)
- **Frontiers**: [fMRI BOLD](../07_Neurotech_Frontiers/fMRI_BOLD.en.md), [Optogenetics](../07_Neurotech_Frontiers/Optogenetics.en.md)

---

## References

1. **Carandini, M.** "From circuits to behavior: a bridge too far?" *Nat Neurosci*, 2012.
2. **Sejnowski, T. J. et al.** "Putting big data to good use in neuroscience." *Nat Neurosci*, 2014.
3. **Jun, J. J. et al.** "Fully integrated silicon probes for high-density recording of neural activity." *Nature*, 2017.
4. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
