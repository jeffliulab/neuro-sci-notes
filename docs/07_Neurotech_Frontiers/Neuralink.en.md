# Neuralink — High-Density Invasive BCI

> *Neuralink is the BCI company founded by Elon Musk in 2016. Core tech: N1 implant — 1,024 high-density electrodes + 64 thread bundles + automated surgical robot R1. First human implant in 2024 (Noland Arbaugh). Goals: solve paralysis → vision restoration → long-term brain-AI symbiosis. A density breakthrough but still invasive, needing years of validation.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [BCI History](../06_Brain_Computer_Interface/index.md), [Cortex](../01_Neuroanatomy/Cortex.en.md)

---

## 1. Company Background

- 2016 founded by Musk + co-founders (Max Hodak et al.)
- 2020 demoed pig brain implant (Gertrude)
- 2021 demoed monkey playing Pong by thought (Pager)
- 2023 FDA approved IDE (Investigational Device Exemption)
- 2024-01 first human implant (Noland Arbaugh, spinal injury)
- 2024-08 second patient (Alex)
- 2025-05 public PRIME trial update on first two

---

## 2. N1 Chip Specs

| Parameter | Value |
|---|---|
| Electrode count | 1,024 |
| Threads | 64 |
| Electrodes / thread | 16 |
| Thread diameter | 5 μm (1/10 of hair) |
| Sample rate | 18 kHz / channel |
| Comm | BLE → external |
| Power | < 1 W (wireless charging) |
| Battery | full-day |

---

## 3. R1 Surgical Robot

- Automated thread insertion (with neurosurgeon supervision)
- Microscopy + optical vessel avoidance
- < 1 hour surgery (craniotomy + implant)
- Reduces human error, key to commercial viability

---

## 4. Signal Processing Pipeline

```
1,024 electrodes
    ↓ 18 kHz sample
    ↓ ADC + spike detection (on chip)
    ↓ thresholding + sorting
Compressed spike events
    ↓ BLE
External device (Macbook/iPad)
    ↓ decoder (logistic regression / NN)
Cursor / action
```

- On-chip pre-processing reduces BLE bandwidth
- Spike detection threshold auto-calibrated

---

## 5. PRIME Trial Progress

- **Noland Arbaugh** (Patient 1):
  - 2024-01 implant
  - Within weeks controls screen cursor, plays games (Civilization, Mario Kart)
  - But thread retraction reduced active channels → algorithm compensated
- **Alex** (Patient 2):
  - 2024-08
  - Better than Noland
  - Learned CAD design
- Public demos: cursor speed approaching healthy subjects

---

## 6. PyTorch — Spike Decoder (simplified)

```python
import torch

class SpikeDecoder(torch.nn.Module):
    """Decode 2D cursor from spike rates."""
    def __init__(self, n_channels=1024, hidden=128):
        super().__init__()
        self.encoder = torch.nn.Linear(n_channels, hidden)
        self.lstm = torch.nn.LSTM(hidden, hidden, batch_first=True)
        self.head = torch.nn.Linear(hidden, 2)  # x, y velocity
    
    def forward(self, spike_rates):
        # spike_rates: (B, T, n_channels) — binned rates over time
        h = torch.relu(self.encoder(spike_rates))
        out, _ = self.lstm(h)
        velocity = self.head(out)
        return velocity
```

---

## 7. Competitors

| Company | Approach | Status |
|---|---|---|
| Neuralink | 1024 thread implant | PRIME trial |
| Synchron | Stentrode endovascular | 6-patient trial (BCI from inside blood vessel) |
| Paradromics | Connexus high-density | 2025 trial launch |
| BrainGate | Utah array (96) | Long-term clinical (classic) |
| Blackrock | Utah / Nanogap | Commercially available |
| Precision | LMA microarray | 2025 trial |
| MindMaze, Cogniwave | Non-invasive | Different track |

---

## 8. Long-Term Goals

- **Near**: paralysis cursor / typing (text, brain-to-speech)
- **Mid**: vision restoration (Blindsight project)
- **Long**: cognitive enhancement, brain-AI direct interface (Musk vision)
- **Controversy**: long-term safety, business model, AI integration risks

---

## 9. Engineering Challenges

- **Biocompatibility**: gliosis (scar tissue) around threads
- **Signal degradation**: channel loss over 12 months
- **Thread retraction**: Noland's case ~ 85% channel lost
- **Wireless power**: charging inconvenience, implant battery life
- **Surgery scaling**: R1 still expensive

---

## 10. Common Pitfalls

### 10.1 Not Mind Reading

Decodes motor intent / cursor only, not thoughts.

### 10.2 "Telepathy" Branding Exaggerated

Musk's "telepathy" naming misleads; actual is motor decoding.

### 10.3 Invasive ≠ Best

Endovascular BCI (Synchron) avoids craniotomy; different tradeoff.

### 10.4 Channels Far Insufficient

Human brain ~ 100B neurons; 1024 samples nano area.

### 10.5 Business Model Unclear

Who pays? Insurance? DARPA? Musk private?

---

## 11. Related Concepts

- **Same section**: [DBS](DBS.en.md), [Optogenetics](Optogenetics.en.md), [fMRI BOLD](fMRI_BOLD.en.md)
- **BCI section**: [BCI overview](../06_Brain_Computer_Interface/index.md)
- **Anatomy**: [Cortex](../01_Neuroanatomy/Cortex.en.md)

---

## References

1. **Musk, E. & Neuralink** "An integrated brain-machine interface platform with thousands of channels." *J Med Internet Res*, 2019.
2. **Neuralink** "PRIME Study Progress Update." 2024-05.
3. **Willett, F. R. et al.** "A high-performance speech neuroprosthesis." *Nature*, 2023.
4. **Hochberg, L. R. et al.** "Reach and grasp by people with tetraplegia using a neurally controlled robotic arm." *Nature*, 2012.
