# Cranial Nerves

> *12 pairs of cranial nerves emerge directly from brain/brainstem (vs spinal nerves). Mixed sensory/motor/parasympathetic. Classic mnemonics. High clinical localization value (each pair has characteristic signs). CN II (optic) + I (olfactory) are CNS extensions; vagus (X) is the parasympathetic workhorse. Cornerstone of neurological exam.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Brainstem](Brainstem.en.md), [Nervous System Overview](../00_Foundations/Nervous_System_Overview.en.md)

---

## 1. The Twelve

| # | Name | Type | Main function |
|---|---|---|---|
| I | Olfactory | Sensory | Smell (see [Olfactory_System](../03_Systems_Neuroscience/Olfactory_System.en.md)) |
| II | Optic | Sensory | Vision (CNS tract) |
| III | Oculomotor | Motor+PSNS | Most eye muscles, pupil constriction, eyelid |
| IV | Trochlear | Motor | Superior oblique (eye down-in) |
| V | Trigeminal | Mixed | Face sensation + mastication |
| VI | Abducens | Motor | Lateral rectus (eye abduction) |
| VII | Facial | Mixed | Facial expression + taste (ant 2/3) + tears/saliva |
| VIII | Vestibulocochlear | Sensory | Hearing + balance (see [Vestibular_System](../03_Systems_Neuroscience/Vestibular_System.en.md)) |
| IX | Glossopharyngeal | Mixed | Pharynx sensation + taste (post 1/3) + parotid |
| X | Vagus | Mixed | Visceral parasympathetic workhorse + pharynx/larynx |
| XI | Accessory | Motor | SCM/trapezius |
| XII | Hypoglossal | Motor | Tongue muscles |

---

## 2. Type Classification

- Pure sensory: I, II, VIII
- Pure motor: III (mainly), IV, VI, XI, XII
- Mixed: V, VII, IX, X
- Contain parasympathetic: III, VII, IX, X ("1973" rule)

---

## 3. Brain Emergence Site

- I, II: forebrain (telencephalon/diencephalon) — actually CNS tracts
- III, IV: midbrain
- V, VI, VII, VIII: pons
- IX, X, XI, XII: medulla
- → Cranial nerve nuclei localization = key to brainstem lesion localization (see [Brainstem](Brainstem.en.md))

---

## 4. PyTorch — Cranial Nerve Lookup (clinical localization)

```python
cranial_nerves = {
 1:("Olfactory","sensory","smell"), 2:("Optic","sensory","vision"),
 3:("Oculomotor","motor+PSNS","most eye muscles, pupil"),
 4:("Trochlear","motor","superior oblique"),
 5:("Trigeminal","mixed","face sensation, mastication"),
 6:("Abducens","motor","lateral rectus"),
 7:("Facial","mixed","facial expression, taste ant2/3"),
 8:("Vestibulocochlear","sensory","hearing+balance"),
 9:("Glossopharyngeal","mixed","pharynx, taste post1/3"),
 10:("Vagus","mixed","parasympathetic viscera"),
 11:("Accessory","motor","SCM/trapezius"),
 12:("Hypoglossal","motor","tongue"),
}
def localize(cn): return cranial_nerves.get(cn)
```

---

## 5. Classic Clinical Signs

- **III palsy**: eye "down and out", ptosis, dilated pupil (uncal herniation emergency!)
- **VII palsy**: Bell's palsy (peripheral: whole hemiface; central: lower face only — forehead bilaterally innervated)
- **V**: trigeminal neuralgia (severe facial pain)
- **X**: hoarseness, dysphagia, uvula deviation
- **Pupillary light reflex**: II (afferent) + III (efferent)
- **Corneal reflex**: V (afferent) + VII (efferent)

---

## 6. Reflex Arcs (Cranial Nerve)

- Light: CN II → midbrain → CN III (bilateral)
- Corneal: CN V1 → CN VII
- Gag: CN IX → CN X
- Carotid sinus (baroreceptor): CN IX → medulla → CN X
- → Reflex testing precisely localizes brainstem level

---

## 7. Vagus Nerve (X) — Parasympathetic Workhorse

- Innervates heart/lung/GI (slow rate, promote digestion)
- 80% afferent (visceral sensory → NTS, see [Interoception](../03_Systems_Neuroscience/Interoception.en.md))
- VNS (vagus stimulation) treats epilepsy/depression (see [Closed_Loop_Neuromodulation](../07_Neurotech_Frontiers/Closed_Loop_Neuromodulation.en.md))

---

## 8. Relation to AI / Engineering

- Cranial nerves = brain's dedicated I/O ports (sensor + actuator interfaces)
- Localization diagnosis = inferring fault node backward (hardware fault localization-like)
- Reflex arc = low-latency hardwired control loop

---

## 9. Development + Evolution

- Branchial arch-derived (V/VII/IX/X correspond to ancient gill arches)
- Cranial nerve nuclei arrangement retains segmental (rhombomere) imprint
- See [Evolution of Nervous Systems](../00_Foundations/Evolution_of_Nervous_Systems.en.md)

---

## 10. Common Pitfalls

### 10.1 All Are "Nerves"

I, II are actually CNS tracts (myelinated by oligodendrocytes, not Schwann).

### 10.2 Central = Peripheral VII Palsy

Central spares forehead (bilateral cortical innervation); peripheral whole hemiface.

### 10.3 Vagus Mainly Motor

80% is **afferent** (visceral sensory).

### 10.4 Cranial Nerves Only in Head

Vagus reaches abdomen (extensive viscera).

### 10.5 Pupil Reflex Unilateral

Normally bilateral (consensual); unilateral abnormality localizes II vs III.

---

## 11. Related Concepts

- **Same section**: [Brainstem](Brainstem.en.md), [Thalamus](Thalamus.en.md)
- **Foundation**: [Nervous System Overview](../00_Foundations/Nervous_System_Overview.en.md)
- **Systems**: [Vestibular_System](../03_Systems_Neuroscience/Vestibular_System.en.md), [Olfactory_System](../03_Systems_Neuroscience/Olfactory_System.en.md), [Autonomic_Nervous_System](../03_Systems_Neuroscience/Autonomic_Nervous_System.en.md)

---

## References

1. **Wilson-Pauwels, L. et al.** *Cranial Nerves: Function and Dysfunction*. 3rd ed., 2010.
2. **Blumenfeld, H.** *Neuroanatomy through Clinical Cases*. 2nd ed., 2010.
3. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
4. **Standring, S.** *Gray's Anatomy*. 42nd ed., 2020.
