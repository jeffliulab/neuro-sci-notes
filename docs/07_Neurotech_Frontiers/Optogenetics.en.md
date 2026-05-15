# Optogenetics — Light-Controlled Neurons

> *Optogenetics (Deisseroth 2005) lets neurons express **light-sensitive ion channels** (channelrhodopsin / halorhodopsin) → light triggers activation / inhibition of specific neurons. More **precise + causal** than electrical stimulation. Revolutionary, changed systems neuroscience.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Neuron](../02_Cellular_Molecular/Neuron.en.md), [Ion Channels](../02_Cellular_Molecular/Ion_Channels.en.md)

---

## 1. Key Opsins

- **ChR2** (Channelrhodopsin-2): blue light (470nm) → cation channel opens → depolarize → activate
- **NpHR** (Halorhodopsin): yellow → Cl- pump → hyperpolarize → inhibit
- **Arch** (Archaerhodopsin): yellow → proton pump → inhibit
- **C1V1, ChrimsonR**: red light (deeper penetration)

---

## 2. Experimental Pipeline

1. Viral vector (AAV) injection → express ChR2 in specific cell type
2. Implant optical fiber
3. Light delivery → activate / inhibit
4. Observe behavior / record neurons

→ Cell type specificity via promoter (e.g. CaMKII for excitatory).

---

## 3. vs Traditional

| Method | Spatiotemporal precision | Cell-type specific | Causal |
|---|---|---|---|
| Electrical stimulation | High | ❌ (activates nearby everything) | ✓ |
| Lesion | Medium | ❌ | ✓ |
| fMRI | Low (space mm, time sec) | ❌ | ❌ |
| Drug | Slow | Partial | ✓ |
| **Optogenetics** | **High + high** | **✓** | **✓** |

---

## 4. Landmark Experiments

### 4.1 Memory Engram (Tonegawa 2012-2015)

Activate fear memory in DG → mouse repeats freeze response.
Proves memory has cellular substrate.

### 4.2 DA Reward (Tsai 2009)

Light-activate VTA DA → mouse spontaneously seeks activation area (place preference).
Proves DA = reward.

### 4.3 Parkinson (Kravitz 2010)

Activate direct vs indirect pathway → movement vs suppression.
Proves BG model causally.

### 4.4 Sleep (Adamantidis 2007)

Activate hypocretin → mouse wakes up.

---

## 5. Applications

- **Basic research**: parse specific cell-type functions
- **Clinical exploration** (retinal degeneration treatment trials)
- **AI training data**: precise neural manipulation data

---

## 6. Limitations

### 6.1 Invasive

Requires skull opening + virus injection. Non-human applications heavily restricted.

### 6.2 Long Setup

Mouse ChR2 expression takes 2-4 weeks.

### 6.3 Light Penetration

Blue light penetrates only ~1 mm; red ~3-5 mm. Deep needs fiber implant.

### 6.4 Virus Specificity

AAV imperfect targeting; some spillover.

---

## 7. PyTorch — Optogenetic Experiment Concept

```python
import torch

class Neuron:
    def __init__(self, has_ChR2=False):
        self.V = -70
        self.has_ChR2 = has_ChR2
        self.spike = False
    
    def step(self, I_input, blue_light=False):
        self.V += 0.1 * (I_input - (self.V + 70) * 0.1)
        if self.has_ChR2 and blue_light:
            self.V += 5
        if self.V > -55:
            self.spike = True
            self.V = -75
        else:
            self.spike = False
        return self.spike
```

---

## 8. Modern Evolution

- **Soft optogenetics**: red / NIR + heat
- **2-photon optogenetics**: single-cell precision
- **Genetically encoded sensors** (GCaMP for Ca, iGluSnFR for Glu)
- **All-optical**: simultaneous read + write
- **Sonogenetics**: ultrasound replaces light

---

## 9. Common Pitfalls

### 9.1 ChR2 Behavioral Interference

Strong stimulation may induce non-physiological firing.

### 9.2 Off-target Effect

AAV expresses ChR2 in glia / other cells.

### 9.3 Light Heating

Strong light → heating → affects readout.

### 9.4 Imperfect Specificity

Promoters not 100% specific.

### 9.5 Human Translation

Ethics + virus vectors remain challenges.

---

## 10. Related Concepts

- **Same section**: [fMRI BOLD](fMRI_BOLD.en.md), [TMS](TMS.en.md)
- **Cellular**: [Ion Channels](../02_Cellular_Molecular/Ion_Channels.en.md)

---

## References

1. **Boyden, E. S. et al.** "Millisecond-timescale, genetically targeted optical control of neural activity." *Nat Neurosci*, 2005.
2. **Deisseroth, K.** "Optogenetics." *Nat Methods*, 2011.
3. **Tonegawa, S. et al.** "Memory engram cells." *Cell*, 2015.
4. **Kravitz, A. V. et al.** "Regulation of parkinsonian motor behaviors by optogenetic control of basal ganglia circuitry." *Nature*, 2010.
