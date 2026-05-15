# Migraine

> *Migraine is a common neurovascular disorder (~ 15% of population), not just "a headache." Mechanism: cortical spreading depression (CSD) + trigeminovascular system activation + CGRP. Four phases: prodrome → aura → headache → postdrome. CGRP monoclonal antibodies (2018+) are the first mechanistic preventive breakthrough in decades.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Brainstem](../01_Neuroanatomy/Brainstem.en.md), [Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.en.md)

---

## 1. Four Clinical Phases

```
Prodrome (hours-days before): yawning, appetite change, mood
   ↓
Aura (~ 20-60 min, ~ 1/3 of patients): visual zigzag/scotoma, paresthesia
   ↓
Headache (4-72 h): unilateral throbbing, photophobia/phonophobia, nausea
   ↓
Postdrome ("migraine hangover"): fatigue, cognitive sluggishness
```

---

## 2. Epidemiology

- ~ 15% globally (female ~ 3× male, hormone-related)
- Disability: a leading global disability cause (young/middle-aged)
- Subtypes: with aura / without aura / chronic migraine (≥ 15 days/month)
- Strong genetic component (familial hemiplegic — CACNA1A and other ion channel genes)

---

## 3. Cortical Spreading Depression (CSD)

- Discovered by Leão 1944
- A wave of depolarization spreading at ~ 3 mm/min across cortex, followed by prolonged suppression
- Corresponds to **aura** (visual zigzag movement speed matches!)
- Triggers trigeminovascular activation → pain

---

## 4. Trigeminovascular System

- Trigeminal nerve innervates meningeal vessels
- Activation → release of **CGRP**, substance P → neurogenic inflammation + vasodilation
- Signal → trigeminal nucleus → thalamus → cortex
- Central sensitization → allodynia (scalp tenderness)

---

## 5. CGRP — Key Molecule

- Calcitonin gene-related peptide: strong vasodilator + pain signal
- CGRP ↑ during migraine attack
- CGRP injection can trigger migraine (in patients)
- → Targeting CGRP = modern treatment revolution

---

## 6. PyTorch — CSD Spreading Wave

```python
import numpy as np

def cortical_spreading_depression(N=100, T=200, D=0.1, speed=3.0):
    """1D reaction-diffusion wave (~3 mm/min) like aura."""
    u = np.zeros(N); u[0] = 1.0       # depolarization start
    history = []
    for t in range(T):
        lap = np.gradient(np.gradient(u))
        react = u * (1 - u) * (u - 0.2)   # bistable excitable
        u = np.clip(u + D * lap + 0.05 * react, 0, 1)
        history.append(u.copy())
    return np.array(history)   # traveling wave = aura percept
```

---

## 7. Treatment

### 7.1 Acute

- **Triptans** (5-HT1B/1D agonist): sumatriptan etc. — vasoconstriction + inhibit CGRP release
- **Gepants** (CGRP receptor antagonist, oral): ubrogepant, rimegepant — no vasoconstriction (cardiovascular safe)
- **Ditans** (5-HT1F): lasmiditan
- NSAID, antiemetic adjuncts

### 7.2 Preventive

- **CGRP mAb** (2018+): erenumab, fremanezumab, galcanezumab, eptinezumab — breakthrough
- Traditional: β-blocker, topiramate, amitriptyline, valproate
- **OnabotulinumtoxinA**: chronic migraine
- **Neuromodulation**: peripheral / transcranial stimulation devices

---

## 8. Triggers

- Hormones (menstrual), sleep disruption, stress (and post-stress "let-down")
- Specific foods/alcohol, dehydration, bright light/strong smell
- Weather changes
- But "triggers" highly individual; some actually prodrome misattribution

---

## 9. Distinction from Other Headaches

| | Migraine | Tension | Cluster |
|---|---|---|---|
| Location | Unilateral throbbing | Bilateral band | Unilateral periorbital severe |
| Duration | 4-72 h | 30 min-7 d | 15-180 min (in clusters) |
| Associated | Photophobia, nausea | Few | Tearing, nasal congestion (autonomic) |
| Sex | Female more | — | Male more |

---

## 10. Common Pitfalls

### 10.1 Just an Ordinary Headache

A neurovascular brain disease, can be severely disabling + has aura neurological symptoms.

### 10.2 Aura = Vascular Ischemia

Modern: CSD (neuronal wave) primary, not pure vascular theory.

### 10.3 Vasodilation Is Root Cause

Vascular theory outdated; neurogenic + CGRP central.

### 10.4 Triptan Suitable for Everyone

Vasoconstriction → cautious in cardiovascular disease; gepant safer alternative.

### 10.5 More Painkillers Better

Overuse → "medication overuse headache" (MOH) worsens it.

---

## 11. Related Concepts

- **Same section**: [Epilepsy](Epilepsy.en.md) (CSD vs seizure), [Stroke](Stroke.en.md)
- **Anatomy**: [Brainstem](../01_Neuroanatomy/Brainstem.en.md)
- **Cellular**: [Ion Channels](../02_Cellular_Molecular/Ion_Channels.en.md) (CACNA1A), [Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.en.md)

---

## References

1. **Goadsby, P. J. et al.** "Pathophysiology of migraine: a disorder of sensory processing." *Physiol Rev*, 2017.
2. **Leão, A. A. P.** "Spreading depression of activity in the cerebral cortex." *J Neurophysiol*, 1944.
3. **Edvinsson, L. et al.** "CGRP as the target of new migraine therapies." *Nat Rev Neurol*, 2018.
4. **Charles, A.** "The pathophysiology of migraine: implications for clinical management." *Lancet Neurol*, 2018.
