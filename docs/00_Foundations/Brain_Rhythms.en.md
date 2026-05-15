# Brain Rhythms & Oscillations

> *Neural oscillations are rhythms from mass-synchronous neuron activity (delta to gamma). Buzsáki proposed oscillations are the brain's "syntax." Functions: temporal coordination, binding, memory (theta-gamma coupling), attention (alpha). Cross-frequency coupling, phase coding are frontiers. AI: oscillation ↔ synchronization/gating inspirations.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [EEG](../07_Neurotech_Frontiers/EEG.en.md), [Neural Coding](Neural_Coding.en.md)

---

## 1. Frequency Bands Overview

| Rhythm | Hz | Main state |
|---|---|---|
| Delta | 0.5-4 | Deep sleep |
| Theta | 4-8 | Hippocampus, navigation, memory, REM |
| Alpha | 8-13 | Relaxed, eyes-closed, inhibition |
| Beta | 13-30 | Motor preparation, active |
| Gamma | 30-100+ | Binding, attention, local processing |
| Ripples | 80-200 | Hippocampal SWR (memory replay) |

---

## 2. How Oscillations Arise

- **E-I loop**: excitatory-inhibitory feedback → oscillation (PING / ING models)
- Interneurons (esp. PV+) are rhythm pacemakers
- Thalamocortical loop → spindles, alpha
- Pacemaker neurons (Ih current)

---

## 3. Functional Hypotheses

### 3.1 Temporal Binding

- Gamma synchrony "binds" scattered features into one object (Singer)

### 3.2 Communication Through Coherence (Fries)

- Two regions with aligned gamma phase → effective communication
- Phase mismatch → information gated out

### 3.3 Theta-Gamma Coupling

- ~ 7 gamma cycles nested in one theta cycle → working memory items (Lisman & Idiart)

### 3.4 Phase Coding

- Spike encodes position relative to theta phase (hippocampal phase precession)

---

## 4. Hippocampal SWR (Sharp-Wave Ripple)

- 80-200 Hz brief burst
- During rest / slow-wave sleep
- Compressed replay of experience sequences → memory consolidation
- See [Hippocampus Memory](../03_Systems_Neuroscience/Hippocampus_Memory.en.md)

---

## 5. PyTorch — Theta-Gamma Coupling Generation

```python
import numpy as np

def theta_gamma_signal(T=2.0, fs=1000):
    """Phase-amplitude coupling: gamma amplitude modulated by theta phase."""
    t = np.arange(0, T, 1/fs)
    theta = np.sin(2*np.pi*6*t)              # 6 Hz theta
    gamma_amp = 0.5 * (1 + theta)            # theta modulates gamma amplitude
    gamma = gamma_amp * np.sin(2*np.pi*40*t) # 40 Hz gamma
    return t, theta + gamma
```

→ Modulation Index (Tort 2010) quantifies coupling strength.

---

## 6. Cross-Frequency Coupling

- **Phase-amplitude (PAC)**: slow-wave phase modulates fast-wave amplitude (most common)
- **Phase-phase**
- **Amplitude-amplitude**
- PAC marks multi-scale coordination

---

## 7. Oscillations + Cognition

| Rhythm | Cognitive association |
|---|---|
| Frontal theta | Cognitive control, error monitoring |
| Posterior alpha | Attentional inhibition (ignore irrelevant) |
| Beta | Status quo maintenance (motor / cognitive set) |
| Gamma | Local processing, attention |
| SWR | Memory consolidation + planning |

---

## 8. Pathology

- **Parkinson**: pathological excess beta (STN) → DBS suppresses
- **Epilepsy**: abnormal hypersynchrony
- **Schizophrenia**: gamma + spindle abnormality (PV interneuron)
- **Alzheimer's**: weakened gamma → 40 Hz light/sound stim experiment (GENUS)
- **Depression**: frontal alpha asymmetry

---

## 9. AI Analogy

- **Gating / attention** ↔ communication through coherence
- **Oscillation in RNN**: some RNNs spontaneously oscillate
- **Binding problem**: AI also faces it (slot attention, object-centric)
- But mainstream ANN has no explicit oscillation mechanism

---

## 10. Common Pitfalls

### 10.1 Oscillation = Epiphenomenon

Debated: some think byproduct; but causal experiments (optogenetic) show functionality.

### 10.2 Band Boundaries Absolute

Bands are an artificial division of a continuous spectrum; large individual variation.

### 10.3 Gamma = Consciousness

Linked to attention/binding, but not sufficient; NCC still open.

### 10.4 Higher Frequency = Higher Processing

Different frequencies different functions, no hierarchy.

### 10.5 EEG Rhythm = Single-Neuron Rhythm

EEG is population synchrony; single neurons can be irregular.

---

## 11. Related Concepts

- **Same section**: [Neural Coding](Neural_Coding.en.md), [Levels of Analysis](Levels_of_Analysis.en.md)
- **Frontiers**: [EEG](../07_Neurotech_Frontiers/EEG.en.md), [DBS](../07_Neurotech_Frontiers/DBS.en.md)
- **Systems**: [Hippocampus Memory](../03_Systems_Neuroscience/Hippocampus_Memory.en.md), [Sleep/Wake](../03_Systems_Neuroscience/Sleep_Wake.en.md)

---

## References

1. **Buzsáki, G.** *Rhythms of the Brain*. Oxford, 2006.
2. **Fries, P.** "Rhythms for cognition: communication through coherence." *Neuron*, 2015.
3. **Lisman, J. E. & Jensen, O.** "The theta-gamma neural code." *Neuron*, 2013.
4. **Tort, A. B. L. et al.** "Measuring phase-amplitude coupling between neuronal oscillations." *J Neurophysiol*, 2010.
