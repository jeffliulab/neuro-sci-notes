# Transcranial Electrical Stimulation (tDCS / tACS / tRNS)

> *Transcranial electrical stimulation uses weak current (~ 1-2 mA) through the scalp to modulate cortical excitability. tDCS (direct: anode↑, cathode↓ excitability), tACS (alternating: entrains oscillations), tRNS (random noise). Non-invasive, cheap, portable, but small effects + large individual variability + replication controversy. Hot in research + enhancement + rehab.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [TMS](TMS.en.md), [Brain Rhythms](../00_Foundations/Brain_Rhythms.en.md)

---

## 1. Three Modes

| Mode | Waveform | Mechanism |
|---|---|---|
| **tDCS** | Constant DC | Anodal depolarization (↑excitability) / cathodal hyperpolarization (↓) |
| **tACS** | Sinusoidal AC | Entrain endogenous oscillations |
| **tRNS** | Random noise | Stochastic resonance / ↑excitability |

---

## 2. tDCS Mechanism

- Does not directly trigger AP (current too weak)
- **Modulates resting membrane potential** → changes firing **probability** (neuromodulatory)
- Anodal: membrane depolarization → ↑ excitability
- Cathodal: hyperpolarization → ↓ excitability
- Lasting effects: LTP/LTD-like (NMDA-dependent)

---

## 3. tACS — Oscillation Entrainment

- Applied at target frequency → entrains endogenous rhythm
- E.g.: gamma-tACS improves memory (hypothesis)
- Frequency / phase specific
- Directly linked to [Brain Rhythms](../00_Foundations/Brain_Rhythms.en.md)

---

## 4. vs TMS

| | tES | TMS |
|---|---|---|
| Mechanism | Modulation (subthreshold) | Triggers AP (suprathreshold) |
| Device | Portable, cheap | Large, expensive |
| Spatial precision | Poor (current spread) | Better (focused coil) |
| Effect size | Small, variable | Stronger |
| Use | Research / enhancement | Clinical (depression FDA-approved) |

---

## 5. PyTorch — tDCS Firing Probability Bias

```python
import torch

def tdcs_modulation(baseline_input, polarity='anodal', shift=0.15):
    """tDCS shifts membrane potential -> changes spike probability."""
    bias = shift if polarity == 'anodal' else -shift
    # Subthreshold: modulates probability, doesn't force spikes
    p_spike = torch.sigmoid(baseline_input + bias)
    return p_spike   # anodal -> higher firing probability
```

---

## 6. Application Domains

- **Research**: causal probing of cortical function (portable TMS alternative)
- **Depression**: tDCS (DLPFC) some evidence (weaker than TMS/ECT)
- **Stroke rehab**: combined with training
- **Pain**: M1 tDCS
- **Cognitive enhancement**: learning, working memory (highly debated)
- **DIY community**: ethics + safety concerns

---

## 7. Key Parameters

- Current: 1-2 mA (safety limit usually ≤ 4 mA)
- Electrodes: large (~ 25-35 cm²) → poor precision; HD-tDCS uses small electrode arrays for improvement
- Duration: ~ 10-30 min
- Montage (electrode positions) determines current path (modeling: current flow simulation)

---

## 8. Replication + Variability

- Small effect size, large individual variation (skull, anatomy, state-dependent)
- Multiple meta-analyses inconsistent
- "Responder vs non-responder"
- Trend: individualized modeling + closed-loop + stricter design

---

## 9. Safety

- Generally safe (within regulated parameters)
- Skin irritation / burns (electrode contact)
- Transient scalp tingling, phosphenes (tACS retina)
- Long-term / DIY high-current unknown risks
- Contraindications: caution with epilepsy history, intracranial metal

---

## 10. Common Pitfalls

### 10.1 tES Triggers Action Potentials

It's subthreshold modulation, doesn't directly evoke spikes (unlike TMS).

### 10.2 Anodal Must Improve Performance

↑ excitability ≠ behavioral improvement (depends on task + individual).

### 10.3 Effects Strong and Reliable

Small effects + large variability; replication controversy unresolved.

### 10.4 Spatially Precise

Current spreads broadly; not focused (HD-tDCS improvement limited).

### 10.5 DIY Safe

Improper parameters / montage risky; not a consumer toy.

---

## 11. Related Concepts

- **Same section**: [TMS](TMS.en.md), [DBS](DBS.en.md), [EEG](EEG.en.md)
- **Foundation**: [Brain Rhythms](../00_Foundations/Brain_Rhythms.en.md), [Neuroplasticity](../00_Foundations/Neuroplasticity.en.md)
- **Disease**: [Depression](../08_Neuro_Disorders/Depression.en.md)

---

## References

1. **Nitsche, M. A. & Paulus, W.** "Excitability changes induced in the human motor cortex by weak transcranial direct current stimulation." *J Physiol*, 2000.
2. **Herrmann, C. S. et al.** "Transcranial alternating current stimulation: entraining oscillations." *Front Hum Neurosci*, 2013.
3. **Polanía, R., Nitsche, M. A., Ruff, C. C.** "Studying and modifying brain function with non-invasive brain stimulation." *Nat Neurosci*, 2018.
4. **Horvath, J. C. et al.** "Quantitative review of tDCS effects (replication concerns)." *Neuropsychologia*, 2015.
