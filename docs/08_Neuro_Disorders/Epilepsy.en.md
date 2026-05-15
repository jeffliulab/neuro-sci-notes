# Epilepsy

> *Epilepsy is a chronic brain disease affecting ~1% of population. Neurons hyper-synchronously fire → seizures. 70% controlled by medication; 30% drug-resistant. DBS, RNS, surgical resection are emerging treatments.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Ion Channels](../02_Cellular_Molecular/Ion_Channels.en.md), [Cortex](../01_Neuroanatomy/Cortex.en.md)

---

## 1. Clinical

ILAE classification:
- **Focal seizure**: starts in local brain area
  - With awareness (simple partial)
  - Without awareness (complex partial)
- **Generalized seizure**: whole brain simultaneously
  - Tonic-clonic (grand mal)
  - Absence (petit mal)
  - Myoclonic
  - Atonic (drop attacks)

Epilepsy diagnosis: **2+ unprovoked seizures** (or 1 + high recurrence risk).

---

## 2. Mechanism

- **Hyper-excitation**: excess Glu / NMDA / Nav abnormality
- **Hypo-inhibition**: insufficient GABA / GABAR mutation
- Neuron group synchronous firing → seizure spreads
- "Tipping point" model

---

## 3. Causes

- **Genetic**: Dravet (Nav1.1), Lennox-Gastaut
- **Structural**: stroke, tumor, malformation
- **Infectious**: encephalitis, neurocysticercosis
- **Metabolic**: hypoglycemia, hyponatremia
- **Autoimmune**: NMDAR encephalitis
- **Unknown** (60% clinical cause unknown)

---

## 4. Diagnosis

- **EEG**: look for spike-wave discharges
- **MRI**: structural abnormalities
- **Long-term video-EEG**: 24-72 hr monitoring
- **PET**: ictal vs interictal

---

## 5. Treatment

### 5.1 Antiepileptic Drugs (AED)

- **Sodium channel blockers**: Carbamazepine, Phenytoin, Lamotrigine
- **GABA enhancers**: Valproate, Benzodiazepines
- **AMPA blockers**: Perampanel
- **Multi-mechanism**: Topiramate, Levetiracetam
- 70% of patients controlled

### 5.2 Surgery

- **Resection**: remove epileptogenic focus (mesial temporal lobe common)
- **Corpus callosotomy**: cut corpus callosum (refractory drop attacks)
- **Hemispherectomy**: extreme cases

### 5.3 Neuromodulation

- **VNS (Vagus Nerve Stimulator)**: neck implant, scheduled stimulation
- **RNS (Responsive Neuro-Stimulator)**: NeuroPace, detects seizure → activates
- **DBS**: anterior thalamic nucleus stimulation

### 5.4 Diet

- **Ketogenic diet**: high fat, low carb; effective for refractory pediatric epilepsy

---

## 6. Status Epilepticus

- Seizure > 5 min or recurrent without recovery
- High risk of neural damage + death
- Emergency: Lorazepam IV → Phenytoin → general anesthesia

---

## 7. PyTorch — Seizure Detection from EEG

```python
import torch
import torch.nn as nn

class SeizureDetector(nn.Module):
    def __init__(self, n_channels=4, n_classes=2):
        super().__init__()
        self.cnn = nn.Sequential(
            nn.Conv1d(n_channels, 32, 5), nn.BatchNorm1d(32), nn.ReLU(),
            nn.MaxPool1d(2),
            nn.Conv1d(32, 64, 5), nn.BatchNorm1d(64), nn.ReLU(),
            nn.MaxPool1d(2),
            nn.AdaptiveAvgPool1d(1),
        )
        self.fc = nn.Linear(64, n_classes)
    
    def forward(self, eeg):
        feat = self.cnn(eeg).squeeze(-1)
        return self.fc(feat)
```

---

## 8. AI Applications

- DL EEG → seizure detection / prediction (30 sec ahead)
- Wearable (Empatica Embrace) — auto detect + alert
- Personalized AED dosing
- Brain network analysis to find epileptogenic zone

---

## 9. Common Pitfalls

### 9.1 Seizure ≠ Epilepsy

Single seizure isn't necessarily epilepsy.

### 9.2 Normal EEG Doesn't Exclude

20% of patients have normal interictal EEG. Need long-term recording.

### 9.3 AED Side Effects

Many AEDs affect cognition / mood / metabolism.

### 9.4 Pregnancy

Valproate teratogenic → not for childbearing age.

### 9.5 SUDEP (Sudden Unexpected Death)

~ 1/1000 patient-year — cardiopulmonary arrest in sleep.

---

## 10. Related Concepts

- **Same section**: [Alzheimer](Alzheimer.en.md), [Parkinson](Parkinson.en.md), [Depression](Depression.en.md)
- **Cellular**: [Ion Channels](../02_Cellular_Molecular/Ion_Channels.en.md), [Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.en.md)

---

## References

1. **Fisher, R. S. et al.** "Operational classification of seizure types (ILAE)." *Epilepsia*, 2017.
2. **Engel, J.** *Seizures and Epilepsy*. 2nd ed., 2013.
3. **Morrell, M. J.** "Responsive cortical stimulation for the treatment of medically intractable partial epilepsy." *Neurology*, 2011.
