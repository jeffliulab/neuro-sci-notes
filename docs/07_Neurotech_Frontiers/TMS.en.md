# TMS (Transcranial Magnetic Stimulation)

> *TMS (Barker 1985) uses fast-changing magnetic fields to induce electrical current in the brain, **non-invasively** activating / inhibiting cortex regions. Important clinical + research tool today: depression treatment, causal brain mapping, cognitive enhancement.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Cortex](../01_Neuroanatomy/Cortex.en.md), electromagnetism basics

---

## 1. Principle

Faraday electromagnetic induction: changing magnetic field → induces electric field.

```
Capacitor discharge → coil → strong short magnetic pulse (~1 T, 100 μs)
                    ↓
              Induces E-field in brain tissue
                    ↓
              Neurons depolarize → spike
```

→ No skull / scalp incision needed.

---

## 2. Types

- **Single-pulse TMS**: single pulse, causal probing
- **Paired-pulse TMS**: tests cortical inhibition
- **Repetitive TMS (rTMS)**: pulse sequences
  - **High-frequency (5-20 Hz)**: excites cortex
  - **Low-frequency (1 Hz)**: inhibits cortex
  - **Theta-burst (TBS)**: short 50Hz bursts at 5Hz → strong + brief

---

## 3. Coil Shapes

- **Circular coil**: shallow + diffuse
- **Figure-8 coil**: focal + precise (~1 cm² target)
- **H-coil**: deeper (5 cm vs 2 cm)

---

## 4. Clinical Applications

### 4.1 Depression (FDA-approved 2008)

- High-frequency rTMS on left dlPFC
- 5 days/week × 4-6 weeks
- 30-40% response rate (vs drug + therapy resistance)

### 4.2 OCD (FDA 2018)

- Deep TMS on mPFC/ACC

### 4.3 Migraine (FDA 2014)

- Single-pulse TMS at-home device

### 4.4 Stroke Rehabilitation

- Stimulate injured side → enhance motor recovery

### 4.5 Experimental

- Schizophrenia, PTSD, addiction (research stage)

---

## 5. Research (Causal Mapping)

TMS is a **causal** tool, not correlation:

```
TMS on Broca → briefly disrupts speech → causal
fMRI Broca active in speech → only correlation
```

- TMS on V1 → blindness for ms
- TMS on M1 hand area → involuntary finger twitch (MEP)
- TMS on dlPFC → working memory disruption

---

## 6. Safety

- Main risk: seizure (rare, < 1/1000 in healthy)
- Exclusions: epilepsy history, metal implants, pregnancy
- Standard protocols are safe

---

## 7. vs tDCS / DBS

| Aspect | TMS | tDCS | DBS |
|---|---|---|---|
| Invasive | No | No | Surgery needed |
| Precision | High (cm) | Low | Very high (mm) |
| Duration | Minutes | Short | Continuous |
| Intensity | High | Very low | Strong |
| Clinical | Depression+ | Experimental | Parkinson, OCD |

---

## 8. Mechanism — Incomplete

- Direct spike (in M1) → MEP
- LTP-like changes (rTMS cumulative effects)
- Network modulation
- Neurochemistry (DA, GABA)

---

## 9. PyTorch / Python — TMS Pulse Simulation

```python
import numpy as np
import matplotlib.pyplot as plt

def tms_pulse(t, peak_T=1.0, decay_us=100):
    t_us = t * 1e6
    return peak_T * np.sin(2 * np.pi * t / (decay_us * 1e-6)) * np.exp(-t_us / decay_us)

t = np.linspace(-50e-6, 200e-6, 1000)
B = tms_pulse(t)
plt.plot(t * 1e6, B)
plt.xlabel('Time (μs)')
plt.ylabel('Magnetic Field (T)')
```

---

## 10. Common Pitfalls

### 10.1 Target Uncertainty

Coil localization ± 5 mm — can't guarantee hitting specific cortical region.

### 10.2 Coil Heating

Long protocols → coil overheating.

### 10.3 Subject Variability

Need individualized intensity (MEP threshold).

### 10.4 Acoustic Effect

TMS click → 50 dB auditory stimulation, confound must be controlled.

### 10.5 Placebo

Sham TMS has similar effects — double-blind essential.

---

## 11. Related Concepts

- **Same section**: [Optogenetics](Optogenetics.en.md), [fMRI BOLD](fMRI_BOLD.en.md)

---

## References

1. **Barker, A. T. et al.** "Non-invasive magnetic stimulation of human motor cortex." *Lancet*, 1985.
2. **Pascual-Leone, A. et al.** "Transcranial magnetic stimulation in cognitive neuroscience." *Annu Rev Neurosci*, 2000.
3. **Rossi, S. et al.** "Safety, ethical considerations, and application guidelines for the use of transcranial magnetic stimulation in clinical practice and research." *Clin Neurophysiol*, 2009.
4. **George, M. S. et al.** "Daily left prefrontal transcranial magnetic stimulation therapy for major depressive disorder." *Arch Gen Psychiatry*, 2010.
