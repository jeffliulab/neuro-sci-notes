# Depression — Major Depressive Disorder

> *Depression is the most common mental disorder globally (~5% prevalence). Neural basis involves monoamines (5-HT, NE, DA) + HPA axis + hippocampus + DMN across systems. SSRI has been mainstream 60+ years; recently esketamine + psilocybin are revolutionary (fast + single-dose).*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.en.md), [Hippocampus](../01_Neuroanatomy/Hippocampus_Anatomy.en.md)

---

## 1. Clinical

DSM-5 criteria: ≥ 5 of 9 for ≥ 2 weeks:
- Depressed mood
- Anhedonia
- Sleep / appetite changes
- Reduced energy
- Poor concentration
- Worthlessness / guilt
- Suicidal thoughts
- Etc.

---

## 2. Neural Basis Hypotheses

### 2.1 Monoamine Hypothesis (1965)

- Reduced 5-HT / NE / DA
- SSRI / SNRI raises these → antidepressant
- But: delayed onset (2-4 weeks) → incomplete explanation

### 2.2 Neurotrophic Hypothesis

- BDNF (Brain-Derived Neurotrophic Factor) reduced
- Hippocampal neurogenesis reduced
- LTP impaired
- Antidepressants + exercise raise BDNF

### 2.3 Inflammation Hypothesis

- Cytokines (IL-6, TNF-α) elevated
- Microglia activated
- Interacts with monoamines

### 2.4 HPA Axis

- Chronic stress → high cortisol
- Damages hippocampus (reduced volume)
- Forms vicious cycle

### 2.5 Network Dysfunction

- DMN (Default Mode Network) hyperactivity → rumination
- Salience network dysregulation
- PFC ↓ amygdala ↑

---

## 3. Treatment

### 3.1 First-line

- **SSRI** (Prozac, Zoloft, Lexapro): 5-HT reuptake blockers
- **SNRI** (Venlafaxine, Duloxetine): adds NE
- **CBT** (Cognitive Behavioral Therapy)

### 3.2 Second-line

- **Bupropion**: DA + NE
- **Mirtazapine**: 5-HT receptor antagonist (sedating)
- **Tricyclic antidepressants** (TCA): older, more side effects
- **MAOI**: old, dietary interactions

### 3.3 Revolutionary (2019+)

- **Esketamine (Spravato)**: NMDA antagonist, nasal spray, hours-fast, FDA 2019
- **Psilocybin** (experimental): 1-2 doses long-lasting; FDA breakthrough 2018

### 3.4 Physical Treatment

- **ECT (Electroconvulsive Therapy)**: severe refractory cases
- **TMS** (rTMS on dlPFC): FDA 2008
- **DBS** (experimental): SCC25 / vmPFC

---

## 4. Treatment-Resistant Depression (TRD)

- 2+ adequate trials still fail
- ~30% of MDD patients
- Candidates: Esketamine, ECT, psilocybin, DBS

---

## 5. Numbers

- ~380M patients globally (2025)
- Leading WHO global disability cause
- Suicide ~700k/year (50% have depression)
- Male:female 1:2

---

## 6. Genetics

- Heritability ~35-40%
- No single gene; polygenic (1000+ SNPs)
- 5-HTTLPR polymorphism × life stress classic GxE

---

## 7. PyTorch — Depression Risk Model (toy)

```python
import torch
import torch.nn as nn

class DepressionRiskModel(nn.Module):
    def __init__(self):
        super().__init__()
        self.snp_encoder = nn.Linear(1000, 64)
        self.phq_encoder = nn.Linear(9, 16)
        self.wear_encoder = nn.LSTM(5, 32, batch_first=True)
        self.classifier = nn.Linear(64 + 16 + 32, 1)
    
    def forward(self, snp, phq, wear_ts):
        s = self.snp_encoder(snp)
        p = self.phq_encoder(phq)
        _, (h, _) = self.wear_encoder(wear_ts)
        feat = torch.cat([s, p, h.squeeze(0)], dim=-1)
        return torch.sigmoid(self.classifier(feat))
```

---

## 8. Common Pitfalls

### 8.1 "Chemical Imbalance" Oversimplification

Low 5-HT ≠ sole cause; modern view multi-system.

### 8.2 SSRI Not Magic Bullet

~40-60% remission only.

### 8.3 Strong Placebo Response

Antidepressant trials show 30-40% placebo response.

### 8.4 Heterogeneity

Depression is syndrome; many subtypes.

### 8.5 SSRI Long-term Effects

Dependence + difficult withdrawal; tapering important.

---

## 9. AI / Tech Applications

- Mobile apps monitor mood
- NLP identifies depressive language
- DL fMRI classification
- Chatbot therapy (Woebot, Replika etc.)

---

## 10. Related Concepts

- **Same section**: [Alzheimer](Alzheimer.en.md), [Parkinson](Parkinson.en.md)
- **Neurotransmitters**: [5-HT, DA](../02_Cellular_Molecular/Neurotransmitters.en.md)
- **Reward system**: [Reward](../03_Systems_Neuroscience/Reward_System.en.md)

---

## References

1. **Schildkraut, J. J.** "The catecholamine hypothesis of affective disorders." *Am J Psychiatry*, 1965.
2. **Duman, R. S. & Aghajanian, G. K.** "Synaptic dysfunction in depression: potential therapeutic targets." *Science*, 2012.
3. **Daly, E. J. et al.** "Efficacy of Esketamine Nasal Spray Plus Oral Antidepressant Treatment for Relapse Prevention in Patients with TRD." *JAMA Psychiatry*, 2019.
4. **Carhart-Harris, R. L. et al.** "Trial of psilocybin versus escitalopram for depression." *NEJM*, 2021.
