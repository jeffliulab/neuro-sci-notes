# Obsessive-Compulsive Disorder (OCD)

> *OCD is a chronic illness of obsessions (intrusive thoughts) + compulsions (repetitive behaviors to reduce anxiety), ~ 2-3% prevalence. Core circuit: hyperactive cortico-striato-thalamo-cortical (CSTC) loop — OFC/ACC + caudate + thalamus. SSRI (high-dose) + ERP (exposure response prevention) first-line; DBS for refractory.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.en.md), [Anxiety_Disorders](Anxiety_Disorders.en.md)

---

## 1. Clinical

- **Obsessions**: intrusive, unwanted, anxiety-provoking (contamination, harm, symmetry, taboo thoughts)
- **Compulsions**: repetitive behaviors/mental acts to reduce anxiety (washing, checking, counting, ordering)
- Insight (most know it's irrational but can't resist)
- Time-consuming (> 1 h/day) + impairs function

---

## 2. Epidemiology

- Prevalence ~ 2-3%
- Onset: adolescence-early adulthood (bimodal: childhood + 20s)
- Boys earlier onset; adults gender-balanced
- Comorbidity: depression, tic/Tourette, anxiety

---

## 3. CSTC Circuit Model

```
OFC / ACC (excessive "error" signal)
   ↓
Caudate / striatum
   ↓ direct vs indirect imbalance
Thalamus (insufficient gating)
   ↓
back to cortex (reinforcing loop)
```

- "Cortico-striato-thalamo-cortical" loop hyperactivity
- OFC/ACC excessive error/threat signal
- Imaging: OFC, caudate, ACC metabolism↑, decreases after treatment

---

## 4. Neurotransmitters

- **5-HT**: SSRI (higher than depression doses) effective → 5-HT hypothesis
- **Glutamate**: CSTC excitability → experimental glutamate modulation (memantine, riluzole)
- **DA**: antipsychotic augmentation (esp. with tics)

---

## 5. Computational View

- **Excessive error/uncertainty signal**: can't reach "enough" certainty → repetition
- **Goal-directed vs habit imbalance**: over-reliance on habit system (dorsal striatum)
- **Harm avoidance** learning abnormality
- Related to RL/model-based imbalance (see [Reinforcement Learning Brain](../05_Computational_Neuroscience/Reinforcement_Learning_Brain.en.md))

---

## 6. PyTorch — Non-Terminating Checking Loop

```python
import torch

def ocd_loop(uncertainty_gain=2.0, threshold=0.95, max_iter=50):
    """Compulsion: repeat until 'certainty' reached, but gain too high."""
    certainty = 0.3
    checks = 0
    while certainty < threshold and checks < max_iter:
        # Each check adds info but pathological discounting prevents closure
        gain = 0.1 / (1 + uncertainty_gain)   # high gain -> tiny increments
        certainty += gain
        checks += 1
    return checks   # high uncertainty_gain -> many repetitions

# OCD: uncertainty_gain high -> never feels 'sure' -> compulsive checking
```

---

## 7. Treatment

### 7.1 Psychological (First-Line)

- **ERP** (Exposure & Response Prevention): expose trigger + block compulsion → extinction
- Most effective psychotherapy

### 7.2 Medication

- **High-dose SSRI** (above antidepressant, slow onset 8-12 weeks): fluoxetine, fluvoxamine, sertraline
- **Clomipramine** (TCA, strong 5-HT)
- Augmentation: atypical antipsychotic (aripiprazole/risperidone)

### 7.3 Refractory

- **DBS**: VC/VS, STN targets (see [DBS](../07_Neurotech_Frontiers/DBS.en.md))
- Historically: cingulotomy/capsulotomy (lesion, cautious)
- TMS

---

## 8. Related Spectrum

- Tourette / tic (high comorbidity, shared CSTC)
- Hoarding disorder (DSM-5 separate)
- Body dysmorphic disorder
- Trichotillomania, skin picking
- PANDAS (post-streptococcal pediatric acute-onset OCD, autoimmune hypothesis, debated)

---

## 9. Distinction from Anxiety

- DSM-5 moved OCD out of anxiety disorders into its own category
- Commonality: anxiety-driven; difference: CSTC loop + compulsion structure
- Insight + ritualization distinguish from delusion / GAD

---

## 10. Common Pitfalls

### 10.1 = Cleanliness / Perfectionism

A function-impairing clinical disorder, not a personality trait.

### 10.2 Normal SSRI Dose Enough

OCD needs higher dose + longer onset (8-12 weeks).

### 10.3 Compulsion Brings Pleasure

It's anxiety reduction (negative reinforcement), not pleasure.

### 10.4 = Anxiety Disorder

DSM-5 separately classified; CSTC mechanism specific.

### 10.5 Patients Don't Know It's Irrational

Most have insight (distinguishes from delusion).

---

## 11. Related Concepts

- **Same section**: [Anxiety_Disorders](Anxiety_Disorders.en.md), [Depression](Depression.en.md)
- **Anatomy**: [Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.en.md)
- **Computational**: [Reinforcement Learning Brain](../05_Computational_Neuroscience/Reinforcement_Learning_Brain.en.md)
- **Frontiers**: [DBS](../07_Neurotech_Frontiers/DBS.en.md)

---

## References

1. **Stein, D. J. et al.** "Obsessive-compulsive disorder." *Nat Rev Dis Primers*, 2019.
2. **Graybiel, A. M. & Rauch, S. L.** "Toward a neurobiology of obsessive-compulsive disorder." *Neuron*, 2000.
3. **Pauls, D. L. et al.** "Obsessive-compulsive disorder: an integrative genetic and neurobiological perspective." *Nat Rev Neurosci*, 2014.
4. **Gillan, C. M. & Robbins, T. W.** "Goal-directed learning and obsessive-compulsive disorder." *Phil Trans R Soc B*, 2014.
