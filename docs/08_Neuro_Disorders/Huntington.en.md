# Huntington's Disease

> *Huntington's Disease (HD) is an autosomal dominant neurodegenerative disease, ~ 1/10,000 prevalence. CAG expansion in HTT gene → mutant huntingtin protein → striatum degeneration (esp. GABA medium spiny neurons) → chorea + cognitive decline + psychiatric symptoms. Described by George Huntington in 1872. No cure; Tetrabenazine controls chorea. Tominersen ASO failed in Phase III; gene therapy is new direction.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.en.md), Parkinson

---

## 1. Clinical

### 1.1 Motor

- **Chorea**: involuntary, fluid, dance-like movements
- Dystonia, bradykinesia (late)
- Saccade abnormality (early marker)
- Falls, swallowing

### 1.2 Cognitive

- Executive deficits (early)
- Working memory decline
- Procedural learning impaired
- Eventually dementia

### 1.3 Psychiatric

- Depression, anxiety
- Apathy
- Psychosis, behavioral disturbances
- High suicide risk

---

## 2. Genetics

- **HTT gene** chromosome 4, exon 1 CAG repeat
- Normal: 9-35 repeats
- Pre-mutation: 27-35
- **HD: ≥ 40 repeats**
- 36-39: incomplete penetrance
- More repeats → earlier onset (anticipation mainly paternal)

---

## 3. Pathophysiology

- **Mutant huntingtin (mHTT)**: polyglutamine tract → misfolding + aggregation
- Main targets:
  - Striatum GABA medium spiny neurons (MSN)
  - Cortex (later)
- Direct vs indirect pathways: indirect path damaged first → chorea
- Later dopamine system decline → parkinsonism

---

## 4. Contrast with Parkinson's

| | Parkinson | Huntington |
|---|---|---|
| Synapse loss | SNc DA | Striatal GABA |
| Pathway affected | Direct ↓ | Indirect ↓ (early) |
| Motor | Hypokinetic (rigid) | Hyperkinetic (chorea) |
| Tx | L-DOPA (+) | Tetrabenazine (DA depleter, -) |

---

## 5. Imaging

- **MRI**: caudate atrophy (early marker, before putamen)
- **PET**: glucose metabolism ↓ in striatum
- **MRS**: glutamate / NAA changes

---

## 6. Onset Timing

- Typical adult onset 30-50 years
- Juvenile HD (< 20 years): 60+ repeats, often paternal inheritance
- Late-onset (> 60 years): 40-50 repeats, milder

---

## 7. Treatment

### 7.1 Symptomatic

- **Tetrabenazine / deutetrabenazine** (Austedo): VMAT2 inhibitor → DA depleter → reduces chorea
- **Antipsychotics**: chorea + psychiatric
- **SSRI**: depression
- **Physical / occupational therapy**

### 7.2 Disease-Modifying (experimental)

- **Tominersen (Roche)**: ASO reducing mHTT — 2021 Phase III failed
- **Branaplam (Novartis)**: similar failure
- **Gene therapy (AAV)**: uniQure trial ongoing
- **Stem cell**: early
- **CRISPR**: pre-clinical

---

## 8. Prediction + Genetic Counseling

- Genetic test predictive (with family + HTT CAG measurement)
- But ethically loaded: no cure → what good is knowing?
- Most at-risk decline testing
- Prenatal / IVF select unaffected embryos (PGD)

---

## 9. PyTorch — Simplified BG Hyperkinetic Sim

```python
import torch

def hd_basal_ganglia_sim(direct_strength=1.0, indirect_strength=0.3, T=200):
    """HD: indirect pathway weakened → hyperkinetic."""
    activity = []
    for t in range(T):
        direct = direct_strength * torch.randn(1)
        indirect = indirect_strength * torch.randn(1)  # weak
        movement = direct - indirect  # excess
        activity.append(movement.item())
    return activity
```

---

## 10. Common Pitfalls

### 10.1 Chorea = HD Only

No; Sydenham chorea (post-strep), Wilson's, tardive dyskinesia also produce chorea.

### 10.2 Familial Must Manifest

> 99% with CAG ≥ 40, but timing varies.

### 10.3 Tominersen Must Work

Not proven; ASO complex challenges.

### 10.4 HD Is Only Motor

Cognitive + psychiatric equally disabling, often missed.

### 10.5 Caudate Only Affected

Late: cortex, cerebellum, other BG affected too.

---

## 11. Related Concepts

- **Same section**: [Parkinson](Parkinson.en.md), [Alzheimer](Alzheimer.en.md), [ALS](ALS.en.md)
- **Anatomy**: [Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.en.md)
- **Reward**: [Reward System](../03_Systems_Neuroscience/Reward_System.en.md)

---

## References

1. **Huntington, G.** "On chorea." *Med Surg Reporter*, 1872.
2. **HD Collaborative Research Group** "A novel gene containing a trinucleotide repeat that is expanded and unstable on Huntington's disease chromosomes." *Cell*, 1993.
3. **Bates, G. P. et al.** "Huntington disease." *Nat Rev Dis Primers*, 2015.
4. **Tabrizi, S. J. et al.** "Targeting Huntingtin Expression in Patients with Huntington's Disease." *NEJM*, 2019.
