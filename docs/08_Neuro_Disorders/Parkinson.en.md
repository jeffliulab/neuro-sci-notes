# Parkinson's Disease (PD)

> *Parkinson is the second most common neurodegenerative disease. Dopamine neurons in Substantia Nigra pars compacta (SNc) degenerate → BG dysregulation → motor symptoms (resting tremor, bradykinesia, rigidity). L-DOPA in the 1960s was revolutionary but still not a cure.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.en.md), [Neurotransmitters (DA)](../02_Cellular_Molecular/Neurotransmitters.en.md)

---

## 1. Clinical Features

Classic 4 symptoms (TRAP):
- **Tremor**: resting (4-6 Hz)
- **Rigidity**: muscle stiffness ("cogwheel")
- **Akinesia / bradykinesia**: difficulty initiating / slow movement
- **Postural instability**: balance issues

Non-motor: depression, sleep, autonomic, cognitive decline late.

---

## 2. Pathology

### 2.1 SNc DA Neuron Loss

- 80%+ SNc DA neurons degenerate before symptoms
- Substantia nigra loses black color (neuromelanin)

### 2.2 Lewy Bodies

- α-synuclein protein aggregates (inclusion bodies)
- In surviving neurons
- Also spread to cortex (Lewy Body Dementia)

### 2.3 Mitochondrial Dysfunction

- DA neurons prone to oxidative stress
- Linked to PINK1 / Parkin genes (mitochondrial QC)

---

## 3. Genetic vs Sporadic

- 90% sporadic
- 10% genetic (LRRK2, SNCA, PINK1, Parkin, GBA)
- LRRK2 G2019S: common AD mutation (5-7% sporadic cases)

---

## 4. Pathway Dysregulation

```
SNc DA reduction
   ↓
Striatum D1/D2 imbalance
   ↓
Direct pathway weak (less Go)
Indirect pathway strong (more No-Go)
   ↓
GPi over-active → thalamus over-inhibited → cortex motor initiation difficult
```

---

## 5. Diagnosis

- Clinical: UK Brain Bank criteria
- DAT-SPECT (DA transporter imaging): reduced striatum DA
- MRI: rule out other causes
- L-DOPA response: positive supports PD

---

## 6. Treatment

### 6.1 Medications

- **L-DOPA + Carbidopa**: DA precursor (Levodopa); gold standard
- **DA agonists** (Pramipexole, Ropinirole)
- **MAO-B inhibitors** (Selegiline, Rasagiline): delay DA metabolism
- **COMT inhibitors**: extend L-DOPA half-life
- **Anticholinergics**: older, inhibit ACh (tremor)

### 6.2 Surgery

- **DBS (Deep Brain Stimulation) on STN / GPi**:
  - Developed 1990s, popularized 2000s
  - Electrodes implanted in STN → high-freq stim → improves motor
  - Not curative but dramatic quality-of-life improvement

### 6.3 Experimental

- **Focused ultrasound thalamotomy** (non-invasive DBS)
- **Stem cell therapy** (iPSC → DA neurons → transplant)
- **Anti-α-synuclein antibody**
- **GDNF / neurotrophic factor** (many trial failures)

---

## 7. AI / Tech Connections

- Wearable sensors (tremor monitoring)
- DL diagnosis from gait video / voice
- DBS algorithm optimization (RL)
- Drug discovery

---

## 8. PyTorch — DBS Concept

```python
import torch

class DBSController:
    """Closed-loop DBS controller."""
    def __init__(self, target_freq=130):
        self.target_freq = target_freq
        self.beta_threshold = 0.5
    
    def control(self, beta_power):
        if beta_power > self.beta_threshold:
            return self.target_freq
        else:
            return 0
```

---

## 9. Numbers

- 10M patients globally (2025)
- Average onset 60
- Early-onset: 10% < 50
- Male > female (1.5×)

---

## 10. Common Pitfalls

### 10.1 80% Loss Before Symptoms

Early diagnosis hard.

### 10.2 L-DOPA Not Curative

Only replenishes DA, doesn't stop progression.

### 10.3 L-DOPA Dyskinesias

Long-term use → involuntary movements (motor complications).

### 10.4 Non-motor Symptoms Severe

Depression / dementia / autonomic dysfunction — dominant later.

### 10.5 vs Lewy Body Dementia

Spectrum overlap; not entirely separate disease.

---

## 11. Related Concepts

- **Same section**: [Alzheimer](Alzheimer.en.md), [Depression](Depression.en.md)
- **Anatomy**: [Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.en.md)
- **NT**: [DA](../02_Cellular_Molecular/Neurotransmitters.en.md)

---

## References

1. **Parkinson, J.** *An Essay on the Shaking Palsy*. 1817.
2. **Carlsson, A.** "The occurrence, distribution and physiological role of catecholamines in the nervous system." *Pharmacol Rev*, 1959.
3. **Olanow, C. W. et al.** "The scientific and clinical basis for the treatment of Parkinson disease." *Neurology*, 2009.
4. **Lozano, A. M. et al.** "Deep brain stimulation: current challenges and future directions." *Nat Rev Neurol*, 2019.
