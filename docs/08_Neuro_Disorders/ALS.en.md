# ALS (Amyotrophic Lateral Sclerosis)

> *ALS is the main motor neuron disease. Both upper motor neuron (cortex) + lower (anterior horn) degenerate → progressive muscle weakness → respiratory failure death in 3-5 years. Stephen Hawking lived 50+ years with ALS (atypical). BCI is now key technology for ALS patient communication.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Motor System](../03_Systems_Neuroscience/Motor_System.en.md), [Neuron](../02_Cellular_Molecular/Neuron.en.md)

---

## 1. Clinical

- Progressive muscle weakness + atrophy
- Doesn't affect sensory, cognitive (most cases)
- Usually onset 50-65
- Onset to death ~3-5 years (without respiratory support)
- ~300k patients globally

---

## 2. Pathology

```
Upper motor neuron (Betz cells in M1) degenerates
  ↓
Lower motor neuron (anterior horn) degenerates
  ↓
Muscle denervation → atrophy + fasciculation
  ↓
Eventually total paralysis + respiratory muscle failure
```

---

## 3. Types

- **Sporadic** (90%): unknown cause
- **Familial** (10%): multiple gene mutations
  - SOD1 (~20% familial)
  - C9orf72 (~40% familial — also involved in frontotemporal dementia)
  - FUS, TARDBP

---

## 4. Pathogenesis Hypotheses

- Glutamate excitotoxicity
- Mitochondrial dysfunction
- Oxidative stress
- Protein misfolding + aggregation (TDP-43)
- Neuroinflammation (microglia)
- Axonal transport defects

---

## 5. Diagnosis

- Clinical exam + EMG (electromyography)
- MRI (exclude others)
- Genetic testing (suspected familial)
- El Escorial criteria

---

## 6. Treatment

### 6.1 Medications

- **Riluzole** (1995): NMDA antagonist, extends life ~3 months
- **Edaravone** (Radicava): antioxidant, slows ~30% functional decline
- **Tofersen** (2023): antisense oligonucleotide for SOD1 ALS
- **AMX0035**: FDA approved 2022 (later withdrawn 2024 due to phase 3 failure)

### 6.2 Supportive

- Non-invasive ventilation (BiPAP)
- Feeding tube (PEG) late stage
- Physical / speech therapy
- Wheelchair adaptation

### 6.3 BCI

- Communication assistive (eye tracking, EEG-based)
- Neuralink, Synchron, Blackrock all testing in ALS patients
- 2024: Synchron + Neuralink let ALS patients control tablet / mouse with thought

---

## 7. PyTorch — ALS Motor Neuron Loss Simulation

```python
import torch
import numpy as np

class MotorPoolSimulation:
    def __init__(self, n_motor_neurons=1000):
        self.alive = torch.ones(n_motor_neurons)
        self.muscle_strength = []
    
    def step(self, death_rate_per_year=0.05, dt_years=0.1):
        deaths = torch.rand(len(self.alive)) < death_rate_per_year * dt_years
        self.alive *= (~deaths).float()
        strength = self.alive.mean().item()
        self.muscle_strength.append(strength)
        return strength
```

---

## 8. BCI in ALS

- **Locked-in stage**: late ALS resembles locked-in syndrome (paralyzed but conscious)
- **EEG-BCI**: P300 speller, SSVEP (commercial)
- **Implant BCI**:
  - Stentrode (Synchron): trans-vascular implant, no craniotomy
  - Neuralink N1: high-channel, in trials
  - BrainGate: academic BCI, ALS patients used ~20 years
- Speech BCI: UCSF 2023 — 80 words / min from brainstem stroke patient

---

## 9. Famous Cases

- **Lou Gehrig** (1939 NYY): "Lou Gehrig's disease" namesake
- **Stephen Hawking** (1963 onset, 2018 death): physicist + extremely atypical long survival
- **Steve Gleason**: NFL → BCI advocate

---

## 10. Common Pitfalls

### 10.1 Progression Individual

Some patients slow course over years, others die in 1 year.

### 10.2 Doesn't Affect Cognition (Mostly)

But 5-15% have FTD (frontotemporal dementia) overlap.

### 10.3 EMG Diagnosis Hard

Needs expert; early stages not obvious.

### 10.4 No Cure

Only delay. Family psychological + financial burden large.

### 10.5 BCI Can't Restore Movement

Only substitutes communication / external device control.

---

## 11. Related Concepts

- **Same section**: [Stroke](Stroke.en.md), [Parkinson](Parkinson.en.md)
- **Systems**: [Motor System](../03_Systems_Neuroscience/Motor_System.en.md)
- **Neurotech**: [DBS](../07_Neurotech_Frontiers/DBS.en.md)

---

## References

1. **Brown, R. H. & Al-Chalabi, A.** "Amyotrophic Lateral Sclerosis." *NEJM*, 2017.
2. **DeJesus-Hernandez, M. et al.** "Expanded GGGGCC hexanucleotide repeat in noncoding region of C9ORF72." *Neuron*, 2011.
3. **Hochberg, L. R. et al.** "Reach and grasp by people with tetraplegia using a neurally controlled robotic arm (BrainGate)." *Nature*, 2012.
