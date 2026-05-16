# Hypothalamus & Homeostasis

> *The hypothalamus (< 1% of brain weight) is the master homeostatic dispatcher: temperature, feeding, drinking, reproduction, stress, circadian, sleep. Mechanism: set point + negative feedback + neuro-endocrine (pituitary) + autonomic output. Allostasis (predictive regulation) revises the classic homeostasis view. Leptin/ghrelin → obesity; HPA axis → stress. The "thermostat" of survival.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Autonomic_Nervous_System](Autonomic_Nervous_System.en.md), [Brainstem](../01_Neuroanatomy/Brainstem.en.md)

---

## 1. Nuclei → Function (selected)

| Nucleus | Function |
|---|---|
| Supraoptic/paraventricular (SON/PVN) | ADH, oxytocin; CRH (stress) |
| Suprachiasmatic (SCN) | Circadian master clock (see [Circadian](Circadian_System.en.md)) |
| Arcuate (ARC) | Feeding (leptin/ghrelin sensing) |
| Ventromedial (VMH) | "Satiety center" |
| Lateral hypothalamus (LH) | "Feeding/arousal" (orexin) |
| Preoptic area (POA) | Temperature, sleep |
| Mammillary bodies | Memory (Papez) |

---

## 2. Three Output Pathways

1. **Autonomic**: → brainstem/spinal → sympathetic/parasympathetic (see [ANS](Autonomic_Nervous_System.en.md))
2. **Endocrine**: → pituitary
   - Posterior: directly releases ADH/oxytocin
   - Anterior: releasing/inhibiting hormones (CRH→ACTH, GnRH, TRH, GHRH...)
3. **Behavioral**: → limbic/cortex → motivated behavior (foraging/drinking)

---

## 3. Homeostasis = Negative Feedback + Set Point

```
Disturbance → receptors (osmotic/temperature/leptin/glucose)
   → hypothalamus compares set point
   → error → autonomic + endocrine + behavioral correction
   → feedback
```

E.g.: body temp ↑ → POA → sweating + vasodilation + behavior (remove clothes).

---

## 4. PyTorch — Homeostatic Controller

```python
import torch

def homeostatic_controller(state, set_point, gain=0.5, T=100):
    """Negative feedback toward set point (e.g., body temperature)."""
    traj = []
    for _ in range(T):
        error = set_point - state
        correction = gain * error            # autonomic + behavioral output
        state = state + correction + 0.05*torch.randn(1).item()  # + disturbance
        traj.append(state)
    return traj   # converges to set_point
```

---

## 5. Feeding Regulation

- **Leptin**: fat → ARC → suppresses feeding (long-term energy store signal); obesity = leptin resistance
- **Ghrelin**: stomach → promotes feeding (rises pre-meal)
- **Insulin, PYY, GLP-1**: satiety
- AgRP/POMC neurons (ARC) antagonistic regulation
- GLP-1 agonists (semaglutide) = weight-loss drug breakthrough

---

## 6. Allostasis (Predictive Regulation)

- Classic homeostasis: passive return to set point
- **Allostasis** (Sterling): **predictive** regulation, set point variable + anticipatory
- "Allostatic load": chronic predictive regulation wear → disease
- Connects with active inference / interoception (see [Interoception](Interoception.en.md))

---

## 7. HPA Axis (Stress)

- Stress → PVN CRH → pituitary ACTH → adrenal cortisol
- Cortisol negative feedback (hippocampus/PFC → suppress HPA)
- Chronic stress → HPA dysregulation → depression/metabolic/immune (see [Depression](../08_Neuro_Disorders/Depression.en.md))

---

## 8. Pathology

- **Obesity / anorexia**: feeding circuit imbalance
- **Diabetes insipidus**: ADH deficiency (hypothalamus/pituitary)
- **Hypothalamic amenorrhea**: GnRH suppression (stress/low energy)
- **Prader-Willi**: hyperphagia (hypothalamus)
- **Kleine-Levin**, hypothalamic tumors → sleep/feeding/temperature disturbance
- Fever = upregulated set point (prostaglandins, not homeostatic failure)

---

## 9. Relation to AI

- Homeostasis ↔ cybernetics (Wiener), PID (see eng-notes control)
- **Homeostatic RL**: internal needs as reward source (drive-reduction → modern Keramati)
- Allostasis ↔ model-predictive control
- Embodied agent's "survival goal" = homeostasis (see [Embodied_Cognition](../04_Cognitive_Neuroscience/Embodied_Cognition.en.md))

---

## 10. Common Pitfalls

### 10.1 Hypothalamus Small → Unimportant

< 1% brain weight but regulates all survival functions.

### 10.2 Homeostasis = Fixed Set Point

Allostasis: set point variable + predictive.

### 10.3 Fever = Temperature Regulation Failure

It's active set point upregulation (immune strategy), not loss of control.

### 10.4 More Leptin → Not Obese

Obesity often leptin **resistance** (high leptin yet hyperphagic).

### 10.5 One Nucleus One Function

Nuclei functions overlap + distributed; not strict one-to-one.

---

## 11. Related Concepts

- **Same section**: [Autonomic_Nervous_System](Autonomic_Nervous_System.en.md), [Interoception](Interoception.en.md), [Circadian_System](Circadian_System.en.md), [Sleep_Wake](Sleep_Wake.en.md)
- **Anatomy**: [Brainstem](../01_Neuroanatomy/Brainstem.en.md)
- **Disease**: [Depression](../08_Neuro_Disorders/Depression.en.md) (HPA)
- **Computational**: [Reinforcement Learning Brain](../05_Computational_Neuroscience/Reinforcement_Learning_Brain.en.md) (homeostatic RL)

---

## References

1. **Saper, C. B. & Lowell, B. B.** "The hypothalamus." *Curr Biol*, 2014.
2. **Sterling, P.** "Allostasis: a model of predictive regulation." *Physiol Behav*, 2012.
3. **Friedman, J. M.** "Leptin and the endocrine control of energy balance." *Nat Metab*, 2019.
4. **McEwen, B. S.** "Stress, adaptation, and disease: allostasis and allostatic load." *Ann NY Acad Sci*, 1998.
