# CSF & Glymphatic System

> *CSF (cerebrospinal fluid) circulates in ventricles + subarachnoid space, providing buoyancy, protection, nutrition, waste clearance. Produced by choroid plexus (~ 500 mL/day). The glymphatic system (Nedergaard 2012): AQP4-mediated CSF-ISF exchange, clearing Aβ/tau during sleep — linked to Alzheimer's. The brain has no traditional lymphatics (until meningeal lymphatics discovered 2015).*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Blood Brain Barrier](Blood_Brain_Barrier.en.md), [Glia](../02_Cellular_Molecular/Glia.en.md)

---

## 1. CSF Basics

| Item | Value |
|---|---|
| Total volume | ~ 150 mL |
| Daily production | ~ 500 mL (turns over ~ 3-4×/day) |
| Production | Choroid plexus (ventricles) |
| Absorption | Arachnoid granulations → venous sinuses |
| Composition | Plasma-like but low protein, low K⁺ |

---

## 2. Circulation Path

```
Choroid plexus (lateral ventricles)
   ↓
Foramen of Monro → 3rd ventricle
   ↓
Cerebral aqueduct → 4th ventricle
   ↓
Foramina (Luschka, Magendie)
   ↓
Subarachnoid space (around brain + spinal cord)
   ↓
Arachnoid granulations → venous sinus
```

---

## 3. CSF Functions

- **Buoyancy**: brain actually weighs ~ 1400 g, ~ 25 g after buoyancy (prevents self-weight compression)
- **Protection**: fluid cushion (impact)
- **Homeostasis**: ion + pH regulation
- **Clearance**: removes metabolic waste
- **Transport**: hormones / signaling molecules

---

## 4. Glymphatic System (Nedergaard 2012)

- "Glial + lymphatic"
- CSF enters along periarterial space
- AQP4 (astrocyte endfeet water channels) mediates CSF ↔ ISF exchange
- Waste (Aβ, tau) drains along perivenous space
- **Active during sleep** (interstitial space expands ~ 60%)

---

## 5. Sleep and Clearance

- Xie 2013: glymphatic flux ↑ during sleep, Aβ clearance ↑
- Explains why sleep deprivation → Aβ accumulation → Alzheimer's risk
- "Sleep is the brain's wash cycle"

---

## 6. Meningeal Lymphatics (2015 Discovery)

- Louveau & Kipnis / Aspelund 2015
- Meninges do have lymphatic vessels (textbooks once said no!)
- Drain CSF + immune cells → deep cervical lymph nodes
- Rewrites CNS "immune privilege" concept

---

## 7. PyTorch — Simplified Glymphatic Clearance

```python
import numpy as np

def glymphatic_clearance(abeta0=100, sleep=True, hours=8):
    """Aβ clearance: faster during sleep (Xie 2013)."""
    k = 0.25 if sleep else 0.10   # clearance rate constant /hour
    t = np.arange(hours)
    abeta = abeta0 * np.exp(-k * t)
    return t, abeta   # sleep → much lower residual Aβ
```

---

## 8. Clinical

- **Hydrocephalus**: CSF circulation / absorption impaired → ventricle enlargement → shunt
- **Normal pressure hydrocephalus (NPH)**: gait + cognition + incontinence triad
- **Idiopathic intracranial hypertension**: high CSF pressure
- **CSF leak**: positional headache
- **Lumbar puncture**: CSF sampling (meningitis, MS oligoclonal bands)
- **Alzheimer's**: glymphatic function decline

---

## 9. CSF Biomarkers

- **Aβ42, tau, p-tau**: Alzheimer diagnosis
- **Oligoclonal bands**: MS
- **WBC / protein / glucose**: meningitis
- **14-3-3**: Creutzfeldt-Jakob

---

## 10. Common Pitfalls

### 10.1 Brain Has No Lymphatics

2015 overturned — meningeal lymphatics + glymphatic exist.

### 10.2 Glymphatic Settled

Mechanism + AQP4 role still debated (some replication failures).

### 10.3 CSF Only Cushions

Also clearance + homeostasis + transport.

### 10.4 Sleep Clearance = Only Function

Sleep is multifunctional; clearance is one.

### 10.5 Shunt Permanent Fix

Shunts often clog / infect / over-drain, need revision.

---

## 11. Related Concepts

- **Same section**: [Blood Brain Barrier](Blood_Brain_Barrier.en.md), [Brain Energy Metabolism](Brain_Energy_Metabolism.en.md)
- **Cellular**: [Glia](../02_Cellular_Molecular/Glia.en.md) (astrocyte AQP4)
- **Systems**: [Sleep/Wake](../03_Systems_Neuroscience/Sleep_Wake.en.md)
- **Disease**: [Alzheimer](../08_Neuro_Disorders/Alzheimer.en.md)

---

## References

1. **Iliff, J. J. et al.** "A paravascular pathway facilitates CSF flow and clearance of interstitial solutes including amyloid β." *Sci Transl Med*, 2012.
2. **Xie, L. et al.** "Sleep drives metabolite clearance from the adult brain." *Science*, 2013.
3. **Louveau, A. et al.** "Structural and functional features of central nervous system lymphatic vessels." *Nature*, 2015.
4. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
