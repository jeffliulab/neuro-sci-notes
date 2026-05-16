# Time Perception

> *The brain has no single "clock." Multi-scale timing mechanisms: milliseconds (cerebellum/network states), seconds-minutes (striatal dopamine, SBF model), circadian (SCN), interval timing (scalar timing, Weber's law). Time perception is strongly modulated by attention, emotion, dopamine ("time flies/drags"). Underlies RL, motor, language rhythm.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.en.md), [Cerebellum](../01_Neuroanatomy/Cerebellum.en.md)

---

## 1. Multi-Scale Timing

| Scale | Mechanism | Region |
|---|---|---|
| μs-ms | Auditory interaural time difference | Brainstem |
| Hundreds ms | Network states / cerebellum | Cerebellum, cortex |
| Sec-min (interval) | Dopamine + striatal oscillation | BG, SMA, PFC |
| Circadian | Molecular clock (SCN) | Hypothalamus (see [Sleep_Wake](../03_Systems_Neuroscience/Sleep_Wake.en.md)) |

→ No single central clock (distributed).

---

## 2. Scalar Timing (SET)

- Gibbon's behavioral model: pacemaker → accumulator → comparison
- **Scalar property**: timing error ∝ duration (Weber's law, constant coefficient of variation)
- Classic internal clock metaphor (though mechanism not a literal pacemaker)

---

## 3. Striatal Beat-Frequency (SBF)

- Population of cortical oscillators (different frequencies)
- Striatum detects oscillator phase **coincidence patterns** → encodes duration
- Dopamine modulates pacemaker rate → explains DA drugs altering time sense
- Mainstream interval timing neural model (Matell & Meck)

---

## 4. PyTorch — SBF Coincidence Detection

```python
import torch

def striatal_beat_frequency(t, freqs, target_T):
    """Cortical oscillators; striatum learns coincidence at target_T."""
    osc = torch.stack([torch.cos(2*torch.pi*f*t) for f in freqs])  # (F, T)
    # Striatal MSN: weighted detector tuned to phase pattern at target_T
    pattern_at_T = torch.cos(2*torch.pi*freqs*target_T)
    readout = (osc * pattern_at_T.unsqueeze(1)).sum(0)
    return readout   # peaks when oscillator phases match those at target_T
```

---

## 5. Subjective Time Distortion

- **Attention**: timing ↔ main task compete for resources → distraction → underestimate duration
- **Emotion**: threat/fear → time "slows" (high arousal → clock speeds up)
- **Dopamine**: ↑ DA (stimulants) → overestimate; ↓ (PD) → underestimate
- **Age**: subjective time "speeds up" in adulthood (proportionality / novelty hypothesis)
- Oddball effect: novel stimulus "time dilation"

---

## 6. Rhythm vs Duration

- **Beat-based** (beat, music): BG-dependent
- **Duration-based** (single interval): cerebellum + BG
- Motor timing vs perceptual timing partly dissociable
- Linked to language rhythm, music (see [Auditory_System](../03_Systems_Neuroscience/Auditory_System.en.md))

---

## 7. Pathology

- **Parkinson**: DA↓ → impaired interval timing (underestimate / ↑ variability)
- **Cerebellar damage**: poor ms-level motor timing
- **Schizophrenia**: abnormal time perception (linked to agency deficit)
- **ADHD**: time perception + delay aversion (see [ADHD](../08_Neuro_Disorders/ADHD.en.md))
- **Depression**: subjective time "slows"

---

## 8. Relation to RL / AI

- Temporal discounting depends on time perception
- TD learning needs temporal credit assignment (eligibility trace, see [Reinforcement Learning Brain](../05_Computational_Neuroscience/Reinforcement_Learning_Brain.en.md))
- RNNs spontaneously produce timing (ramping / population dynamics, see [Neural Population Dynamics](../05_Computational_Neuroscience/Neural_Population_Dynamics.en.md))
- Sequence model positional/temporal encoding ↔ neural timing

---

## 9. Measurement Paradigms

- Duration discrimination / bisection
- Duration reproduction
- Beat synchronization tapping
- Verbal estimation
- Different paradigms dissociate mechanisms (motor vs perceptual)

---

## 10. Common Pitfalls

### 10.1 Brain Has a Single Clock

Multi-scale distributed mechanisms; no central clock.

### 10.2 SBF Literal Pacemaker

It's an oscillator coincidence model; "pacemaker" is computational abstraction.

### 10.3 Subjective = Objective Time

Strongly modulated by attention/emotion/DA (systematic distortion).

### 10.4 Rhythm = Duration Same Mechanism

Beat-based vs duration-based partly dissociable.

### 10.5 Time Perception Unrelated to RL

Temporal discounting + credit assignment crucially depend on timing.

---

## 11. Related Concepts

- **Same section**: [Decision_Making](Decision_Making.en.md), [Attention](Attention.en.md), [Working Memory](Working_Memory.en.md)
- **Anatomy**: [Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.en.md), [Cerebellum](../01_Neuroanatomy/Cerebellum.en.md)
- **Computational**: [Reinforcement Learning Brain](../05_Computational_Neuroscience/Reinforcement_Learning_Brain.en.md)
- **Systems**: [Sleep_Wake](../03_Systems_Neuroscience/Sleep_Wake.en.md)

---

## References

1. **Buhusi, C. V. & Meck, W. H.** "What makes us tick? Functional and neural mechanisms of interval timing." *Nat Rev Neurosci*, 2005.
2. **Gibbon, J. et al.** "Toward a neurobiology of temporal cognition." *Curr Opin Neurobiol*, 1997.
3. **Matell, M. S. & Meck, W. H.** "Cortico-striatal circuits and interval timing (SBF)." *Cogn Brain Res*, 2004.
4. **Eagleman, D. M.** "Human time perception and its illusions." *Curr Opin Neurobiol*, 2008.
