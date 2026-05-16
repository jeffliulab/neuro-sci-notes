# Circadian System

> *Circadian rhythm ≈ 24 h endogenous oscillation, master clock in the hypothalamic suprachiasmatic nucleus (SCN). Molecular mechanism = transcription-translation negative feedback loop (CLOCK/BMAL1 ↔ PER/CRY), 2017 Nobel (Hall/Rosbash/Young). Light (ipRGC → SCN) is the main zeitgeber. Dysregulation → jet lag/shift work disorder/metabolic/mood. Tightly linked to sleep but dissociable.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Sleep_Wake](Sleep_Wake.en.md), [Hypothalamus_Homeostasis](Hypothalamus_Homeostasis.en.md)

---

## 1. Core Concepts

- **Circadian**: endogenous ~24 h rhythm (persists under constant conditions → free-running)
- **Zeitgeber** (time-giver): light (main), feeding, temperature, social
- **Entrainment**: external signal calibrates internal clock to 24 h
- **Phase**: rhythm phase (phase advance/delay)

---

## 2. Molecular Clock (TTFL)

```
CLOCK + BMAL1 (transcription activator)
   ↓ drive transcription
PER + CRY
   ↓ accumulate → dimerize → enter nucleus
inhibit CLOCK/BMAL1 (negative feedback)
   ↓ PER/CRY degrade (~24 h period)
loop restarts
```

→ Transcription-Translation Feedback Loop (2017 Nobel). Nearly every cell has one (peripheral clocks).

---

## 3. SCN Master Clock

- Hypothalamic ~ 20,000 neurons (above optic chiasm)
- Autonomous oscillation (persists ~24 h in vitro) + inter-neuron coupling → robust synchrony
- Output → pineal (melatonin), autonomic, behavior, peripheral clock sync
- SCN lesion → rhythm abolished (random behavior)

---

## 4. Light Entrainment Pathway

- **ipRGC** (intrinsically photosensitive retinal ganglion cells, contain melanopsin) → retinohypothalamic tract → SCN
- Non-image vision (blind people with intact ipRGC still entrain)
- Blue light (~480 nm) strongest → nighttime screens suppress melatonin → phase delay

---

## 5. PyTorch — Oscillation + Entrainment

```python
import numpy as np

def circadian_oscillator(T=120, intrinsic_period=24.5, light_schedule=None):
    """Free-running ~24.5h; light entrains toward 24h."""
    phase = 0.0
    out = []
    for t in range(T):
        omega = 2*np.pi / intrinsic_period
        dphase = omega
        if light_schedule and light_schedule(t):        # light pulse
            # Phase response: advance/delay depending on phase
            dphase += 0.3 * np.sin(phase)               # PRC-like
        phase += dphase
        out.append(np.sin(phase))
    return out   # entrained to 24h with light, drifts without
```

---

## 6. Phase Response Curve (PRC)

- Light's effect on phase **depends on the phase applied**:
  - Early subjective night → phase **delay**
  - Late subjective night/morning → phase **advance**
  - Subjective day → little effect
- Explains jet lag recovery, light therapy timing, melatonin dosing strategy

---

## 7. Relation to Sleep (Two-Process Model)

- **Process C** (circadian): SCN-driven wakefulness propensity rhythm
- **Process S** (homeostatic): sleep pressure rises with wakefulness duration (adenosine)
- Sleep timing = C × S interaction (Borbély, see [Sleep_Wake](Sleep_Wake.en.md))
- Rhythm ≠ sleep (dissociable: forced desynchrony experiments)

---

## 8. Dysregulation + Health

- **Jet lag**: abrupt external phase change, internal clock lags
- **Shift work disorder**: chronic desynchrony → metabolic/cardiovascular/cancer risk↑
- **DSPD/ASPD**: delayed/advanced sleep phase syndromes (often PER gene variants)
- **Seasonal affective disorder (SAD)**: insufficient light → light therapy effective
- Peripheral clock desync (feeding time) → metabolic disease
- "Chronotherapy": rhythm-timed drug/treatment

---

## 9. Relation to AI / Engineering

- Oscillator + entrainment ↔ phase-locked loop (PLL), clock synchronization
- Rhythm modeling ↔ time-series periodic decomposition
- Chronobiology + ML: wearables infer chronotype / health
- System clock sync ↔ distributed clocks (analogy)

---

## 10. Common Pitfalls

### 10.1 Rhythm = Sleep

Dissociable (Process C vs S); SCN lesion abolishes rhythm but sleep persists (fragmented).

### 10.2 Period Exactly 24 h

Endogenous often ~24.2 h (human), calibrated daily by light.

### 10.3 Only SCN Has a Clock

Nearly all cells have peripheral clocks; SCN is the coordinator.

### 10.4 Melatonin = Sleeping Pill

It's a phase signal (timing), not sedative-hypnotic; timing matters more than dose.

### 10.5 Light Entrainment via Rods/Cones

Mainly via ipRGC (melanopsin); blind people may retain entrainment.

---

## 11. Related Concepts

- **Same section**: [Sleep_Wake](Sleep_Wake.en.md), [Hypothalamus_Homeostasis](Hypothalamus_Homeostasis.en.md), [Visual_System](Visual_System.en.md) (ipRGC)
- **Anatomy**: [Brainstem](../01_Neuroanatomy/Brainstem.en.md)
- **Disease**: [Depression](../08_Neuro_Disorders/Depression.en.md) (SAD), [Bipolar_Disorder](../08_Neuro_Disorders/Bipolar_Disorder.en.md) (rhythm triggering)

---

## References

1. **Takahashi, J. S.** "Transcriptional architecture of the mammalian circadian clock." *Nat Rev Genet*, 2017.
2. **Bass, J. & Takahashi, J. S.** "Circadian integration of metabolism and energetics." *Science*, 2010.
3. **Hattar, S. et al.** "Melanopsin-containing retinal ganglion cells." *Science*, 2002.
4. **Borbély, A. A. et al.** "The two-process model of sleep regulation: a reappraisal." *J Sleep Res*, 2016.
