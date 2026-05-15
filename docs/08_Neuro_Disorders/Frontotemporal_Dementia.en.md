# Frontotemporal Dementia (FTD)

> *FTD is the second most common dementia under 65 (after early-onset AD). Selective frontal/temporal degeneration → behavioral variant (bvFTD: dramatic personality/social change) or language variant (PPA). Pathology: tau / TDP-43 / FUS inclusions. Overlaps with ALS spectrum (C9orf72). Easily misdiagnosed as psychiatric illness. No disease-modifying treatment.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Alzheimer](Alzheimer.en.md), [Social_Cognition](../04_Cognitive_Neuroscience/Social_Cognition.en.md)

---

## 1. Clinical Subtypes

| Subtype | Core |
|---|---|
| **bvFTD** (behavioral variant) | Personality change, disinhibition, apathy, lack of empathy, compulsions, eating changes |
| **svPPA** (semantic) | Loss of word/object meaning (temporal pole) |
| **nfvPPA** (non-fluent) | Grammar/speech production impairment (left fronto-insular) |
| **FTD-ALS** | + motor neuron disease |

---

## 2. Differences from AD

| | FTD | Alzheimer |
|---|---|---|
| Onset | Often < 65 years | Often > 65 years |
| First symptom | Behavior/language/personality | Episodic memory |
| Early memory | Relatively preserved | Early impaired |
| Pathology | tau/TDP-43/FUS | amyloid + tau |
| Atrophy | Frontal/temporal | Hippocampus/temporoparietal |

---

## 3. Molecular Pathology

- **FTLD-tau** (~ 45%): Pick body, PSP, CBD spectrum
- **FTLD-TDP-43** (~ 50%): shared with ALS (C9orf72, GRN)
- **FTLD-FUS** (~ 5%)
- Protein misfolding + aggregation + prion-like spread (see [Prion Diseases](Prion_Diseases.en.md))

---

## 4. Genetics

- ~ 30-40% familial (high genetic proportion among dementias)
- **C9orf72** hexanucleotide repeat expansion: most common shared FTD + ALS gene
- **GRN** (progranulin), **MAPT** (tau)
- C9orf72 links FTD-ALS spectrum

---

## 5. Affected Circuits

- **bvFTD**: prefrontal (orbitofrontal, ACC, vmPFC) + anterior insula + anterior temporal
- Affects social cognition, empathy, decision, inhibition (see [Social_Cognition](../04_Cognitive_Neuroscience/Social_Cognition.en.md))
- Von Economo neurons (anterior insula/ACC) selectively vulnerable
- "Social brain" degeneration → dramatic behavior change

---

## 6. PyTorch — Loss of Inhibitory Control Simulation

```python
import torch

def bvftd_disinhibition(impulse, pfc_inhibition, T=50):
    """vmPFC/OFC degeneration -> loss of behavioral inhibition."""
    behavior = []
    for t in range(T):
        # Healthy: PFC suppresses inappropriate impulses
        action = torch.relu(torch.tensor(impulse - pfc_inhibition))
        behavior.append(action.item())
    return behavior

# bvFTD: pfc_inhibition -> 0 -> impulses become unfiltered actions
```

---

## 7. Diagnostic Challenges

- Early often misdiagnosed as **psychosis / depression / bipolar / midlife crisis**
- Dramatic behavior change but memory/cognitive tests may be normal early
- Need imaging (frontotemporal atrophy) + behavioral history + sometimes genetics
- Long average diagnostic delay

---

## 8. Treatment

- **No disease-modifying treatment**
- SSRI: reduce disinhibition / compulsion / eating behavior (symptomatic)
- Antipsychotics: cautious (extrapyramidal side effects prone)
- **Cholinesterase inhibitors ineffective or worsening** (unlike AD — key difference!)
- Caregiver support, behavior management
- GRN / C9orf72 gene therapy in clinical trials

---

## 9. Social / Ethical

- Early onset → major work/family impact
- Early "moral/behavioral" changes easily misunderstood as character issues
- Forensic: responsibility debate for disinhibited behavior
- Extremely heavy caregiving burden

---

## 10. Common Pitfalls

### 10.1 Dementia = Poor Memory

FTD early memory often preserved; first symptom is behavior/language.

### 10.2 = Alzheimer

Pathology, age, first symptom, treatment all differ; cholinesterase inhibitors ineffective/harmful in FTD.

### 10.3 Behavioral Change = Psychosis

bvFTD often misdiagnosed; need neurodegeneration differentiation.

### 10.4 An Elderly Disease

FTD mostly onsets 45-65 years (a leading early-onset dementia cause).

### 10.5 Unrelated to ALS

C9orf72 links FTD-ALS as one spectrum.

---

## 11. Related Concepts

- **Same section**: [Alzheimer](Alzheimer.en.md), [ALS](ALS.en.md), [Parkinson](Parkinson.en.md)
- **Cognition**: [Social_Cognition](../04_Cognitive_Neuroscience/Social_Cognition.en.md), [Decision_Making](../04_Cognitive_Neuroscience/Decision_Making.en.md)
- **Anatomy**: [Cortex](../01_Neuroanatomy/Cortex.en.md) (prefrontal)

---

## References

1. **Bang, J., Spina, S., Miller, B. L.** "Frontotemporal dementia." *Lancet*, 2015.
2. **DeJesus-Hernandez, M. et al.** "Expanded GGGGCC hexanucleotide repeat in C9ORF72 causes FTD and ALS." *Neuron*, 2011.
3. **Seeley, W. W. et al.** "Early frontotemporal dementia targets neurons unique to apes and humans (von Economo)." *Ann Neurol*, 2006.
4. **Rascovsky, K. et al.** "Sensitivity of revised diagnostic criteria for bvFTD." *Brain*, 2011.
