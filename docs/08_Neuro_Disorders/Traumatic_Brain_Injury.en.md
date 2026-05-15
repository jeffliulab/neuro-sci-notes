# Traumatic Brain Injury (TBI)

> *TBI is brain dysfunction from external force, from concussion (mild) to severe. Mechanism: primary injury (impact moment) + secondary injury (hours-days cascade: excitotoxicity, inflammation, ischemia, edema). CTE (chronic traumatic encephalopathy) linked to repetitive TBI. Glasgow Coma Scale grading. The "golden window" to prevent secondary injury is the core of care.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Stroke](Stroke.en.md), [Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.en.md)

---

## 1. Classification

| Severity | GCS | Features |
|---|---|---|
| Mild (concussion) | 13-15 | Brief consciousness/cognitive change, often normal imaging |
| Moderate | 9-12 | Longer coma, abnormal imaging |
| Severe | 3-8 | Long coma, high mortality/disability |

Types: focal (contusion, hematoma) vs diffuse (DAI — diffuse axonal injury).

---

## 2. Primary vs Secondary Injury

```
Primary (impact moment, irreversible):
  contusion, axonal shearing, vascular tearing
       ↓
Secondary (hours-days, treatable!):
  glutamate excitotoxicity → Ca²⁺ influx
  mitochondrial failure, oxidative stress
  neuroinflammation (microglia activation)
  cerebral edema → ICP↑ → ischemia
  BBB disruption
```

→ Treatment window = blocking the secondary cascade.

---

## 3. Excitotoxic Cascade

- Mechanical injury → massive glutamate release
- NMDA over-activation → Ca²⁺ influx
- → proteases / lipases / mitochondrial damage → neuron death
- Overlaps with stroke mechanism (see [Stroke](Stroke.en.md))

---

## 4. Intracranial Pressure (ICP) & Monro-Kellie

- Skull fixed volume = brain + blood + CSF
- Edema / hematoma → ICP ↑ → cerebral perfusion pressure ↓ → ischemia → vicious cycle
- Severe → herniation (fatal)
- Management: osmotic therapy (mannitol/hypertonic saline), drainage, decompressive craniectomy

---

## 5. PyTorch — Monro-Kellie ICP

```python
import numpy as np

def icp_dynamics(edema_volume, baseline_icp=10):
    """Exponential pressure-volume curve (compliance exhausts)."""
    E = 0.15  # elastance coefficient
    icp = baseline_icp * np.exp(E * edema_volume)
    cpp = 80 - icp           # cerebral perfusion pressure (MAP~80)
    ischemia = cpp < 50      # critical threshold
    return icp, cpp, ischemia
```

---

## 6. Concussion (mTBI)

- No structural injury but functional disturbance (metabolic, ionic, axonal microdamage)
- "Neurometabolic cascade" (K⁺ efflux → energy crisis)
- Most recover 7-10 days; few have persistent post-concussion syndrome
- Second-impact syndrome: re-hit before recovery → catastrophic brain swelling

---

## 7. CTE (Chronic Traumatic Encephalopathy)

- Repetitive TBI/subconcussion (boxing, football, military)
- Tau proteinopathy (perivascular p-tau)
- Behavioral/cognitive/emotional decline (years-decades later)
- Confirmed only at autopsy (antemortem biomarkers under research)
- Major issue in sports

---

## 8. Treatment

- **Acute**: ABC + prevent secondary (control ICP, oxygenation, blood pressure, avoid hypoglycemia/hyperthermia)
- **Surgery**: hematoma evacuation, decompressive craniectomy
- **Neuroprotective drugs**: many trials **failed** (glutamate antagonists etc.) → clinical care remains mainly supportive
- **Rehabilitation**: cognitive / physical / occupational (core in chronic phase)

---

## 9. Long-Term Consequences

- Cognition (attention, memory, executive), mood (depression, irritability), PTSD comorbidity
- Increased epilepsy risk (post-traumatic epilepsy)
- Increased neurodegeneration risk (AD, PD, CTE)
- Chronic disability (esp. young people)

---

## 10. Common Pitfalls

### 10.1 Normal Imaging = Fine

mTBI often has normal imaging but functional disturbance; not trivial.

### 10.2 Primary Injury Treatable

Primary is irreversible; treatment targets the secondary cascade.

### 10.3 Immediate Return-to-Play After Concussion Safe

Second-impact syndrome can be fatal; graded return required.

### 10.4 Neuroprotective Drugs Mature

Dozens of Phase III failures; no effective neuroprotectant currently.

### 10.5 CTE Diagnosable in Life

Currently autopsy-only; antemortem diagnosis still research-stage.

---

## 11. Related Concepts

- **Same section**: [Stroke](Stroke.en.md), [Epilepsy](Epilepsy.en.md), [Alzheimer](Alzheimer.en.md) (tau)
- **Cellular**: [Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.en.md) (glutamate excitotoxicity)
- **Foundation**: [Blood Brain Barrier](../00_Foundations/Blood_Brain_Barrier.en.md)

---

## References

1. **Maas, A. I. R. et al.** "Traumatic brain injury: integrated approaches to improve prevention, clinical care, and research." *Lancet Neurol*, 2017.
2. **Giza, C. C. & Hovda, D. A.** "The new neurometabolic cascade of concussion." *Neurosurgery*, 2014.
3. **McKee, A. C. et al.** "The spectrum of disease in chronic traumatic encephalopathy." *Brain*, 2013.
4. **Jennett, B. & Teasdale, G.** "Assessment of coma and impaired consciousness (GCS)." *Lancet*, 1974.
