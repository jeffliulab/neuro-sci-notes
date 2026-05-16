# Cerebral Vasculature

> *The brain is ~2% of body weight but consumes ~15-20% of cardiac output (no energy reserve → ischemia damages immediately, see [Brain Energy Metabolism](../00_Foundations/Brain_Energy_Metabolism.en.md)). Anterior (internal carotid) + posterior (vertebro-basilar) circulation anastomose via Circle of Willis. Stroke by vascular territory (ACA/MCA/PCA) has characteristic syndromes. Venous return via dural sinuses. Cerebral vascular anatomy = cornerstone of stroke localization.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Cortex](Cortex.en.md), [Stroke](../08_Neuro_Disorders/Stroke.en.md)

---

## 1. Dual Supply System

| System | Source | Territory |
|---|---|---|
| **Anterior circulation** | Internal carotid artery (ICA) | Anterior 2/3 of hemispheres, eye |
| **Posterior circulation** | Vertebral → basilar artery | Brainstem, cerebellum, occipital, posterior thalamus |

→ Circle of Willis (base) anastomotic connection, provides collateral redundancy.

---

## 2. Circle of Willis

```
   Anterior communicating (ACoA)
  /            \
ACA           ACA
 |             |
ICA ─ MCA   MCA ─ ICA
 |             |
PCoA(post comm) PCoA
  \            /
   PCA  ─basilar─ PCA
        vertebral ×2
```

Complete circle in < 50% of people (high variation) → collateral compensation differences → different stroke outcomes.

---

## 3. Three Major Cerebral Artery Territories

| Artery | Territory | Occlusion syndrome |
|---|---|---|
| **ACA** | Medial frontoparietal (leg motor/sensory) | Contralateral leg weakness > arm |
| **MCA** | Most of lateral surface (arm/face, language areas) | Contralateral arm/face palsy, aphasia (L), neglect (R) |
| **PCA** | Occipital + posterior thalamus | Contralateral hemianopia, visual agnosia |

Lacunar (perforators) → internal capsule/basal ganglia (pure motor/sensory stroke).

---

## 4. PyTorch — Vascular Territory → Deficit Localization

```python
vascular_syndromes = {
 "ACA": "contralateral leg weakness > arm",
 "MCA": "contralateral arm/face palsy, aphasia(L)/neglect(R)",
 "PCA": "contralateral hemianopia, visual agnosia",
 "lenticulostriate": "pure motor (internal capsule lacune)",
 "basilar": "locked-in / coma (brainstem)",
 "PICA": "Wallenberg (lateral medullary)",
}
def localize(occluded): return vascular_syndromes.get(occluded, "?")
```

---

## 5. Autoregulation

- Cerebral blood flow (CBF) constant over MAP ~ 60-150 mmHg (myogenic + metabolic + neural)
- Beyond range → perfusion-pressure-dependent (low → ischemia; high → edema/hemorrhage)
- Hypertension/post-stroke curve shifts right
- Neurovascular coupling = local activity ↑CBF (fMRI basis, see [fMRI BOLD](../07_Neurotech_Frontiers/fMRI_BOLD.en.md))

---

## 6. Venous Drainage

- Cortical veins → dural sinuses (superior sagittal etc.) → transverse → sigmoid → internal jugular
- Deep → great cerebral vein (Galen) → straight sinus
- **Bridging vein** rupture → subdural hematoma (see [Meninges_Ventricles](Meninges_Ventricles.en.md))
- **Cerebral venous sinus thrombosis (CVST)**: headache + seizure + focal (easily missed)

---

## 7. Watershed Zones

- Borders of adjacent arterial territories (ACA/MCA, MCA/PCA)
- Global hypoperfusion (cardiac arrest/severe hypotension) → watershed infarct ("man in a barrel")
- Terminal vessels → ischemia-vulnerable

---

## 8. Clinical

- **Ischemic stroke** (~85%): thrombosis/embolism → tPA/thrombectomy (time window, see [Stroke](../08_Neuro_Disorders/Stroke.en.md))
- **Hemorrhagic stroke** (~15%): hypertension (basal ganglia), aneurysm (SAH), AVM, amyloid
- **TIA**: transient reversible (stroke warning)
- **Moyamoya**, vasculitis, dissection (young stroke)
- Imaging: CTA/MRA/DSA (vessels), CT/DWI (parenchyma)

---

## 9. Relation to AI / Engineering

- Dual supply + Circle of Willis = redundancy + collateral (fault-tolerant network design analogy)
- Autoregulation = closed-loop pressure regulation (see eng-notes control)
- Vascular territory localization = fault node inference (topological diagnosis-like)
- Neurovascular coupling → physical basis of fMRI signal

---

## 10. Common Pitfalls

### 10.1 Circle of Willis Complete in Everyone

< 50% complete; variation determines collateral compensation + stroke outcome.

### 10.2 Brain Has Energy Reserve

None; blood flow stop for minutes → irreversible damage (ischemic core).

### 10.3 Stroke Symptoms Random

Highly stereotyped by vascular territory (ACA/MCA/PCA syndromes).

### 10.4 Venous Problems Rare/Unimportant

CVST easily missed but potentially fatal/disabling.

### 10.5 Autoregulation Unlimited

Only ~60-150 mmHg; beyond range perfusion-pressure-dependent (hypertension shifts right).

---

## 11. Related Concepts

- **Same section**: [Cortex](Cortex.en.md), [Brainstem](Brainstem.en.md), [Meninges_Ventricles](Meninges_Ventricles.en.md)
- **Foundation**: [Brain Energy Metabolism](../00_Foundations/Brain_Energy_Metabolism.en.md), [Blood Brain Barrier](../00_Foundations/Blood_Brain_Barrier.en.md)
- **Disease**: [Stroke](../08_Neuro_Disorders/Stroke.en.md)
- **Frontiers**: [fMRI BOLD](../07_Neurotech_Frontiers/fMRI_BOLD.en.md)

---

## References

1. **Blumenfeld, H.** *Neuroanatomy through Clinical Cases*. 2nd ed., 2010.
2. **Caplan, L. R.** *Caplan's Stroke: A Clinical Approach*. 5th ed., 2016.
3. **Cipolla, M. J.** *The Cerebral Circulation*. Morgan & Claypool, 2009.
4. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
