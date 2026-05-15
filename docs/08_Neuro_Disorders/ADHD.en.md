# ADHD (Attention-Deficit/Hyperactivity Disorder)

> *ADHD is a neurodevelopmental disorder, ~ 5-7% in children, ~ 2.5% in adults. Core: inattention + hyperactivity/impulsivity. Mechanism: prefrontal-striatal dopamine/norepinephrine dysregulation + executive function deficits. Stimulants (methylphenidate/amphetamine) are counterintuitively effective. Highly heritable (~ 74%). Often persists into adulthood (debated).*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Working Memory](../04_Cognitive_Neuroscience/Working_Memory.en.md), [Reward System](../03_Systems_Neuroscience/Reward_System.en.md)

---

## 1. Clinical (DSM-5)

Two symptom dimensions (onset < 12 years, multiple settings):
- **Inattention**: careless, distractible, loses things, can't sustain
- **Hyperactivity/Impulsivity**: restless, talkative, interrupts, can't wait

Three subtypes: predominantly inattentive (old "ADD"), hyperactive-impulsive, combined.

---

## 2. Epidemiology

- Children ~ 5-7%, adults ~ 2.5%
- Male : Female ~ 2-3 : 1 (females easily missed — more inattentive type)
- ~ 50-65% symptoms persist into adulthood
- High comorbidity: learning disorders, ODD, anxiety, depression, substance abuse

---

## 3. Neural Basis

- **DA / NE dysregulation**: prefrontal-striatal-cerebellar circuit
- **Executive function deficits**: working memory, inhibitory control, planning
- **Delay aversion**: reward circuit → prefers immediate small reward
- **DMN regulation abnormality**: insufficient DMN suppression during task → mind-wandering
- Imaging: PFC, striatum volume/activity ↓; delayed maturation (not deficit) hypothesis

---

## 4. Genetics

- Heritability ~ 74% (high in psychiatry)
- Polygenic: DAT1, DRD4, DRD5 etc. (small effects)
- Environment: prematurity, low birth weight, prenatal smoking/alcohol/lead exposure

---

## 5. Stimulants Paradoxically Effective

- Methylphenidate (Ritalin), Amphetamine (Adderall)
- Block DAT/NET → synaptic DA/NE ↑ → **improve** PFC signal-to-noise
- "Stimulants calm the hyperactive" = inverted-U (optimal DA level, Arnsten)
- Not sedation; optimizes PFC catecholamine

---

## 6. PyTorch — Inverted-U DA-Performance Curve

```python
import torch

def inverted_u_performance(da_level):
    """PFC function vs catecholamine: inverted-U (Arnsten)."""
    optimal = 1.0
    return torch.exp(-((da_level - optimal) ** 2) / 0.3)

# ADHD: low baseline DA -> stimulant moves toward optimal -> better
# Too much (overdose) -> past peak -> worse
```

---

## 7. Treatment

### 7.1 Medication

- **Stimulants** (first-line): methylphenidate, amphetamine
- **Non-stimulants**: atomoxetine (NET inhibition), guanfacine/clonidine (α2 agonist)
- Stimulant abuse / diversion risk (controlled)

### 7.2 Non-Pharmacological

- Behavioral therapy (first choice esp. young children)
- Parent training, school support
- CBT (adult executive skills)
- Environmental structuring

---

## 8. Executive Function Model

- Barkley: ADHD core is **behavioral inhibition** deficit → affects working memory, self-regulation, internalized speech
- Sonuga-Barke dual pathway: executive dysfunction + delay aversion
- Closely linked to [Working Memory](../04_Cognitive_Neuroscience/Working_Memory.en.md)

---

## 9. Controversies

- Overdiagnosis / overmedication (large regional variation)
- School relative-age effect (youngest in class more likely diagnosed)
- Adult ADHD boundary
- "Disease or neurodiversity spectrum"

---

## 10. Common Pitfalls

### 10.1 ADHD = Lazy / Poorly Disciplined

A neurodevelopmental disorder, not a moral / upbringing issue.

### 10.2 Stimulants Make People High

At therapeutic doses ADHD patients improve focus, not euphoria; inverted-U optimization.

### 10.3 Outgrown in Adulthood

~ Half persist into adulthood (presentation shifts to internal inattention).

### 10.4 Only Affects Boys

Females more inattentive type, underdiagnosed; not truly lower incidence.

### 10.5 Hyperactivity Is the Core

Inattention + executive dysfunction often more core, more persistent.

---

## 11. Related Concepts

- **Same section**: [Autism](Autism.en.md), [Bipolar_Disorder](Bipolar_Disorder.en.md), [Depression](Depression.en.md)
- **Cognition**: [Working Memory](../04_Cognitive_Neuroscience/Working_Memory.en.md), [Attention](../04_Cognitive_Neuroscience/Attention.en.md)
- **Systems**: [Reward System](../03_Systems_Neuroscience/Reward_System.en.md), [Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.en.md)

---

## References

1. **Faraone, S. V. et al.** "Attention-deficit/hyperactivity disorder." *Nat Rev Dis Primers*, 2015.
2. **Arnsten, A. F. T.** "Catecholamine influences on dorsolateral prefrontal cortical networks." *Biol Psychiatry*, 2011.
3. **Barkley, R. A.** "Behavioral inhibition, sustained attention, and executive functions." *Psychol Bull*, 1997.
4. **Sonuga-Barke, E. J. S.** "The dual pathway model of AD/HD." *Neurosci Biobehav Rev*, 2003.
