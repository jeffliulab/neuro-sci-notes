# Chemogenetics / DREADD

> *DREADD (Designer Receptors Exclusively Activated by Designer Drugs) uses engineered GPCRs (insensitive to endogenous ligands, activated only by inert synthetic drugs CNO/DCZ) for cell-specific, reversible, **implant-free** neural control. Advantages: no fiber invasion, whole-brain coverage; cost: low temporal resolution (minutes-hours). A complementary tool to optogenetics.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Optogenetics](Optogenetics.en.md), GPCR signaling basics

---

## 1. Core Idea

- Engineered muscarinic GPCR: desensitized to acetylcholine, responds only to **inert synthetic ligand**
- "Designer receptor + designer drug" lock-and-key
- AAV + Cre → cell-type specific expression
- Systemic administration (IP/oral) → modulate specific DREADD-expressing neurons

---

## 2. Main DREADDs

| DREADD | G protein | Effect |
|---|---|---|
| **hM3Dq** | Gq | Excite (depolarize / ↑ firing) |
| **hM4Di** | Gi | Inhibit (hyperpolarize / ↓ release) |
| **rM3Ds** | Gs | cAMP↑ |
| **KORD** | Gi (κ-opioid based) | Inhibit (ligand SalB) → dual control with hM3Dq |

---

## 3. Ligand Evolution

- **CNO** (clozapine-N-oxide): classic, but metabolizes back to clozapine in vivo (off-target!)
- **DCZ** (deschloroclozapine): new generation, high affinity + low off-target + low dose
- **Compound 21**, **JHU37160** etc.
- Ligand choice is key to experimental rigor

---

## 4. vs Optogenetics

| | DREADD | Optogenetics |
|---|---|---|
| Trigger | Drug (systemic) | Light (fiber) |
| Implant | None | Fiber (invasive) |
| Temporal resolution | Min-hours | Milliseconds |
| Coverage | All expression area (broad) | Light cone (local) |
| Free behavior | Easy (no tether) | Needs wireless / tether |
| Use | Slow processes, long, wide | Fast, temporally precise |

---

## 5. PyTorch — DREADD Modulation Simulation

```python
import torch

def dreadd_modulation(baseline_firing, dreadd_type, ligand_conc, t):
    """Slow onset (minutes) drug-induced gain change."""
    # Pharmacokinetics: gradual rise then decay over ~hour
    drug_effect = ligand_conc * torch.exp(-((t - 30) ** 2) / 400)
    if dreadd_type == 'hM3Dq':       # excitatory
        return baseline_firing * (1 + 1.5 * drug_effect)
    elif dreadd_type == 'hM4Di':     # inhibitory
        return baseline_firing * torch.exp(-1.5 * drug_effect)
    return baseline_firing
```

---

## 6. Applications

- Slow behavioral processes (feeding, motivation, sleep, social) long-term modulation
- Whole-brain/wide-area cell-type pathway function (no fiber limit)
- Causality of neural circuits in **long-range behavior** (hours-long experiments)
- Clinical translation potential (no implant → easier than optogenetics)

---

## 7. Key Controls

- **Must do**: DREADD(-) + ligand control (exclude ligand itself / metabolite effects)
- After CNO off-target controversy, DCZ + strict controls became standard
- Expression verification (immuno / reporter)

---

## 8. Clinical Direction

- Engineered human GPCR + safe ligand → tunable neural therapy (epilepsy, pain, movement disorders)
- "Chemogenetic DBS" concept (no electrodes)
- Still needs gene delivery + long-term safety (same optogenetics clinical barrier)

---

## 9. Limitations

- Low temporal resolution (can't study millisecond processes)
- Ligand pharmacokinetics → imprecise onset/offset
- CNO off-target historical lesson
- Still needs AAV delivery + expression control
- Effect size / expression-level dependent

---

## 10. Common Pitfalls

### 10.1 CNO Fully Inert

CNO metabolizes back to clozapine → off-target; switch to DCZ + controls.

### 10.2 Temporally Precise

Minutes-hours scale; not milliseconds (use optogenetics for fast processes).

### 10.3 No Controls Needed

Must have DREADD(-) + ligand control (ligand effect).

### 10.4 = Optogenetics Replacement

Complementary: DREADD slow/wide/implant-free, opto fast/precise/local.

### 10.5 Clinically Ready

Needs gene delivery + long-term safety; still research-stage.

---

## 11. Related Concepts

- **Same section**: [Optogenetics](Optogenetics.en.md), [Optogenetics_Advanced](Optogenetics_Advanced.en.md), [Focused Ultrasound](Focused_Ultrasound.en.md)
- **Cellular**: [Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.en.md) (GPCR)
- **Foundation**: [Neural Circuits](../00_Foundations/Neural_Circuits.en.md)

---

## References

1. **Armbruster, B. N. et al.** "Evolving the lock to fit the key to create a family of GPCRs (DREADD)." *PNAS*, 2007.
2. **Roth, B. L.** "DREADDs for neuroscientists." *Neuron*, 2016.
3. **Gomez, J. L. et al.** "Chemogenetics revealed: DREADD occupancy and activation via converted clozapine." *Science*, 2017.
4. **Nagai, Y. et al.** "Deschloroclozapine, a potent and selective chemogenetic actuator." *Nat Neurosci*, 2020.
