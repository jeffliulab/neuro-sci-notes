# Prion Diseases

> *Prion disease (CJD, kuru, FFI, GSS, animal BSE/scrapie) is the only infectious neurodegenerative disease transmitted by **protein conformation**. Prusiner's "protein-only" hypothesis (1997 Nobel): misfolded PrPˢᶜ templates normal PrPᶜ to misfold → chain amplification. 100% fatal, no treatment. The paradigm for understanding "prion-like" spread in AD/PD/ALS proteinopathies.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Alzheimer](Alzheimer.en.md), protein folding basics

---

## 1. Protein-Only Hypothesis

- An infectious agent with no nucleic acid (counterintuitive to central dogma)
- **PrPᶜ** (normal, α-helix rich) → **PrPˢᶜ** (pathogenic, β-sheet rich)
- PrPˢᶜ acts as template inducing PrPᶜ conformation change → exponential amplification
- Prusiner proposed 1982, 1997 Nobel (was highly controversial)

---

## 2. Human Prion Diseases

| Disease | Source |
|---|---|
| **sCJD** (sporadic) | Spontaneous misfolding (~ 85%) |
| **vCJD** (variant) | Eating BSE beef |
| **iCJD** (iatrogenic) | Cornea/dura graft, growth hormone, instruments |
| **fCJD/GSS/FFI** (familial) | PRNP gene mutation |
| **Kuru** | Cannibalism (Fore people, now extinct) |

---

## 3. Clinical Features

- Rapidly progressive dementia (months, far faster than AD)
- Myoclonus
- Ataxia, visual disturbance
- **FFI**: progressive insomnia → death (anterior thalamus degeneration — see [Thalamus](../01_Neuroanatomy/Thalamus.en.md))
- Course: months-2 years, 100% fatal

---

## 4. Pathology

- **Spongiform** (sponge-like vacuolation) — hallmark
- PrPˢᶜ deposition, neuron loss, astrogliosis
- No typical inflammation (unlike infection)
- Brain tissue extremely infectious (standard sterilization **ineffective**!)

---

## 5. PyTorch — Prion Autocatalytic Amplification

```python
import numpy as np

def prion_propagation(prp_c=1000, prp_sc=1, rate=0.05, T=200):
    """Autocatalytic templated conversion: PrP^Sc converts PrP^C."""
    c, sc = float(prp_c), float(prp_sc)
    traj = []
    for t in range(T):
        converted = rate * sc * c / (prp_c)   # template-dependent
        c -= converted
        sc += converted
        c += 5                                # synthesis of normal PrP
        traj.append(sc)
    return traj   # exponential then saturating PrP^Sc growth

# Same kinetic motif underlies 'prion-like' Aβ/tau/α-syn spread
```

---

## 6. Diagnosis

- **RT-QuIC** (Real-Time Quaking-Induced Conversion): highly sensitive/specific (detects seeding activity)
- MRI: DWI cortical/basal ganglia hyperintensity (cortical ribboning)
- EEG: periodic sharp waves (sCJD)
- CSF: 14-3-3, tau ↑
- Confirmation: brain biopsy/autopsy (spongiform + PrPˢᶜ)

---

## 7. "Prion-Like" Spread Paradigm

- AD (Aβ, tau), PD (α-synuclein), ALS (TDP-43, SOD1), Huntington (polyQ)
- All show **templated misfolding + cell-to-cell spread** (explains Braak staging)
- But **non-infectious** (no person-to-person) → called "prion-like" not "prion"
- Prion is the mechanistic prototype for these proteinopathies (see [Alzheimer](Alzheimer.en.md), [Frontotemporal_Dementia](Frontotemporal_Dementia.en.md))

---

## 8. Species Barrier + Strains

- Species barrier: PrP sequence differences limit cross-species transmission (BSE→human exception)
- **Prion strains**: same PrP sequence, different conformations → different phenotypes (challenged early "protein-only" skepticism, now supports it)
- Conformation = information carrier (like "protein heredity")

---

## 9. Treatment + Prevention

- **No effective treatment** (100% fatal)
- Trials: doxycycline, quinacrine, PrP ASO (effective in animals, human trials ongoing)
- Prevention: eliminate BSE (feed ban), strict medical instruments (prions resist routine sterilization → 134°C autoclave / NaOH)
- Surveillance: national CJD monitoring networks

---

## 10. Common Pitfalls

### 10.1 Prion Is a Virus

No nucleic acid; it's a misfolded protein (name misleading).

### 10.2 Routine Sterilization Kills It

Resists standard autoclave/chemicals; needs special procedures (extended 134°C / strong base).

### 10.3 AD/PD Are Infectious Prion Diseases

"Prion-like" mechanism, but **not** person-to-person infectious.

### 10.4 All Are Diet-Acquired

Most sCJD is spontaneous; only vCJD/kuru relate to ingestion.

### 10.5 Protein-Only No Longer Debated

Widely accepted, but details like strain phenomena still researched.

---

## 11. Related Concepts

- **Same section**: [Alzheimer](Alzheimer.en.md), [Frontotemporal_Dementia](Frontotemporal_Dementia.en.md), [ALS](ALS.en.md), [Parkinson](Parkinson.en.md)
- **Anatomy**: [Thalamus](../01_Neuroanatomy/Thalamus.en.md) (FFI)
- **Systems**: [Sleep/Wake](../03_Systems_Neuroscience/Sleep_Wake.en.md) (FFI)

---

## References

1. **Prusiner, S. B.** "Prions." *PNAS*, 1998 (Nobel Lecture).
2. **Collinge, J.** "Prion diseases of humans and animals: their causes and molecular basis." *Annu Rev Neurosci*, 2001.
3. **Jucker, M. & Walker, L. C.** "Self-propagation of pathogenic protein aggregates in neurodegenerative diseases." *Nature*, 2013.
4. **Atarashi, R. et al.** "Real-time quaking-induced conversion (RT-QuIC)." *Nat Med*, 2011.
