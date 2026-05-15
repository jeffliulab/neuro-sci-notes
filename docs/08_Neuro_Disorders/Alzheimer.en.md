# Alzheimer's Disease (AD)

> *Alzheimer is the most common neurodegenerative disease — 60-70% of dementia cases. Features: amyloid plaques + tau tangles + early hippocampal atrophy + memory decline. After 50+ years of research no cure, but 2023-2024 anti-amyloid drugs (Lecanemab, Donanemab) are the first disease-modifying treatments.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Hippocampus Anatomy](../01_Neuroanatomy/Hippocampus_Anatomy.en.md), [Glia](../02_Cellular_Molecular/Glia.en.md)

---

## 1. Clinical Features

- Progressive episodic memory decline (hippocampus early)
- Late stage: language, motor, personality changes
- 7-10 years from diagnosis to death
- 55M patients globally (2025)

---

## 2. Pathology

### 2.1 Amyloid Plaques

- β-amyloid (Aβ 40, 42) protein deposits
- Source: APP (Amyloid Precursor Protein) cleaved wrong
- Extracellular cortex
- Affects synaptic function + neuronal toxicity

### 2.2 Tau Tangles

- Hyperphosphorylated tau
- Forms Neurofibrillary Tangles (NFT)
- Intracellular neurons
- Disrupts microtubule transport

### 2.3 Neuron Loss

- Hippocampus + entorhinal early
- Late: widespread cortex
- 50%+ loss before clear symptoms

### 2.4 Inflammation

- Microglia + astrocytes chronically activated
- Drives progression

---

## 3. Amyloid Cascade Hypothesis

```
APP miscleaved → Aβ ↑ → plaque → triggers tau abnormality → tangle → synaptic loss → cell death
```

50-year mainstream; but clinical trial failures forced reconsideration.

---

## 4. Tau Hypothesis

Tau as true driver?
- Tau spread more tightly correlates with cognitive decline
- Tau immunotherapy in trials

---

## 5. Genetics

- **APOE4 allele**: high risk (heterozygous 3×, homozygous 12×)
- **APP, PSEN1, PSEN2 mutations**: early-onset (familial) AD
- Most AD is sporadic (acquired)

---

## 6. Diagnosis

- **Clinical**: cognitive tests (MoCA, MMSE)
- **MRI**: hippocampus + cortex atrophy
- **PET amyloid (Pittsburgh compound B)**: visualizes plaques
- **PET tau (e.g. AV-1451)**
- **CSF biomarkers**: Aβ42, tau
- **Plasma biomarkers** (new): p-tau217

---

## 7. Treatment (2025 status)

### 7.1 Symptomatic (no disease-modifying effect)

- **Cholinesterase inhibitors** (Donepezil, Rivastigmine): raise ACh
- **Memantine**: NMDA modulator

### 7.2 Disease-modifying (2023-2024)

- **Lecanemab (Leqembi)**: anti-Aβ antibody, **FDA approved 2023**
- **Donanemab**: similar, **FDA 2024**
- Slows progression ~25% (with brain edema risk)

### 7.3 Experimental

- Tau immunotherapy
- BACE inhibitors (reduce Aβ production)
- Anti-inflammatory
- Lifestyle (exercise, cognitive training)

---

## 8. Risk Factors

- Age (main)
- Genetics (APOE)
- Low education
- Cardiovascular disease
- Hypertension, diabetes
- Lack of exercise / social engagement
- Poor sleep (impaired amyloid clearance)
- Head trauma

---

## 9. AI Connections

- DL diagnosis from MRI (~95% accuracy)
- Drug discovery (AlphaFold + screening)
- Cognitive testing apps
- Speech analysis for early signs

---

## 10. PyTorch — Hypothetical AD Prediction

```python
import torch
import torch.nn as nn

class ADPredictor(nn.Module):
    def __init__(self):
        super().__init__()
        self.conv = nn.Sequential(
            nn.Conv3d(1, 32, 3), nn.ReLU(),
            nn.MaxPool3d(2),
            nn.Conv3d(32, 64, 3), nn.ReLU(),
            nn.AdaptiveAvgPool3d(1),
        )
        self.fc = nn.Linear(64, 2)
    
    def forward(self, mri):
        feat = self.conv(mri).flatten(1)
        return self.fc(feat)
```

---

## 11. Common Pitfalls

### 11.1 Amyloid Hypothesis Incomplete

Many amyloid+ people cognitively normal; tau better predicts decline.

### 11.2 Early Diagnosis Hard

Plaques accumulate 10-20 years before symptoms.

### 11.3 Limited First-line Drugs

Lecanemab only ~25% slowing, with ARIA risk.

### 11.4 Individual Variability

Heterogeneous → many subtypes; single drug can't fit all.

### 11.5 Many Clinical Trial Failures

Multiple anti-Aβ drug failures (verubecestat, solanezumab etc.).

---

## 12. Related Concepts

- **Same section**: [Parkinson](Parkinson.en.md), [Depression](Depression.en.md)
- **Anatomy**: [Hippocampus](../01_Neuroanatomy/Hippocampus_Anatomy.en.md)
- **Cellular**: [Glia](../02_Cellular_Molecular/Glia.en.md)

---

## References

1. **Hardy, J. & Selkoe, D. J.** "The amyloid hypothesis of Alzheimer's disease." *Science*, 2002.
2. **Selkoe, D. J. & Hardy, J.** "The amyloid hypothesis of Alzheimer's disease at 25 years." *EMBO Mol Med*, 2016.
3. **van Dyck, C. H. et al.** "Lecanemab in Early Alzheimer's Disease." *NEJM*, 2023.
4. **Jack, C. R. et al.** "NIA-AA Research Framework: Toward a biological definition of Alzheimer's disease." *Alzheimers Dement*, 2018.
