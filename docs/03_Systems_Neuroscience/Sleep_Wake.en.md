# Sleep / Wake System

> *Sleep is an active brain process, not passive shutdown. NREM (4 stages) + REM cycle, each with distinct neural basis. Critical functions: memory consolidation, glymphatic clearance, immune regulation. Insomnia / abnormalities linked to many diseases.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Brainstem](../01_Neuroanatomy/Brainstem.en.md), [Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.en.md)

---

## 1. Sleep Stages

```
Wake → N1 → N2 → N3 (deep, slow-wave) → REM → wake or back
        (~ 90 min cycle, 4-6 cycles / night)
```

### 1.1 NREM (Non-REM)

- **N1**: transition, 5%
- **N2**: spindles + K-complex, 45%
- **N3**: slow-wave sleep (SWS), delta wave, 25%
- Body temp drops, HR / BP decrease

### 1.2 REM (Rapid Eye Movement)

- Dreams primarily occur
- Brain activity near wake
- Muscle atonia (paralysis, prevents acting out dreams)
- 25% of total sleep

---

## 2. EEG Features

| Stage | EEG | Freq | Amplitude |
|---|---|---|---|
| Wake | beta, alpha | 8-30 Hz | low |
| N1 | theta | 4-8 Hz | low |
| N2 | spindles + K-complex | 12-14 Hz spindle | medium |
| N3 | delta | 0.5-4 Hz | very high |
| REM | beta + theta | similar to wake | low |

---

## 3. Neural Control

**Wake** (ascending arousal system):
- **ACh** (basal forebrain, brainstem PPT)
- **NE** (locus coeruleus)
- **5-HT** (raphe)
- **DA** (VTA)
- **Histamine** (TMN, hypothalamus)
- **Orexin** (hypocretin, hypothalamus) — flip-flop switch

**Sleep**:
- **VLPO** (ventrolateral preoptic nucleus) → GABA → inhibits arousal nuclei

→ Flip-flop switch model (Saper 2001).

---

## 4. Subjective vs Objective

- **Subjective**: tired feeling
- **Objective**:
  - PSG (polysomnography): EEG + EOG (eye) + EMG (muscle)
  - Actigraphy (wrist activity)
  - Sleep diary

---

## 5. Sleep Functions

### 5.1 Memory Consolidation

- SWS (N3): declarative memory consolidation (hippocampal replay)
- REM: procedural / emotional memory
- Sleep after learning ≠ no sleep (memory tests 50%+ better)

### 5.2 Glymphatic Clearance

- During sleep astrocyte aquaporin 4 → CSF flushes β-amyloid
- Sleep deprivation → amyloid accumulation → Alzheimer risk

### 5.3 Synaptic Homeostasis

- Tononi: wake-time LTP accumulates → sleep LTD balances
- Maintains network efficiency

### 5.4 Other

- Immune regulation
- Metabolic restoration
- Hormones (growth hormone, cortisol)

---

## 6. Sleep Deprivation

- 24 hr no sleep → cognition like BAC 0.10 (drunk driving)
- Continued → microsleeps (involuntary blink-second sleep)
- Long-term → cardiovascular, immune, cognitive decline
- Fatal familial insomnia (genetic, complete insomnia → death)

---

## 7. Circadian Rhythm

- ~24 hr cycle, driven by SCN (suprachiasmatic nucleus)
- Light reset (melatonin from pineal)
- Individual differences: morning lark vs night owl
- Jet lag, shift work disrupt

---

## 8. Sleep Disorders

- **Insomnia**: difficulty falling / staying asleep
- **OSA (Obstructive Sleep Apnea)**: throat collapse, snoring + apnea, cardiovascular risk
- **Narcolepsy**: orexin deficiency → daytime sudden sleep + cataplexy
- **REM Behavior Disorder**: REM atonia fails → acting dreams (kick, run) — early Parkinson signal
- **Restless legs syndrome**

---

## 9. PyTorch — Sleep Stage Classifier (toy)

```python
import torch
import torch.nn as nn

class SleepClassifier(nn.Module):
    def __init__(self, in_features=128, n_stages=5):
        super().__init__()
        self.cnn = nn.Sequential(
            nn.Conv1d(1, 32, 5), nn.ReLU(),
            nn.MaxPool1d(2),
            nn.Conv1d(32, 64, 5), nn.ReLU(),
            nn.AdaptiveAvgPool1d(16),
        )
        self.lstm = nn.LSTM(64, 64, batch_first=True)
        self.fc = nn.Linear(64, n_stages)
    
    def forward(self, eeg_30s):
        feat = self.cnn(eeg_30s)
        seq = feat.transpose(1, 2)
        out, _ = self.lstm(seq)
        return self.fc(out[:, -1])
```

---

## 10. AI Applications

- **Apple Watch / Fitbit sleep tracking**: actigraphy + heart rate
- **AASM scoring auto**: DL classify EEG → AASM stages
- **CPAP**: auto-adjust for OSA treatment
- **Sleep coaching apps**: cognitive behavioral therapy for insomnia (CBT-I)

---

## 11. Common Pitfalls

### 11.1 Sleep ≠ Passive

Active brain process.

### 11.2 8 hr ≠ Universal

Adults need 7-9 hr; teens 8-10; elderly 7-8.

### 11.3 Coffee / Alcohol Affect Sleep Architecture

Alcohol → REM suppression; coffee → SWS suppression (long-acting).

### 11.4 Naps

> 30 min nap → sleep inertia; ideal 10-20 min.

### 11.5 Sleep Tracker Accuracy

Consumer wearables have large errors; PSG is gold standard.

---

## 12. Related Concepts

- **Same section**: [Hippocampus + Memory](Hippocampus_Memory.en.md), [Reward System](Reward_System.en.md)
- **Anatomy**: [Brainstem](../01_Neuroanatomy/Brainstem.en.md)
- **Diseases**: [Alzheimer](../08_Neuro_Disorders/Alzheimer.en.md), [Depression](../08_Neuro_Disorders/Depression.en.md)

---

## References

1. **Saper, C. B. et al.** "The sleep switch: hypothalamic control of sleep and wakefulness." *Trends Neurosci*, 2001.
2. **Walker, M.** *Why We Sleep*. 2017.
3. **Diekelmann, S. & Born, J.** "The memory function of sleep." *Nat Rev Neurosci*, 2010.
4. **Xie, L. et al.** "Sleep drives metabolite clearance from the adult brain (glymphatic)." *Science*, 2013.
