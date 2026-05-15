# Multiple Sclerosis

> *MS is an autoimmune demyelinating disease of CNS. Immune cells attack myelin → multiple lesions → slow / fail nerve conduction. Prevalence ~ 1/1000 (higher latitudes more). Female 2-3× more. Relapsing-remitting (RRMS) → secondary progressive (SPMS). Beta-interferon first treatment in 1993. Ocrelizumab, Natalizumab and other mAbs are modern. MRI gadolinium is marker.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.en.md), [Glia](../02_Cellular_Molecular/Glia.en.md)

---

## 1. Pathology

- T cells + B cells + macrophages attack CNS myelin (oligodendrocytes)
- Multiple lesions (plaques) scattered:
  - Periventricular
  - Juxtacortical
  - Infratentorial
  - Spinal cord
- Demyelination → saltatory conduction fails → slow, error
- Late axonal degeneration → permanent disability

---

## 2. Clinical Types

| Type | Description |
|---|---|
| **CIS** (Clinically Isolated Syndrome) | Single episode, possibly progresses to RRMS |
| **RRMS** (Relapsing-Remitting) | Relapse + recovery (85% initially) |
| **SPMS** (Secondary Progressive) | RRMS later slow progressive |
| **PPMS** (Primary Progressive) | Progressive from start (15%) |

---

## 3. Symptoms (Highly Variable)

- **Visual**: optic neuritis (acute monocular blindness, 50% RRMS onset)
- **Motor**: spasticity, weakness
- **Sensory**: numbness, paresthesia, Lhermitte sign
- **Cerebellar**: ataxia, dysmetria
- **Bladder**: urgency, retention
- **Fatigue**: common + severe
- **Cognitive**: speed, working memory

---

## 4. Diagnosis

- **MRI**: T2 / FLAIR hyperintense lesions, Gadolinium-enhancing (active)
- **CSF**: oligoclonal bands (local B-cell antibodies)
- **VEP** (Visual Evoked Potential): optic conduction speed
- **McDonald criteria**: lesion temporal-spatial dissemination

---

## 5. Risk Factors

- **Geography**: higher latitude (Vit D deficiency?)
- **Ethnicity**: Northern European
- **Genetics**: HLA-DRB1*15:01
- **Female**
- **EBV** (Epstein-Barr virus): Bjornevik 2022 big-data strong link
- **Smoking**, **adolescent obesity**

---

## 6. Treatment

### 6.1 Acute Relapse

- High-dose corticosteroids (methylprednisolone 1g × 3-5 days)
- Plasmapheresis (refractory)

### 6.2 Disease-Modifying (DMT)

| Drug | Class | Mechanism |
|---|---|---|
| **Beta-interferon** | Injectable | ↓ T-cell migration |
| **Glatiramer acetate** | Injectable | Myelin mimic peptide |
| **Fingolimod** | Oral | S1P modulator → lymphocytes stuck in lymph |
| **Dimethyl fumarate** | Oral | NRF2 pathway |
| **Teriflunomide** | Oral | Anti-proliferative |
| **Natalizumab** | mAb | α4-integrin blocker (but PML risk) |
| **Ocrelizumab** | mAb | Anti-CD20 (B-cell depletion) |
| **Ofatumumab** | mAb | Anti-CD20 (sub-Q) |
| **Cladribine** | Oral | Lymphocyte depletion |
| **AHSCT** (autologous stem cell) | Aggressive | Reset immune |

### 6.3 PPMS Special

- Ocrelizumab is the only FDA-approved PPMS drug
- Most DMTs ineffective for PPMS

### 6.4 Symptomatic

- Spasticity: baclofen, tizanidine
- Fatigue: amantadine, modafinil
- Bladder: anticholinergics
- Pain: gabapentin
- Depression: SSRI

---

## 7. PyTorch — Demyelination Sim

```python
import torch

def simulate_demyelination(N_segments=50, myelin_loss=0.3, T=100):
    """AP propagation with myelinated vs demyelinated axon."""
    conduction_speed = torch.ones(N_segments)
    demyelinated_mask = torch.rand(N_segments) < myelin_loss
    conduction_speed[demyelinated_mask] *= 0.1
    
    total_time = (1 / conduction_speed).sum().item()
    return total_time
```

---

## 8. EBV Link (2022 Big Discovery)

- Bjornevik et al. *Science* 2022: DoD blood bank 10M soldiers
- Pre-MS EBV exposure → 32× MS risk
- Nearly all MS patients have prior EBV
- → EBV likely necessary (not sufficient) cause
- → Vaccine + antiviral therapy future

---

## 9. AHSCT (Autologous Hematopoietic Stem Cell)

- Chemo ablates immune system → autologous stem cell reset
- Aggressive but one-time
- Multiple trials show more effective than DMT in RRMS
- But mortality risk ~ 1-2%

---

## 10. Common Pitfalls

### 10.1 MS = ALS

Completely different; MS is demyelination autoimmune, ALS is motor neuron degeneration.

### 10.2 Tx Must Stop Progression

DMTs reduce relapse, slow progression, but don't stop.

### 10.3 EBV Determinant

EBV necessary, not sufficient; other factors cofactor.

### 10.4 PPMS = RRMS

Possibly different biology; PPMS treatment hard.

### 10.5 Heat Worsens MS Permanently

Uhthoff phenomenon — heat temporary worsening, but reversible.

---

## 11. Related Concepts

- **Same section**: [Alzheimer](Alzheimer.en.md), [Parkinson](Parkinson.en.md), [ALS](ALS.en.md)
- **Cellular**: [Glia](../02_Cellular_Molecular/Glia.en.md), [Action Potential](../02_Cellular_Molecular/Action_Potential.en.md)

---

## References

1. **Reich, D. S., Lucchinetti, C. F., Calabresi, P. A.** "Multiple sclerosis." *NEJM*, 2018.
2. **Bjornevik, K. et al.** "Longitudinal analysis reveals high prevalence of Epstein-Barr virus associated with multiple sclerosis." *Science*, 2022.
3. **Thompson, A. J. et al.** "Diagnosis of multiple sclerosis: 2017 revisions of the McDonald criteria." *Lancet Neurol*, 2018.
4. **Hauser, S. L. & Cree, B. A. C.** "Treatment of Multiple Sclerosis: A Review." *Am J Med*, 2020.
