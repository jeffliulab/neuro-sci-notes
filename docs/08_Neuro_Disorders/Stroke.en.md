# Stroke

> *Stroke is an acute cerebrovascular event — ischemic (87%) or hemorrhagic (13%). Globally **2nd cause of death + 1st cause of disability**. "Time is brain" — losing ~ 2M neurons per minute. tPA + thrombectomy are revolutionary treatments.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Cortex](../01_Neuroanatomy/Cortex.en.md), [Brainstem](../01_Neuroanatomy/Brainstem.en.md)

---

## 1. Types

### 1.1 Ischemic Stroke (87%)

- **Thrombotic**: local atherosclerotic plaque rupture → thrombus
- **Embolic**: heart (afib) or large artery source → distal blockage
- **Lacunar**: small perforating arteries (hypertension related)

### 1.2 Hemorrhagic (13%)

- **Intracerebral hemorrhage** (ICH): hypertension / amyloid angiopathy
- **Subarachnoid hemorrhage** (SAH): aneurysm rupture

### 1.3 TIA (Transient Ischemic Attack)

- "Mini-stroke", symptoms < 24 hr resolve
- But high risk of subsequent full stroke

---

## 2. Pathophysiology

```
Vessel occlusion (or rupture)
  ↓
Local brain ischemia (no O₂ + glucose)
  ↓
ATP depletion → Na+/K+ pump fails
  ↓
Glutamate massive release (excitotoxicity)
  ↓
Ca²⁺ inflow → apoptosis + necrosis
  ↓
Neurons die within minutes (core)
Penumbra (semi-saved surrounding) can be rescued hours
```

→ "Time is brain".

---

## 3. Risk Factors

- Hypertension (#1)
- Atrial fibrillation
- Diabetes
- Smoking
- High cholesterol
- Sedentary
- Family history
- Age > 65

---

## 4. Symptoms (FAST)

- **F**ace drooping
- **A**rm weakness
- **S**peech difficulty
- **T**ime to call 911

Plus visual loss, dizziness, severe headache (SAH).

---

## 5. Diagnosis

- **CT head** (first, exclude hemorrhage)
- **CT angiography** (find LVO, large vessel occlusion)
- **MRI DWI**: early ischemic detection
- **NIHSS** score (severity)

---

## 6. Acute Treatment

### 6.1 Ischemic Stroke

- **tPA (alteplase) IV**: < 4.5 hr from onset → thrombolysis
- **Thrombectomy** (mechanical clot removal): < 24 hr for LVO, **revolutionary** (2015+)
- **Aspirin** antiplatelet

### 6.2 Hemorrhagic

- BP control
- Anticoagulation reversal
- Neurosurgery if needed

---

## 7. Secondary Prevention

- **Antiplatelet**: aspirin, clopidogrel
- **Anticoagulant** (if afib): warfarin, DOAC
- **Statin** (LDL)
- **BP control** < 130/80
- **Lifestyle**: quit smoking, exercise

---

## 8. Rehabilitation

- Acute (week 1-2): hospital
- Subacute: rehab facility (PT, OT, speech)
- Chronic: home / outpatient
- Recovery greatest in first 3 months

### Cortical Reorganization

- Healthy hemisphere partially compensates
- Plasticity-based therapy (CIMT, mirror therapy)
- TMS / tDCS experimental

---

## 9. BCI for Stroke

- Motor BCI: decode imagined movement → control exoskeleton / FES
- Plasticity boost (closed-loop training)
- Some chronic patients recover fine motor

---

## 10. AI Applications

- **CT/MRI imaging AI**: early detection (Aidoc, Viz.ai)
- **Stroke scale auto-scoring**
- **Outcome prediction**
- **Rehabilitation robot** (Lokomat, Hocoma)
- **VR therapy**

---

## 11. PyTorch — Stroke Imaging Classification

```python
import torch
import torch.nn as nn

class StrokeCTClassifier(nn.Module):
    def __init__(self):
        super().__init__()
        self.backbone = nn.Sequential(
            nn.Conv3d(1, 64, 7, stride=2), nn.BatchNorm3d(64), nn.ReLU(),
            nn.MaxPool3d(3, stride=2),
            nn.AdaptiveAvgPool3d(1),
        )
        self.fc = nn.Linear(64, 3)
    
    def forward(self, ct_volume):
        feat = self.backbone(ct_volume).flatten(1)
        return self.fc(feat)
```

---

## 12. Common Pitfalls

### 12.1 FAST Incomplete

Posterior circulation stroke (dizziness, balance) may not fit FAST.

### 12.2 Missing tPA Window

> 4.5 hr → can't IV tPA; thrombectomy still possible to 24 hr.

### 12.3 Hemorrhage Can't tPA

CT excluding hemorrhage is critical.

### 12.4 Mimics

Hypoglycemia, migraine, seizure can resemble stroke.

### 12.5 BP Target Balance

Too high → bleeding risk; too low → reduced penumbra perfusion.

---

## 13. Related Concepts

- **Same section**: [Alzheimer](Alzheimer.en.md), [Parkinson](Parkinson.en.md)
- **Anatomy**: [Cortex](../01_Neuroanatomy/Cortex.en.md), [Brainstem](../01_Neuroanatomy/Brainstem.en.md)

---

## References

1. **Powers, W. J. et al.** "2018 Guidelines for the Early Management of Patients With Acute Ischemic Stroke." *Stroke*, 2018.
2. **Hacke, W. et al.** "Thrombolysis with alteplase 3 to 4.5 hours after acute ischemic stroke." *NEJM*, 2008.
3. **Goyal, M. et al.** "Endovascular thrombectomy after large-vessel ischaemic stroke." *Lancet*, 2016.
4. **Murphy, T. H. & Corbett, D.** "Plasticity during stroke recovery: from synapse to behaviour." *Nat Rev Neurosci*, 2009.
