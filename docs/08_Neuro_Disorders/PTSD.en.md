# Post-Traumatic Stress Disorder (PTSD)

> *PTSD is persistent re-experiencing + avoidance + hyperarousal + negative cognition after a traumatic event. Mechanism: over-strong fear memory consolidation + extinction failure + amygdala hyperactivity + insufficient hippocampus/vmPFC regulation. Trauma-focused CBT / EMDR first-line; MDMA-assisted therapy breakthrough trials.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Amygdala](../01_Neuroanatomy/Amygdala.en.md), [Hippocampus_Memory](../03_Systems_Neuroscience/Hippocampus_Memory.en.md)

---

## 1. Clinical (DSM-5)

Trauma exposure + 4 symptom clusters (> 1 month):
1. **Intrusion**: flashbacks, nightmares, intrusive memories
2. **Avoidance**: avoid related cues
3. **Negative cognition/mood**: negative beliefs, numbing, dissociation
4. **Arousal/reactivity**: hypervigilance, startle, irritability, poor sleep

---

## 2. Epidemiology

- Lifetime prevalence ~ 6-8% (~ 10-20% of trauma-exposed develop PTSD)
- Female > Male
- Risk: trauma severity, prior trauma, lack of social support, genetics
- Most trauma-exposed do **not** develop PTSD (resilience is the norm)

---

## 3. Neural Basis

```
Amygdala overactive (fear)
  ↑ insufficient vmPFC regulation (extinction failure)
Hippocampus ↓ (poor contextualization → generalization)
```

- **Amygdala** hyperreactivity
- **vmPFC** insufficient inhibition / extinction
- **Hippocampus** ↓ volume + ↓ function → poor context discrimination → over-generalization
- HPA axis abnormality (peculiar cortisol pattern — often low + hypersensitive)

---

## 4. Memory View

- Trauma memory = over-strong consolidation (NE/cortisol enhance amygdala encoding)
- Extinction learning failure (vmPFC-amygdala)
- **Reconsolidation**: recall makes memory briefly plastic → therapeutic window
- Flashbacks = involuntary, sensory, no time tag (weak hippocampus)

---

## 5. PyTorch — Over-Strong Fear Consolidation

```python
import torch

def trauma_consolidation(stress_hormone=1.0, baseline=0.3):
    """Cortisol/NE amplify amygdala memory encoding strength."""
    encoding_strength = baseline * (1 + 2.0 * stress_hormone)
    extinction_rate = 0.5 / (1 + stress_hormone)   # high stress -> poor extinction
    return encoding_strength, extinction_rate

# PTSD: high stress_hormone -> over-strong memory + weak extinction
```

---

## 6. Treatment

### 6.1 Psychological (First-Line)

- **Trauma-focused CBT**: prolonged exposure, cognitive processing therapy
- **EMDR** (eye movement desensitization) — effective, mechanism debated
- Core: extinction + memory reconsolidation

### 6.2 Medication

- **SSRI / SNRI** (sertraline, paroxetine FDA-approved)
- **Prazosin** (α1 antagonist) → improves nightmares
- Benzo **contraindicated** (worsens + blocks extinction)

### 6.3 Breakthrough

- **MDMA-assisted therapy** (Phase 3 positive, reduces amygdala fear + enhances therapeutic alliance)
- **Propranolol** + recall → block reconsolidation (mixed results)
- Ketamine, cannabinoid under research

---

## 7. Resilience

- Most people don't develop PTSD after trauma
- Protective: social support, cognitive flexibility, NPY, safety learning
- It's the norm not the exception → studying resilience as important as pathology

---

## 8. Comorbidities + Consequences

- Depression, substance abuse, chronic pain
- Suicide risk ↑
- Physical health (cardiovascular, inflammation)
- Complex PTSD (prolonged repeated trauma → adds emotion regulation + self + relationship disturbance)

---

## 9. Controversies

- Whether EMDR eye movements are necessary (vs exposure alone)
- "Repressed/recovered" trauma memory debate (false memory risk)
- Diagnostic expansion
- Psychological debriefing (immediate post-event) may be harmful

---

## 10. Common Pitfalls

### 10.1 Trauma Always Causes PTSD

Most are resilient; PTSD is the minority.

### 10.2 Benzo Helps

Worsens + blocks extinction → contraindicated.

### 10.3 Flashback = Ordinary Memory

It's involuntary, sensory, lacks time tag (weak hippocampus).

### 10.4 Must Recount Trauma to Heal

Forced debriefing can harm; therapy needs structure + timing.

### 10.5 MDMA Is Recreational Use

Therapeutic use is controlled + with psychotherapy, mechanism differs from abuse.

---

## 11. Related Concepts

- **Same section**: [Anxiety_Disorders](Anxiety_Disorders.en.md), [Depression](Depression.en.md)
- **Anatomy**: [Amygdala](../01_Neuroanatomy/Amygdala.en.md)
- **Systems**: [Hippocampus_Memory](../03_Systems_Neuroscience/Hippocampus_Memory.en.md), [Sleep/Wake](../03_Systems_Neuroscience/Sleep_Wake.en.md)

---

## References

1. **Yehuda, R. et al.** "Post-traumatic stress disorder." *Nat Rev Dis Primers*, 2015.
2. **Milad, M. R. & Quirk, G. J.** "Fear extinction as a model for translational neuroscience." *Annu Rev Psychol*, 2012.
3. **Mitchell, J. M. et al.** "MDMA-assisted therapy for severe PTSD (Phase 3)." *Nat Med*, 2021.
4. **Pitman, R. K. et al.** "Biological studies of post-traumatic stress disorder." *Nat Rev Neurosci*, 2012.
