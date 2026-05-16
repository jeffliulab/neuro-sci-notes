# Autonomic Nervous System

> *The ANS regulates viscera + glands + smooth muscle (involuntary). Sympathetic ("fight or flight") vs Parasympathetic ("rest and digest") vs Enteric ("gut brain"). Central: hypothalamus + brainstem + insula. Dual innervation + tonic balance. HRV (heart rate variability) is its readout. Tightly linked to emotion (James-Lange), stress, interoception.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Brainstem](../01_Neuroanatomy/Brainstem.en.md), [Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.en.md)

---

## 1. Three Branches

| | Sympathetic | Parasympathetic | Enteric |
|---|---|---|---|
| Function | Fight/flight (mobilize) | Rest/digest (conserve) | GI semi-autonomous |
| Origin | Thoracolumbar (T1-L2) | Brainstem + sacral | Gut wall (~500M neurons) |
| Postganglionic NT | NE (mostly) | ACh | Multiple |
| Heart rate | ↑ | ↓ | — |

---

## 2. Dual Innervation + Tone

- Most organs receive **dual** sympathetic + parasympathetic, often antagonistic
- Not an "on/off switch" but **tonic balance** (tone) dynamic regulation
- E.g.: resting HR ~ 70 = intrinsic ~100 continuously suppressed by vagus
- HRV (heart rate variability) reflects vagal tone

---

## 3. Neurochemistry

- **Preganglionic**: all ACh (nicotinic)
- **Sympathetic postganglionic**: NE (→ α/β adrenergic receptors); sweat glands exception use ACh
- **Parasympathetic postganglionic**: ACh (muscarinic)
- Adrenal medulla = specialized sympathetic ganglion (releases Epi directly into blood)

---

## 4. Central Control

```
Cortex (insula/mPFC/ACC)
   ↓
Hypothalamus (integration center: temp/osmotic/stress)
   ↓
Brainstem (NTS input ↔ nucleus ambiguus/RVLM output)
   ↓
Spinal intermediolateral column
   ↓
Ganglia → target organs
```

Insula = visceral sensory cortex (see [Interoception](Interoception.en.md)).

---

## 5. PyTorch — Sympatho-Vagal Balance

```python
import torch

def autonomic_balance(stressor, baseline_hr=70.0):
    """Sympatho-vagal balance sets heart rate dynamically."""
    sympathetic = torch.sigmoid(torch.tensor(stressor - 0.5)) * 60   # ↑HR
    parasympathetic = torch.sigmoid(torch.tensor(0.5 - stressor)) * 30  # ↓HR
    hr = baseline_hr + sympathetic - parasympathetic
    hrv = parasympathetic / (sympathetic + 1e-3)   # vagal tone proxy
    return hr, hrv
```

---

## 6. Reflexes

- **Baroreflex**: BP↑ → NTS → vagus↑/sympathetic↓ → BP↓ (negative feedback)
- **Chemoreflex**: O₂↓/CO₂↑ → respiration/circulation
- **GI reflexes**: enteric + vagus
- Reflex arc: receptor → NTS → brainstem → output

---

## 7. Emotion + Stress

- **James-Lange**: emotion = perception of bodily (autonomic) states
- **HPA axis** (stress): hypothalamus CRH → pituitary ACTH → adrenal cortisol (slow); sympatho-adrenal-medullary (fast)
- Chronic stress → autonomic imbalance → cardiovascular disease, immunosuppression
- Polyvagal theory (Porges, controversial)

---

## 8. Pathology

- **Orthostatic hypotension / POTS**: autonomic regulation imbalance
- **Diabetic autonomic neuropathy**: silent MI, gastroparesis
- **Multiple system atrophy (MSA)**: severe autonomic failure
- **Panic attack**: sympathetic surge + interoceptive misinterpretation
- **Vagus nerve stimulation (VNS)**: refractory epilepsy/depression treatment (see [DBS](../07_Neurotech_Frontiers/DBS.en.md))

---

## 9. Relation to AI / Engineering

- HRV biofeedback ↔ closed-loop regulation
- Autonomic signals (GSR/HR) → affective computing
- Homeostatic regulation ↔ cybernetic homeostasis (see [Hypothalamus_Homeostasis](Hypothalamus_Homeostasis.en.md))
- VNS = closed-loop neuromodulation (see [Closed_Loop_Neuromodulation](../07_Neurotech_Frontiers/Closed_Loop_Neuromodulation.en.md))

---

## 10. Common Pitfalls

### 10.1 Sympathetic = Bad, Parasympathetic = Good

They cooperate; both essential, imbalance is pathological.

### 10.2 ANS Completely Uncontrollable

Breathing/biofeedback/meditation partly modulate (via vagus).

### 10.3 Postganglionic All NE/ACh Simple Dichotomy

Sweat glands sympathetic use ACh; enteric multi-transmitter (many exceptions).

### 10.4 Fight-or-Flight = All-or-None

It's continuous tonic regulation, not a switch.

### 10.5 ANS Unrelated to Emotion/Cognition

Insula/mPFC regulate; interoception is emotion's basis (James-Lange).

---

## 11. Related Concepts

- **Same section**: [Interoception](Interoception.en.md), [Hypothalamus_Homeostasis](Hypothalamus_Homeostasis.en.md), [Sleep_Wake](Sleep_Wake.en.md)
- **Anatomy**: [Brainstem](../01_Neuroanatomy/Brainstem.en.md)
- **Cognition**: [Emotion](../04_Cognitive_Neuroscience/Emotion.en.md)
- **Cellular**: [Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.en.md) (ACh/NE)

---

## References

1. **Jänig, W.** *The Integrative Action of the Autonomic Nervous System*. Cambridge, 2006.
2. **Benarroch, E. E.** "The central autonomic network: functional organization." *Mayo Clin Proc*, 1993.
3. **Thayer, J. F. & Lane, R. D.** "A model of neurovisceral integration in emotion regulation." *J Affect Disord*, 2000.
4. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
