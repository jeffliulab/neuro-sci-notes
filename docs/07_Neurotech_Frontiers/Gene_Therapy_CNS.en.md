# CNS Gene Therapy

> *Gene therapy brings curative hope for monogenic neurological diseases. AAV (adeno-associated virus) delivers genes, ASO (antisense oligonucleotide) modulates RNA, CRISPR edits DNA. Approved: Zolgensma (SMA, AAV9, ~ $2M), Spinraza (SMA ASO), Luxturna (genetic blindness), Tofersen (SOD1-ALS). BBB + delivery + cost are core challenges (see [Blood Brain Barrier](../00_Foundations/Blood_Brain_Barrier.en.md)).*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Blood Brain Barrier](../00_Foundations/Blood_Brain_Barrier.en.md), molecular biology basics

---

## 1. Three Modes

| Mode | Mechanism | Example |
|---|---|---|
| **Gene replacement** (AAV) | Deliver functional gene copy | Zolgensma (SMN1) |
| **RNA modulation** (ASO/siRNA) | Antisense oligonucleotide alters splicing/degradation | Spinraza, Tofersen |
| **Gene editing** (CRISPR) | Directly alter DNA | Early clinical (HD, Huntington) |

---

## 2. AAV Vector

- Small, low immunogenicity, non-integrating (mostly episomal), long expression
- Serotype determines tropism: **AAV9**, **AAVrh10** (can cross BBB, esp. neonatal)
- Engineered capsids (PHP.eB etc.) enhance CNS tropism (animals)
- Limits: packaging capacity ~ 4.7 kb (large genes hard), pre-existing antibodies, dose hepatotoxicity

---

## 3. Delivery Routes

| Route | Coverage | Invasiveness |
|---|---|---|
| IV (intravenous) | Wide (needs to cross BBB, AAV9) | Low |
| Intrathecal / ICV | CSF → wide CNS | Medium |
| Intraparenchymal injection | Local precise | High (stereotactic) |
| Intranasal | Bypass BBB (limited) | Low |

---

## 4. Approved Milestones

- **Zolgensma** (2019): AAV9-SMN1, SMA one-time, ~ $2.1M (one of priciest drugs)
- **Spinraza** (2016): ASO, SMN2 splicing correction, repeated intrathecal
- **Luxturna** (2017): AAV-RPE65, hereditary retinal disease (first FDA gene therapy)
- **Tofersen / Qalsody** (2023): SOD1-ALS ASO (reduces SOD1)

---

## 5. PyTorch — ASO Splice Correction (conceptual)

```python
import torch

def aso_splice_correction(pre_mrna_exons, aso_target_exon, efficiency=0.8):
    """ASO blocks a splice silencer -> include skipped exon (e.g., SMN2)."""
    included = pre_mrna_exons.clone().float()
    # Without ASO: target exon skipped (=0). ASO restores inclusion.
    included[aso_target_exon] = efficiency      # fraction corrected
    functional_protein = included.prod()         # all exons needed
    return functional_protein
```

---

## 6. Candidate Diseases

- **Monogenic** best fit: SMA, ALS (SOD1/C9orf72), HD (HTT ASO — Tominersen failed, see [Huntington](../08_Neuro_Disorders/Huntington.en.md)), Rett (MECP2, dose-sensitive hard), Friedreich, AADC deficiency (Upstaza approved)
- **Polygenic/sporadic** (AD, PD, SCZ) much harder (no single target)
- Lysosomal storage diseases (neuronal type)

---

## 7. ASO vs AAV

| | ASO | AAV |
|---|---|---|
| Durability | Needs repeat (intrathecal) | One-time (long expression) |
| Reversible | ✓ (stop drug) | ✗ (hard to withdraw) |
| Capacity | Short sequence | ≤ 4.7 kb |
| Best for | Knockdown / splicing | Replace missing gene |
| Example | Spinraza/Tofersen | Zolgensma/Luxturna |

---

## 8. Challenges

- **BBB**: systemic delivery hard to enter brain (AAV9 partial, neonatal better)
- **Cost**: $1-3M (accessibility + insurance)
- **Immunity**: pre-existing AAV antibodies exclude some patients; capsid immunity
- **Dose toxicity**: high-dose AAV hepatic / DRG toxicity (fatal cases)
- **Irreversible** (AAV) + off-target (CRISPR)
- **Time window**: neurodegeneration often needs early intervention

---

## 9. Frontiers

- Engineered capsids crossing BBB (human validation ongoing)
- + Focused Ultrasound BBB opening for delivery (see [Focused Ultrasound](Focused_Ultrasound.en.md))
- In vivo base/prime editing (more precise, less off-target)
- Regulatable expression (inducible promoters)
- Personalized ASO (Milasen — single-patient custom precedent)

---

## 10. Common Pitfalls

### 10.1 Cures All Neurological Disease

Best for **monogenic**; polygenic/sporadic (AD/PD) far from reach.

### 10.2 AAV One-Shot Risk-Free

Irreversible + immunity + high-dose hepatotoxicity; not worry-free.

### 10.3 IV Injection Enters Brain

BBB strongly blocks; only specific serotypes + neonatal better.

### 10.4 ASO = Gene Therapy (Permanent)

ASO reversible, needs repeat; differs from AAV replacement.

### 10.5 CRISPR Routine Clinical

CNS CRISPR still early; off-target + delivery unsolved.

---

## 11. Related Concepts

- **Same section**: [Focused Ultrasound](Focused_Ultrasound.en.md), [Optogenetics_Advanced](Optogenetics_Advanced.en.md)
- **Foundation**: [Blood Brain Barrier](../00_Foundations/Blood_Brain_Barrier.en.md)
- **Disease**: [ALS](../08_Neuro_Disorders/ALS.en.md), [Huntington](../08_Neuro_Disorders/Huntington.en.md), [Multiple Sclerosis](../08_Neuro_Disorders/Multiple_Sclerosis.en.md)

---

## References

1. **Mendell, J. R. et al.** "Single-dose gene-replacement therapy for spinal muscular atrophy (Zolgensma)." *NEJM*, 2017.
2. **Finkel, R. S. et al.** "Nusinersen versus sham control in infantile-onset SMA (Spinraza)." *NEJM*, 2017.
3. **Miller, T. et al.** "Trial of antisense oligonucleotide tofersen for SOD1 ALS." *NEJM*, 2022.
4. **Deverman, B. E. et al.** "Gene therapy for neurological disorders: progress and prospects." *Nat Rev Drug Discov*, 2018.
