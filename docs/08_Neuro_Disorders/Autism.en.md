# Autism Spectrum Disorder

> *ASD is a neurodevelopmental disorder, prevalence ~1/36 (CDC 2023). Three core features: social communication deficits + restricted/repetitive behaviors + sensory atypicality. Heterogeneous spectrum: from severe disability to high-functioning (Asperger removed 2013). Heritability ~ 80%, multi-gene + environmental multi-factor. No cure; behavioral intervention (ABA) mainstream.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Social Cognition](../04_Cognitive_Neuroscience/Social_Cognition.en.md), [Cortex](../01_Neuroanatomy/Cortex.en.md)

---

## 1. Clinical (DSM-5)

A. Social communication deficits (persistent):
   1. Social-emotional reciprocity
   2. Nonverbal communication
   3. Maintaining relationships
   
B. Restricted, repetitive behaviors / interests / activities (≥ 2):
   1. Stereotyped movements
   2. Insistence on sameness
   3. Intense interests
   4. Sensory hyper/hypo-reactivity

Onset: < 3 years (early markers 9-18 months measurable).

---

## 2. Spectrum Replaces Subtypes

- 2013 DSM-5 merged Asperger, PDD-NOS, Autism into ASD
- Added levels 1-3 (mild → severe)
- Emphasizes spectrum, rejects "low/high functioning" dichotomy

---

## 3. Epidemiology

- US prevalence: 2023 ~ 1/36 (children)
- Male : Female = 4 : 1 (but females under-diagnosed)
- High IQ + ASD: high-functioning autism
- Co-morbidity: ADHD (50%), anxiety, epilepsy, GI issues

---

## 4. Neural Basis (Disputed)

### 4.1 Atypical Connectivity

- Local hyperconnectivity + long-range hypoconnectivity
- Default Mode Network atypical
- Mirror neuron hypothesis (debated)

### 4.2 Brain Overgrowth

- Early brain volume increases (esp. amygdala)
- Later "normalization"

### 4.3 GABA/Glu Imbalance

- E/I balance shifted toward excitation
- Linked to higher epilepsy risk

### 4.4 Theory of Mind Deficit (Baron-Cohen)

- False belief task underperformance
- Recently questioned

---

## 5. Genetics

- Heritability ~ 80%
- 100+ candidate genes (SHANK3, NRXN1, MECP2, CHD8, FMR1, etc.)
- Single-gene ASD (e.g., Fragile X, Rett) ~ 10%
- Polygenic + CNV (copy number variants)

---

## 6. Intervention

### 6.1 Behavioral

- **Applied Behavior Analysis (ABA)**: mainstream, ethical concerns
- **Early Start Denver Model (ESDM)**: ABA + developmental psych
- **PECS**: picture exchange
- **Social skills training**

### 6.2 Pharmacological

- No drug for core symptoms
- Risperidone / Aripiprazole for irritability
- SSRI for anxiety
- Experimental: Oxytocin, bumetanide (GABA shift)

### 6.3 Sensory

- OT (occupational therapy)
- Sensory accommodation (noise-canceling, etc.)

---

## 7. PyTorch — E/I Imbalance Sim

```python
import torch

def simulate_E_I_imbalance(N=100, T=200, E_strength=1.5, I_strength=1.0):
    """Higher E/I ratio → more sustained firing (ASD model)."""
    h = torch.zeros(N)
    W_E = torch.randn(N, N).abs() * E_strength
    W_I = -torch.randn(N, N).abs() * I_strength
    W = W_E + W_I
    W.fill_diagonal_(0)
    
    activity = []
    for _ in range(T):
        h = torch.relu(W @ h + torch.randn(N) * 0.1)
        activity.append(h.mean().item())
    return activity
```

---

## 8. Strengths / Neurodiversity

- Not only "deficits": strong pattern recognition, attention to detail
- Many ASD scientists, engineers (Temple Grandin, Greta Thunberg)
- Neurodiversity movement: autism is variant, not disease

---

## 9. AI Connection

- LLM autistic-coded? (debated, anti-stigma)
- Robots for autism therapy (Kaspar, Nao)
- Eye-tracking + ML for early detection

---

## 10. Common Pitfalls

### 10.1 Vaccines Cause ASD

Completely wrong (Wakefield 1998 fraud retracted).

### 10.2 High-functioning = No Problems

Still has sensory, anxiety, social challenges.

### 10.3 Autism = ToM Deficit

Recent double empathy problem theory: autistic-to-autistic communication fine; mismatch only with NT.

### 10.4 ABA Universal

ABA has ethical critique (forced masking, suppressing traits).

### 10.5 Boys More

Females often overlooked / mis-diagnosed (camouflaging).

---

## 11. Related Concepts

- **Same section**: [Schizophrenia](Schizophrenia.en.md), [Depression](Depression.en.md)
- **Cognition**: [Social Cognition](../04_Cognitive_Neuroscience/Social_Cognition.en.md), [Working Memory](../04_Cognitive_Neuroscience/Working_Memory.en.md)
- **Anatomy**: [Amygdala](../01_Neuroanatomy/Amygdala.en.md)

---

## References

1. **APA** *DSM-5*. 2013.
2. **Baron-Cohen, S.** *Mindblindness*. MIT Press, 1995.
3. **Lord, C. et al.** "Autism spectrum disorder." *Nat Rev Dis Primers*, 2020.
4. **Maenner, M. J. et al.** "Prevalence and characteristics of autism spectrum disorder." *MMWR*, 2023.
