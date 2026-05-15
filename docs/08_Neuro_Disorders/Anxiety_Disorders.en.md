# Anxiety Disorders

> *Anxiety disorders are the most common psychiatric disorders (lifetime ~ 30%). Include GAD, panic disorder, social anxiety, specific phobia, agoraphobia. Core circuit: amygdala hyperreactivity + insufficient PFC regulation. SSRI/SNRI + CBT first-line. A dysregulation of the adaptive fear system, not mere "overthinking."*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Amygdala](../01_Neuroanatomy/Amygdala.en.md), [Emotion](../04_Cognitive_Neuroscience/Emotion.en.md)

---

## 1. Classification (DSM-5)

| Type | Feature |
|---|---|
| GAD | Persistent generalized worry (≥ 6 mo) |
| Panic disorder | Recurrent panic attacks + anticipatory anxiety |
| Social anxiety | Fear of being judged |
| Specific phobia | Specific object (snake, height, needle) |
| Agoraphobia | Fear of hard-to-escape places |
| Separation anxiety | Separation fear |

(OCD, PTSD now separately classified in DSM-5)

---

## 2. Epidemiology

- Lifetime prevalence ~ 30% (highest category)
- Female : Male ~ 2 : 1
- Early onset (childhood-youth)
- High comorbidity: depression, substance abuse

---

## 3. Neural Circuit

```
Threat → Amygdala (over-activation)
            ↑ insufficient regulation
          vmPFC / dlPFC
          
Amygdala → hypothalamus (HPA, cortisol)
         → brainstem (HR↑, breathing↑)
         → BNST (sustained anxiety vs acute fear)
```

- Amygdala hyperreactivity + weak PFC top-down regulation
- BNST: sustained / uncertain threat (distinct from phobia acute fear)
- Insula: interoception → panic

---

## 4. Neurotransmitters

- **GABA**: insufficient inhibition → benzodiazepine (GABA-A positive allosteric) works
- **5-HT**: SSRI long-term modulation
- **NE**: locus coeruleus overactivity → panic
- **Glutamate**, CRF, neuropeptide Y

---

## 5. Fear vs Anxiety

| | Fear | Anxiety |
|---|---|---|
| Trigger | Clear / immediate threat | Uncertain / future |
| Circuit | central amygdala | BNST (extended amygdala) |
| Time course | Phasic (fast) | Sustained |
| Behavior | Fight / flight | Vigilance / avoidance |

---

## 6. PyTorch — Amygdala-PFC Regulation Imbalance

```python
import torch

def anxiety_circuit(threat, pfc_control=1.0, T=100):
    """High amygdala gain + weak PFC regulation -> sustained arousal."""
    amy = 0.0
    arousal = []
    for t in range(T):
        drive = threat - pfc_control * amy        # PFC inhibits amygdala
        amy = torch.relu(torch.tensor(amy + 0.1 * drive))
        arousal.append(amy.item())
    return arousal   # weak pfc_control -> runaway anxiety

# anxiety disorder: pfc_control low -> amygdala unchecked
```

---

## 7. Treatment

### 7.1 Psychological (First-Line)

- **CBT**: cognitive restructuring + exposure (extinction learning — see [Amygdala](../01_Neuroanatomy/Amygdala.en.md))
- Exposure most effective for phobia/panic

### 7.2 Medication

- **SSRI / SNRI**: first-line (onset 2-6 weeks)
- **Benzodiazepines**: acute use, dependence risk → not long-term
- **Buspirone** (5-HT1A), **β-blocker** (performance anxiety somatic symptoms)
- **Pregabalin** (GAD)

### 7.3 Experimental

- D-cycloserine enhances exposure (NMDA partial agonist)
- Transcranial stimulation, digital CBT

---

## 8. Learning View

- Phobia/panic = abnormal fear conditioning + extinction failure
- Exposure = new inhibitory learning (doesn't erase old memory)
- Avoidance maintains anxiety (blocks extinction)
- Related to RL / extinction computational models

---

## 9. Adaptive vs Pathological

- Anxiety is fundamentally adaptive (warning + preparation)
- Pathological = excessive / dysregulated / disproportionate to threat / impairs function
- "Smoke detector principle" (Nesse): false-positive cost < false-negative

---

## 10. Common Pitfalls

### 10.1 = Overthinking / Weak Character

A medical condition of fear-system dysregulation, not a character flaw.

### 10.2 Benzo Good Long-Term

Tolerance + dependence + cognitive impairment; short-term / acute only.

### 10.3 Avoidance Relieves

Short-term relief, long-term maintenance + worsening (blocks extinction).

### 10.4 Fear = Anxiety

Circuit + time course differ (amygdala vs BNST).

### 10.5 SSRI Immediate Effect

Anxiolysis takes 2-6 weeks; may briefly worsen early.

---

## 11. Related Concepts

- **Same section**: [Depression](Depression.en.md), [Bipolar_Disorder](Bipolar_Disorder.en.md)
- **Anatomy**: [Amygdala](../01_Neuroanatomy/Amygdala.en.md)
- **Cognition**: [Emotion](../04_Cognitive_Neuroscience/Emotion.en.md)
- **Cellular**: [Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.en.md) (GABA, 5-HT)

---

## References

1. **Craske, M. G. et al.** "Anxiety disorders." *Nat Rev Dis Primers*, 2017.
2. **Davis, M. et al.** "Phasic vs sustained fear in rats and humans: role of the extended amygdala in fear vs anxiety." *Neuropsychopharmacology*, 2010.
3. **Nesse, R. M.** "The smoke detector principle." *Ann NY Acad Sci*, 2001.
4. **Bystritsky, A. et al.** "Current diagnosis and treatment of anxiety disorders." *P&T*, 2013.
