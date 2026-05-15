# Bipolar Disorder

> *Bipolar disorder is a chronic illness with mood swinging between mania/hypomania and depression, ~ 1-2% prevalence, highly heritable (~ 70-80%). Bipolar I (full mania) vs Bipolar II (hypomania + major depression). Lithium, discovered 1949, remains the gold standard. Mechanism: rhythm + neuroplasticity + ion dysregulation, far from understood. Very high suicide risk.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Depression](Depression.en.md), [Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.en.md)

---

## 1. Clinical (DSM-5)

- **Manic episode** (≥ 1 week): elevated/irritable mood + ↑ energy + ↓ sleep need + grandiosity + impulsivity (spending/sex/investment)
- **Hypomanic** (≥ 4 days, milder, no marked functional impairment)
- **Major depressive episode**
- **Bipolar I**: ≥ 1 mania
- **Bipolar II**: hypomania + major depression (no full mania)
- **Cyclothymia**: subclinical fluctuation ≥ 2 years

---

## 2. Epidemiology

- Prevalence ~ 1-2% (BP I + II)
- Onset: late adolescence-25 years
- Avg diagnostic delay ~ 8-10 years (often misdiagnosed as unipolar depression)
- Suicide rate: lifetime ~ 15-20× general population

---

## 3. Genetics

- Heritability ~ 70-80% (among the highest in psychiatry)
- Polygenic + CACNA1C (calcium channel), ANK3, ODZ4
- Partially shares genetic risk with schizophrenia
- Strong family clustering

---

## 4. Neural Basis (Hypotheses)

- **Ion / calcium channels**: CACNA1C → excitability dysregulation
- **Rhythm dysregulation**: circadian + sleep disturbance often triggers episodes
- **Neuroplasticity**: BDNF, mitochondrial function
- **Monoamines + glutamate**: complex, not single
- Imaging: amygdala-PFC regulatory circuit abnormality

---

## 5. Lithium — Gold Standard

- Cade 1949 serendipitous discovery
- Reduces mania + anti-suicide (unique evidence)
- Mechanism unclear: GSK-3β inhibition, inositol depletion, neuroprotection
- Narrow therapeutic window (0.6-1.2 mmol/L) → blood monitoring needed (renal/thyroid toxicity)

---

## 6. PyTorch — Bistable Mood Model

```python
import torch

def bipolar_dynamics(T=2000, dt=0.01, instability=1.3, noise=0.15):
    """Double-well mood model: low instability=stable, high=oscillates."""
    mood = 0.0
    traj = []
    for _ in range(T):
        # dV/dx of double-well; high instability -> switches states
        force = instability * mood - mood**3
        mood += dt * force + noise * torch.randn(1).item() * (dt**0.5)
        traj.append(mood)
    return traj   # bistable: mania (+) <-> depression (-)
```

---

## 7. Treatment

### 7.1 Mood Stabilizers

- **Lithium** (first-line, anti-suicide)
- **Valproate**, **Lamotrigine** (more anti-depressive pole), **Carbamazepine**

### 7.2 Atypical Antipsychotics

- Quetiapine, Olanzapine, Aripiprazole, Lurasidone
- Treat acute mania + maintenance

### 7.3 Caution

- **Antidepressant monotherapy dangerous**: can induce mania / rapid cycling → must combine with mood stabilizer
- **ECT**: refractory / acute severe (highly effective)
- Psychological: psychoeducation, rhythm stabilization (IPSRT), regular sleep

---

## 8. Triggers

- Sleep deprivation (potent mania inducer)
- Season (spring/summer mania, autumn/winter depression tendency)
- Stressful events
- Antidepressants / stimulants
- Substance abuse

---

## 9. Comorbidities

- Anxiety disorders, substance abuse (high)
- ADHD (esp. adolescents, hard to differentiate)
- Metabolic syndrome (drug + illness itself)
- Cardiovascular disease (shortened lifespan)

---

## 10. Common Pitfalls

### 10.1 = Moodiness / Temperamental

A clinical syndrome (episodes lasting days-weeks), not everyday mood swings.

### 10.2 Antidepressants Safe

Monotherapy can induce mania / rapid cycling; must use mood stabilizer protection.

### 10.3 Bipolar II Milder

Depressive pole often longer + more disabling; suicide risk not low.

### 10.4 Lithium Outdated

Still anti-suicide + maintenance gold standard; no better replacement.

### 10.5 Mania = Happiness

Often irritable + distressing + severe consequences (not pleasurable).

---

## 11. Related Concepts

- **Same section**: [Depression](Depression.en.md), [Schizophrenia](Schizophrenia.en.md)
- **Cellular**: [Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.en.md), [Ion Channels](../02_Cellular_Molecular/Ion_Channels.en.md)
- **Systems**: [Sleep/Wake](../03_Systems_Neuroscience/Sleep_Wake.en.md), [Reward System](../03_Systems_Neuroscience/Reward_System.en.md)

---

## References

1. **Grande, I. et al.** "Bipolar disorder." *Lancet*, 2016.
2. **Cade, J. F. J.** "Lithium salts in the treatment of psychotic excitement." *Med J Aust*, 1949.
3. **Geddes, J. R. & Miklowitz, D. J.** "Treatment of bipolar disorder." *Lancet*, 2013.
4. **Craddock, N. & Sklar, P.** "Genetics of bipolar disorder." *Lancet*, 2013.
