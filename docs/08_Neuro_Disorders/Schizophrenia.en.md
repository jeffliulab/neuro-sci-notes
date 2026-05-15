# Schizophrenia

> *Schizophrenia is a chronic mental disorder, ~1% prevalence. Features: positive (hallucinations / delusions) + negative (avolition) + cognitive (executive) symptoms. DA hypothesis mainstream but incomplete; modern view as NMDA + DA + Glu multi-system dysregulation. Antipsychotics revolutionary in 1950s but still mainly symptomatic.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.en.md), [Cortex](../01_Neuroanatomy/Cortex.en.md)

---

## 1. Clinical

DSM-5: 2+ symptoms ≥ 1 month:
- **Positive**: hallucinations (auditory common), delusions, disorganized thought
- **Negative**: flat affect, lack of motivation, withdrawal
- **Cognitive**: attention, working memory, executive deficits

Onset: typically young adult (15-25).

---

## 2. Neural Basis Hypotheses

### 2.1 Dopamine Hypothesis (Classic)

- DA excess (mesolimbic) → positive symptoms
- DA deficit (mesocortical) → negative
- Evidence: amphetamine (DA agonist) triggers psychosis
- Antipsychotics = D2 receptor antagonists

### 2.2 NMDA Hypofunction (Modern)

- NMDA deficit → glutamate dysregulation
- Evidence: Ketamine (NMDA antagonist) triggers schizophrenia-like symptoms
- PCP same
- Better explains cognitive + negative symptoms

### 2.3 GABA Interneurons

- PV-positive interneurons malfunction
- Affects cortical sync (gamma oscillations)
- Linked to working memory dysfunction

### 2.4 Neurodevelopmental

- Abnormal adolescent pruning
- Early-life stressor + genetic susceptibility

---

## 3. Genetics

- Heritability ~80%
- Polygenic (thousands of SNPs)
- 22q11.2 deletion: 30× risk
- COMT, DISC1, NRG1 etc. candidate genes

---

## 4. Treatment

### 4.1 First-generation antipsychotics (FGA)

- **Haloperidol, Chlorpromazine**: D2 antagonists
- Strong positive symptom treatment
- Extrapyramidal side effects (parkinsonism, tardive dyskinesia)

### 4.2 Second-generation (atypical, SGA)

- **Risperidone, Olanzapine, Quetiapine, Clozapine**
- Add 5-HT2A antagonism
- Fewer motor side effects
- But metabolic (weight gain, diabetes)

### 4.3 Clozapine

- Only proven for refractory cases
- But agranulocytosis risk (requires blood monitoring)

### 4.4 Experimental

- NMDA modulator (D-serine, GLYX-13)
- Anti-inflammatory
- Psychotherapy + cognitive rehabilitation

---

## 5. Imaging / Biomarkers

- MRI: enlarged lateral ventricles, reduced gray matter
- DTI: white matter abnormalities
- fMRI: DMN + cognitive control network dysregulation
- DA imaging (PET): increased striatum DA release

---

## 6. AI / Mech Interp Connections

- Predictive coding errors → hallucinations
- LLM "hallucination" is interesting parallel
- Karl Friston uses free energy framework

---

## 7. PyTorch — Neural Net Simulating Prediction Error

```python
import torch

def simulate_schizo_prediction_error(noise_level=2.0):
    context = torch.zeros(100)
    sensory = torch.randn(100) * 0.1
    prediction = context * 0 + torch.randn(100) * noise_level
    error = sensory - prediction
    return error.abs().mean()
```

---

## 8. Numbers

- ~24M patients globally (2025)
- Suicide rate 5-10% (higher than general population)
- Lifespan < 15-20 years (cardiovascular, suicide)
- Male slight > female

---

## 9. Common Pitfalls

### 9.1 DA Hypothesis Incomplete

Doesn't explain negative + cognitive.

### 9.2 D2 Antagonism Side Effects

Motor + metabolic problems severe.

### 9.3 "Split Personality" Misunderstanding

Schizophrenia ≠ multiple personality disorder.

### 9.4 Early Diagnosis Hard

Prodromal phase only has cognitive abnormalities.

### 9.5 Stigma

Social bias → poor treatment compliance.

---

## 10. Related Concepts

- **Same section**: [Depression](Depression.en.md), [Alzheimer](Alzheimer.en.md), [Parkinson](Parkinson.en.md)
- **Neurotransmitters**: [DA, Glu, GABA](../02_Cellular_Molecular/Neurotransmitters.en.md)
- **Computational**: [Predictive Coding](../05_Computational_Neuroscience/Predictive_Coding.en.md)

---

## References

1. **Howes, O. D. & Kapur, S.** "The dopamine hypothesis of schizophrenia: version III." *Schizophr Bull*, 2009.
2. **Olney, J. W. & Farber, N. B.** "Glutamate receptor dysfunction and schizophrenia." *Arch Gen Psychiatry*, 1995.
3. **Lieberman, J. A. et al.** "Effectiveness of antipsychotic drugs in patients with chronic schizophrenia (CATIE)." *NEJM*, 2005.
4. **Kahn, R. S. et al.** "Schizophrenia." *Nat Rev Dis Primers*, 2015.
