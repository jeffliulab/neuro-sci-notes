# Closed-Loop Neuromodulation

> *Traditional stimulation is open-loop (fixed parameters, ignoring brain state). Closed-loop = real-time read neural/behavior → algorithm decision → on-demand stimulation. Advantages: more effective, power-saving, fewer side effects, personalized. Approved: NeuroPace RNS (responsive epilepsy stimulation), adaptive DBS (aDBS, PD beta-triggered). A neuromodulation + BCI fusion frontier.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [DBS](DBS.en.md), [Brain Rhythms](../00_Foundations/Brain_Rhythms.en.md)

---

## 1. Open-Loop vs Closed-Loop

| | Open-loop | Closed-loop |
|---|---|---|
| Parameters | Fixed / manually tuned | Real-time adaptive |
| Trigger | Continuous / scheduled | Biomarker-triggered |
| Battery | Drains | Saves (on-demand) |
| Side effects | More (over-stimulation) | Fewer |
| Personalization | Low | High |

---

## 2. Closed-Loop Circuit

```
Sensing (LFP / ECoG / spike / behavior)
   ↓
Feature extraction (band power, biomarker)
   ↓
Decision algorithm (threshold / ML / control theory)
   ↓
Stimulator (DBS / cortical / light / drug)
   ↓ (alters brain state)
back to sensing
```

---

## 3. Approved Systems

- **NeuroPace RNS** (2013 FDA): cortical electrodes detect seizure-like activity → immediate counter-stimulation aborts seizure (responsive)
- **Adaptive DBS (aDBS)**: Medtronic Percept — detects STN **beta oscillation** (PD motor symptom biomarker) → on-demand amplitude
- **Closed-loop SCS**: spinal cord stimulation adjusts intensity by ECAP feedback

---

## 4. Biomarkers

| Disease | Biomarker |
|---|---|
| PD | STN beta (13-30 Hz) excess |
| Epilepsy | Epileptiform discharge / high-frequency oscillation |
| Essential tremor | Tremor frequency LFP / IMU |
| Depression | Individualized network state (experimental) |
| OCD | Limbic circuit markers (experimental) |

Biomarker quality determines closed-loop success.

---

## 5. PyTorch — Responsive Stimulation Controller

```python
import torch

def closed_loop_controller(lfp_window, beta_threshold=0.6):
    """Detect pathological beta -> trigger stimulation (aDBS-like)."""
    # Band power in beta (simplified via FFT)
    spec = torch.abs(torch.fft.rfft(lfp_window))
    beta_power = spec[13:30].mean()
    total = spec.mean() + 1e-6
    rel_beta = beta_power / total
    stim_amplitude = torch.clamp((rel_beta - beta_threshold) * 5, 0, 3)
    return stim_amplitude   # 0 if below threshold -> battery saved
```

---

## 6. Control Strategy Hierarchy

- **Threshold-triggered** (RNS): stimulate when above threshold (simple, robust)
- **Proportional / PID**: adjust amplitude by biomarker deviation
- **Model-based / optimal control**: state-space models
- **RL / adaptive**: learn optimal policy (experimental, strict safety constraints)
- Clinical preference: interpretable + robust > black-box

---

## 7. Advantages

- **Power-saving**: on-demand → battery life↑ (fewer replacement surgeries)
- **Reduced side effects**: avoid over-stimulation (speech / mood)
- **More effective**: precise intervention targeting pathological state
- **Personalized**: adapts to individual biomarker + fluctuation
- **Data**: long-term recording → disease understanding + optimization

---

## 8. Challenges

- **Biomarker**: reliable + real-time + specific (biggest bottleneck, esp. psychiatric)
- **Artifacts**: stimulation contaminates recording (needs artifact rejection)
- **Latency**: sense→decide→stimulate must be fast enough (esp. epilepsy)
- **Closed-loop instability**: feedback oscillation / destabilization risk
- **Algorithm validation + regulation** (approval path for adaptive algorithms)
- Computation / power (in-implant vs external)

---

## 9. Fusion with BCI

- Closed-loop stimulation = "write"; BCI decoding = "read" → bidirectional BCI
- All-optical closed-loop (light read + light write, see [Optogenetics_Advanced](Optogenetics_Advanced.en.md))
- Memory prosthesis (detect → stimulate to enhance encoding, DARPA RAM)
- Mood / psychiatric adaptive (experimental, ethically sensitive)

---

## 10. Common Pitfalls

### 10.1 Closed-Loop Always Better

Depends on biomarker quality; poor biomarker → worse than stable open-loop.

### 10.2 Biomarker Easy to Get

Reliable real-time specific biomarker is the biggest challenge (esp. psychiatric).

### 10.3 More Complex Algorithm Better

Clinical values interpretability + robustness; black-box hard to approve + risky.

### 10.4 No Latency

Sense-decide-stimulate latency critical (epilepsy needs millisecond).

### 10.5 Closed-Loop Inherently Stable

Feedback can cause oscillation / instability; needs control-theoretic analysis.

---

## 11. Related Concepts

- **Same section**: [DBS](DBS.en.md), [Optogenetics_Advanced](Optogenetics_Advanced.en.md), [Neuralink](Neuralink.en.md)
- **Foundation**: [Brain Rhythms](../00_Foundations/Brain_Rhythms.en.md)
- **Computational**: [Reinforcement Learning Brain](../05_Computational_Neuroscience/Reinforcement_Learning_Brain.en.md)
- **Disease**: [Parkinson](../08_Neuro_Disorders/Parkinson.en.md), [Epilepsy](../08_Neuro_Disorders/Epilepsy.en.md)

---

## References

1. **Sun, F. T. & Morrell, M. J.** "The RNS System: responsive cortical stimulation for the treatment of epilepsy." *Expert Rev Med Devices*, 2014.
2. **Little, S. et al.** "Adaptive deep brain stimulation in advanced Parkinson disease." *Ann Neurol*, 2013.
3. **Sani, O. G. et al.** "Mood variations decoded from multi-site intracranial recordings." *Nat Biotechnol*, 2018.
4. **Bouthour, W. et al.** "Biomarkers for closed-loop deep brain stimulation in Parkinson disease and beyond." *Nat Rev Neurol*, 2019.
