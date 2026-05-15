# EEG — Scalp Electroencephalography

> *Electroencephalography (EEG) is the most common and cheapest brain imaging. Berger first recorded human EEG in 1924. Non-invasive, ms temporal resolution, cm spatial resolution. Clinical use: epilepsy diagnosis, sleep staging, coma assessment. Consumer EEG (Emotiv, Muse, Neurosky) growing. ML (CNN, Transformer) improving decoding. One of mainstream BCI modalities.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Action Potential](../02_Cellular_Molecular/Action_Potential.en.md), [Cortex](../01_Neuroanatomy/Cortex.en.md)

---

## 1. Physics

- Pyramidal neuron synchronous postsynaptic potentials produce dipoles
- Trillions of neurons synchronously → scalp signal
- Main signal is EPSP/IPSP, not spikes
- EEG ~ μV magnitude, needs high-gain amplifier

---

## 2. Frequency Bands (Rhythms)

| Name | Hz | Association |
|---|---|---|
| Delta (δ) | 0.5-4 | Deep sleep, infants |
| Theta (θ) | 4-8 | Light sleep, hippocampus, deep thought |
| Alpha (α) | 8-13 | Relaxed eyes-closed (occipital) |
| Beta (β) | 13-30 | Awake, active thinking |
| Gamma (γ) | 30-100 | Binding, attention, consciousness hypothesis |

---

## 3. 10-20 System

International standard 21-channel placement:
- Letters: F (frontal), C (central), P (parietal), O (occipital), T (temporal)
- Numbers: odd left, even right
- Modern: 64, 128, 256 channels

---

## 4. EEG vs MEG vs fMRI

| Technique | Time | Space | Cost |
|---|---|---|---|
| EEG | ms | cm | $1K-100K |
| MEG | ms | cm (better than EEG) | $1M+ |
| fMRI | sec | mm | $1M-3M |
| fNIRS | sec | cm | $100K |

EEG is cheapest, most widespread.

---

## 5. Clinical Applications

- **Epilepsy diagnosis**: spike-wave discharge
- **Sleep staging**: NREM 1-3 + REM (PSG)
- **Coma / brain death**: flat EEG
- **Anesthesia depth**: BIS monitor
- **ICU monitoring**: continuous EEG (cEEG)

---

## 6. ERP (Event-Related Potential)

- Task-triggered averaged EEG
- Several components:
  - **P300**: ~ 300 ms post-oddball
  - **N400**: ~ 400 ms semantic
  - **N170**: faces / words
  - **MMN** (Mismatch Negativity)
- BCI P300 speller uses this

---

## 7. PyTorch — EEG CNN

```python
import torch
import torch.nn as nn

class EEGNet(nn.Module):
    """EEGNet (Lawhern 2018) — classic EEG CNN."""
    def __init__(self, n_channels=64, n_classes=4, n_samples=128):
        super().__init__()
        self.conv1 = nn.Conv2d(1, 8, (1, 64), padding='same')
        self.bn1 = nn.BatchNorm2d(8)
        self.depthwise = nn.Conv2d(8, 16, (n_channels, 1), groups=8)
        self.bn2 = nn.BatchNorm2d(16)
        self.pool1 = nn.AvgPool2d((1, 4))
        self.sep_conv = nn.Conv2d(16, 16, (1, 16), padding='same', groups=16)
        self.pool2 = nn.AvgPool2d((1, 8))
        self.fc = nn.Linear(16 * (n_samples // 32), n_classes)
    
    def forward(self, x):
        x = self.bn1(self.conv1(x))
        x = self.bn2(self.depthwise(x))
        x = self.pool1(torch.relu(x))
        x = self.sep_conv(x)
        x = self.pool2(torch.relu(x))
        return self.fc(x.flatten(1))
```

---

## 8. Common BCI Paradigms

- **Motor imagery**: imagine moving → control cursor / arm
- **P300 speller**: flashing matrix → P300 → letter select
- **SSVEP**: visual stimulus → V1 same-frequency → option
- **Cognitive workload**: assessing alertness

---

## 9. Pitfall: Artifacts

- **EMG**: muscle (clenching, blinking)
- **EOG**: eye movement
- **Movement**: head motion
- **Power line** (50/60 Hz noise)
- ICA (Independent Component Analysis) removes

---

## 10. Consumer EEG

- **Muse** (Interaxon): 4 channels, meditation
- **Emotiv EPOC / Insight**: 14 / 5 channels
- **Neurosky MindWave**: 1 channel
- **OpenBCI** (open source)
- **Neurable**: AR/VR
- Limited precision, scientific limits

---

## 11. Common Pitfalls

### 11.1 High Spatial Precision

No; scalp signal sums mm² regions, not single neurons.

### 11.2 EEG = Thoughts

Only decodes some motor intent, attention, emotion categories; not "thought reading."

### 11.3 Gamma = Consciousness

Debated; gamma correlates with attention, but NCC still open.

### 11.4 Single Trial Reliable

Single-trial SNR poor; usually trial averaging shows ERPs.

### 11.5 Consumer EEG = Research

Low precision, few channels; serious research uses 64+ clinical-grade.

---

## 12. Related Concepts

- **Same section**: [fMRI BOLD](fMRI_BOLD.en.md), [TMS](TMS.en.md), [Neuralink](Neuralink.en.md)
- **BCI**: [BCI overview](../06_Brain_Computer_Interface/index.md)
- **AI**: CNN / Transformer for EEG

---

## References

1. **Berger, H.** "Über das Elektrenkephalogramm des Menschen." *Arch Psychiatr Nervenkr*, 1929.
2. **Niedermeyer, E. & da Silva, F. L.** *Electroencephalography*. 5th ed., 2005.
3. **Lawhern, V. J. et al.** "EEGNet: A compact convolutional neural network for EEG-based brain-computer interfaces." *J Neural Eng*, 2018.
4. **Cohen, M. X.** *Analyzing Neural Time Series Data*. MIT Press, 2014.
