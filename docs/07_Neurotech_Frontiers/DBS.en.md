# Deep Brain Stimulation (DBS)

> *DBS implants electrodes deep in brain, continuous high-frequency stimulation improves motor / psychiatric symptoms. Started treating Parkinson in 1980s; expanded to OCD, depression, epilepsy. 100k+ patients worldwide. RNS, focused ultrasound are newer versions.*
>
> **Difficulty**: Intermediate-Advanced
> **Prerequisites**: [Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.en.md), [Brainstem](../01_Neuroanatomy/Brainstem.en.md)

---

## 1. DBS System

3 components:
- **Leads**: 4 contact electrodes in target nucleus
- **IPG (Implantable Pulse Generator)**: chest (like cardiac pacemaker)
- **Extension wires**: subcutaneous connection

Stimulation: high frequency (130-185 Hz), pulse width 60-120 μs, voltage 1-5 V.

---

## 2. Main Targets

### 2.1 Parkinson

- **STN (Subthalamic Nucleus)**: most common target
- **GPi**: alternative to STN, good for dyskinesia
- Significant motor improvement (75%+ tremor reduction / 50%+ bradykinesia)
- Doesn't affect disease progression

### 2.2 Essential Tremor

- **VIM thalamus**: highly effective for hand tremor

### 2.3 Dystonia

- **GPi**

### 2.4 OCD

- **ALIC, NAcc**: refractory OCD, FDA HDE approved

### 2.5 Depression (Experimental)

- **SCC25 (subcallosal cingulate)**: pioneered by Mayberg 2005
- Some refractory depression patients improve significantly
- Trial results inconsistent (BROADEN trial 2017 failed primary endpoint)

### 2.6 Epilepsy

- **Anterior thalamic nucleus (ANT)**: SANTE trial
- FDA approved 2018

---

## 3. Surgery Flow

1. MRI plan target
2. Stereotactic frame (Leksell)
3. Burr hole + electrode insertion
4. **Microelectrode recording** (MER): verify target neurons
5. Test stimulation
6. Permanent electrode lock
7. IPG implant (1 week later)
8. Programming (weeks of fine-tune)

---

## 4. Mechanism (Incompletely Understood)

- **Inhibition of target**: high-freq stim may "disrupt" pathological signal
- **Excitation of axons**: stimulate efferent fibers
- **Network modulation**: affects cortico-BG-thalamic loop
- **Neuromodulator release** changes

---

## 5. Closed-Loop DBS

Next-gen (RNS, adaptive DBS):
- Detects pathological signal (beta band oscillation in PD)
- Stimulates only when needed
- Battery lifespan ×3, fewer side effects
- Medtronic Percept (2020 FDA)

---

## 6. Safety / Side Effects

- Surgery risk: bleeding (~ 1%), infection (~ 3%)
- Stimulation side effects: speech, balance, mood
- Long-term: device failure (lead break, IPG dead)
- Battery 5-15 year replacement

---

## 7. PyTorch — Closed-Loop DBS Algorithm

```python
import torch

class AdaptiveDBSController:
    def __init__(self, beta_threshold=10, stim_freq=130):
        self.beta_threshold = beta_threshold
        self.stim_freq = stim_freq
        self.fft_window = 256
    
    def step(self, lfp_window):
        spec = torch.fft.rfft(lfp_window)
        beta_power = (spec[13:30].abs() ** 2).mean()
        if beta_power > self.beta_threshold:
            return self.stim_freq
        else:
            return 0
```

---

## 8. AI / Data

- ML predict optimal stim parameters
- Closed-loop algorithm optimization
- Long-term LFP database (Medtronic Percept)
- Personalized DBS (custom target / freq)

---

## 9. Economics / Adoption

- DBS system ~ $50k + surgery
- Global 100k+ patients implanted
- China, India adoption accelerating
- New devices: Boston Scientific Vercise, Abbott Infinity

---

## 10. Common Pitfalls

### 10.1 Not All PD Suitable

DBS most effective on **L-DOPA-responsive** motor symptoms; axial symptoms (balance, swallow) poor.

### 10.2 Lead Placement Critical

mm-level deviation → no effect or side effects.

### 10.3 Cognitive Side Effects

Long-term subtle cognitive decline in some patients.

### 10.4 Programming Complex

Best parameters individual; needs experienced neurologist.

### 10.5 Irreversible (But Can Be Off)

Electrode permanent but IPG can be turned off. Side effects reversible by turning off stim.

---

## 11. Related Concepts

- **Same section**: [Optogenetics](Optogenetics.en.md), [TMS](TMS.en.md), [fMRI BOLD](fMRI_BOLD.en.md)
- **Diseases**: [Parkinson](../08_Neuro_Disorders/Parkinson.en.md), [Depression](../08_Neuro_Disorders/Depression.en.md), [Epilepsy](../08_Neuro_Disorders/Epilepsy.en.md)

---

## References

1. **Benabid, A. L. et al.** "Long-term suppression of tremor by chronic stimulation of the ventral intermediate thalamic nucleus." *Lancet*, 1991.
2. **Mayberg, H. S. et al.** "Deep brain stimulation for treatment-resistant depression." *Neuron*, 2005.
3. **Krack, P. et al.** "Five-year follow-up of bilateral stimulation of the subthalamic nucleus in advanced Parkinson's disease." *NEJM*, 2003.
4. **Lozano, A. M. et al.** "Deep brain stimulation: current challenges and future directions." *Nat Rev Neurol*, 2019.
