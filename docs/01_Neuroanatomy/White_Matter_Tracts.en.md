# White Matter Tracts

> *White matter = myelinated axon bundles (myelin lipid → white), ~ half of brain volume. Three types: association fibers (intra-hemisphere areas, e.g., arcuate fasciculus), commissural fibers (between hemispheres, e.g., corpus callosum), projection fibers (cortex↔subcortical, e.g., internal capsule/corticospinal). "Disconnection syndrome" (Geschwind) = tract damage disconnection. DTI reconstructs in vivo (see [DTI Tractography](../07_Neurotech_Frontiers/DTI_Tractography.en.md)).*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Cortex](Cortex.en.md), [Myelination](../02_Cellular_Molecular/Myelination.en.md)

---

## 1. Three Fiber Types

| Type | Connection | Example |
|---|---|---|
| **Association** | Intra-hemisphere cortical areas | Arcuate, superior/inferior longitudinal, uncinate, cingulum |
| **Commissural** | Between hemispheres | Corpus callosum (largest), anterior commissure |
| **Projection** | Cortex ↔ subcortical/spinal | Internal capsule, corticospinal, thalamic radiations |

---

## 2. Key Tracts

- **Arcuate fasciculus**: Broca↔Wernicke (language; damage → conduction aphasia)
- **Corpus callosum**: ~ 200 million axons; split-brain research (Sperry Nobel)
- **Internal capsule**: projection fiber "bottleneck" (small stroke → large deficit)
- **Corticospinal tract**: voluntary movement (pyramidal tract)
- **Uncinate fasciculus**: orbitofrontal↔anterior temporal (emotion-memory)

---

## 3. PyTorch — Disconnection Syndrome (graph)

```python
tracts = {
 "Broca": ["arcuate->Wernicke"],
 "Wernicke": ["arcuate->Broca"],
}
def disconnect(tract_cut):
    # Cutting arcuate: comprehension OK, production OK, but repetition fails
    if tract_cut == "arcuate":
        return "conduction aphasia: poor repetition, intact comprehension"
    return "depends on tract"
```

---

## 4. Projection Fibers — Internal Capsule

- Anterior limb/genu/posterior limb: different fibers (frontopontine, corticobulbar, corticospinal, thalamic radiation)
- Highly concentrated → small lacunar infarct → "pure motor stroke" (see [Cerebral_Vasculature](Cerebral_Vasculature.en.md))
- Explains why small lesions cause large deficits

---

## 5. Corpus Callosum + Split-Brain

- Sectioned to treat intractable epilepsy → "split-brain patients"
- Sperry/Gazzaniga: hemispheres can process independently (left language, right spatial)
- "Alien hand", left visual field naming deficit (info can't cross callosum)
- See [Cortex](Cortex.en.md) (lateralization)

---

## 6. Disconnection Syndromes (Geschwind)

- Symptoms from **tract damage** (connection severed) not cortical area damage itself
- Conduction aphasia (arcuate), alexia without agraphia (callosal splenium), apraxia
- Geschwind revived 1965 → modern connectomics precursor (see [Connectomics](../00_Foundations/Connectomics.en.md))

---

## 7. In Vivo Imaging

- **DTI tractography**: water diffusion anisotropy → reconstruct tracts (mm-scale, see [DTI Tractography](../07_Neurotech_Frontiers/DTI_Tractography.en.md))
- FA↓ = white matter integrity decline (non-specific)
- Presurgical planning (avoid arcuate/corticospinal)

---

## 8. Pathology

- **MS**: multifocal demyelination (see [Multiple_Sclerosis](../08_Neuro_Disorders/Multiple_Sclerosis.en.md))
- **Leukoaraiosis**: vascular (aging/hypertension)
- **TBI diffuse axonal injury (DAI)**: shear force severs tracts (see [Traumatic_Brain_Injury](../08_Neuro_Disorders/Traumatic_Brain_Injury.en.md))
- **Hereditary leukodystrophies**
- Psychiatric disorders as "connectopathy" (tract abnormality)

---

## 9. Relation to AI

- White matter = long-range "wiring" ↔ network connection topology / communication bandwidth
- Disconnection ↔ functional consequence of inter-module communication cut (ablation study analogy)
- Arcuate fasciculus ↔ encoder-decoder pathway
- See [Connectomics](../00_Foundations/Connectomics.en.md) (macro vs micro)

---

## 10. Common Pitfalls

### 10.1 White Matter Doesn't "Compute"

Wiring determines information routing + timing; damage causes disconnection symptoms.

### 10.2 Symptoms = Gray Matter Damage

Can be pure tract damage (conduction aphasia = arcuate, cortex intact).

### 10.3 DTI Tract = Real Axon Tract

mm-scale statistical inference, false positives (see [DTI Tractography](../07_Neurotech_Frontiers/DTI_Tractography.en.md)).

### 10.4 Small White Matter Lesion = Small Deficit

Internal capsule small lacune → large motor deficit (fiber concentration).

### 10.5 Split-Brain = Two Consciousnesses

Usually unified (right hemisphere limited language + behavioral integration); avoid over-interpretation.

---

## 11. Related Concepts

- **Same section**: [Cortex](Cortex.en.md), [Cerebral_Vasculature](Cerebral_Vasculature.en.md), [Spinal_Cord_Anatomy](Spinal_Cord_Anatomy.en.md)
- **Cellular**: [Myelination](../02_Cellular_Molecular/Myelination.en.md)
- **Foundation**: [Connectomics](../00_Foundations/Connectomics.en.md)
- **Frontiers**: [DTI Tractography](../07_Neurotech_Frontiers/DTI_Tractography.en.md)
- **Cognition**: [Language](../04_Cognitive_Neuroscience/Language.en.md)

---

## References

1. **Geschwind, N.** "Disconnexion syndromes in animals and man." *Brain*, 1965.
2. **Catani, M. & Thiebaut de Schotten, M.** *Atlas of Human Brain Connections*. Oxford, 2012.
3. **Gazzaniga, M. S.** "Cerebral specialization and interhemispheric communication." *Brain*, 2000.
4. **Standring, S.** *Gray's Anatomy*. 42nd ed., 2020.
