# Myelination

> *Myelin is a lipid insulation layer wrapped around axons by oligodendrocytes (CNS) / Schwann cells (PNS) → saltatory conduction, conduction velocity ↑ 50× + energy ↓. Ranvier nodes enriched in Nav. Activity-dependent myelination (adaptive myelination) is a new learning mechanism. Demyelination → MS. Key to [Action_Potential](Action_Potential.en.md) speed.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Action_Potential](Action_Potential.en.md), [Glia](Glia.en.md)

---

## 1. Who Forms Myelin

| | CNS | PNS |
|---|---|---|
| Cell | Oligodendrocyte | Schwann cell |
| Ratio | 1 cell → many axons (~50) | 1 cell → 1 segment |
| Regeneration | Poor (limited OPC) | Better |
| Disease | MS, leukodystrophy | GBS, CMT |

---

## 2. Saltatory Conduction

- Myelin segment insulated + low capacitance → AP not regenerated there
- **Ranvier node** (unmyelinated, ~1 μm): high Nav density → AP "jumps" node to node, regenerating
- Velocity ∝ diameter (myelinated, linear) vs ∝ √diameter (unmyelinated)
- ~ 50-120 m/s (myelinated) vs 0.5-2 m/s (unmyelinated)

---

## 3. Energy + Space Advantages

- AP regenerated only at nodes → Na⁺/K⁺ pump energy greatly reduced
- At same velocity, myelinated axon far thinner than unmyelinated → space economy
- Evolution: solution to brain wiring + energy constraint (see [Brain Energy Metabolism](../00_Foundations/Brain_Energy_Metabolism.en.md))

---

## 4. PyTorch — Myelinated vs Unmyelinated Velocity

```python
import torch

def conduction_velocity(diameter, myelinated=True):
    """Myelinated: v ∝ d (linear). Unmyelinated: v ∝ sqrt(d)."""
    if myelinated:
        return 6.0 * diameter            # ~6 m/s per μm (Hursh)
    return 1.0 * torch.sqrt(torch.tensor(float(diameter)))

# Same velocity: myelinated axon far thinner -> wiring economy
```

---

## 5. Developmental Timeline

- Continues postnatally to ~ 25-30 years
- **Back→front**: sensory/motor early, **PFC last** → explains adolescent immature decisions (see [Cognitive_Development](../04_Cognitive_Neuroscience/Cognitive_Development.en.md))
- Critical periods + experience influence

---

## 6. Adaptive Myelination (Activity-Dependent)

- Neural activity → OPC proliferation/differentiation → experience-dependent myelination
- Learning (piano/memory tasks) → white matter changes (DTI-measurable, see [DTI Tractography](../07_Neurotech_Frontiers/DTI_Tractography.en.md))
- Modulates conduction **timing** (synchronous arrival → oscillation/binding)
- Myelin = new plasticity dimension (not just "insulation")

---

## 7. Nodal Molecular Structure

- **Node**: Nav1.6 clustering + ankyrin-G
- **Paranode**: axo-glial junction (Caspr/contactin)
- **Juxtaparanode**: Kv1 potassium channels
- Structural disruption (autoimmune/genetic) → conduction failure

---

## 8. Pathology

- **MS**: CNS demyelination (autoimmune, see [Multiple_Sclerosis](../08_Neuro_Disorders/Multiple_Sclerosis.en.md))
- **Guillain-Barré**: PNS acute demyelination (autoimmune)
- **CMT (Charcot-Marie-Tooth)**: hereditary peripheral nerve myelin disease
- **Leukodystrophy**: genetic myelin metabolic disease
- **Prematurity**: immature myelination → cerebral palsy risk
- Remyelination = new MS treatment direction

---

## 9. Relation to AI / Engineering

- Myelin = insulation + repeater ↔ transmission line + repeater (engineering analogy)
- Tunable conduction delay (adaptive myelin) ↔ learnable delays / temporal coding
- Energy/space optimal ↔ hardware wiring constraint
- See [Action_Potential](Action_Potential.en.md), [Neural Coding](../00_Foundations/Neural_Coding.en.md) (timing)

---

## 10. Common Pitfalls

### 10.1 Myelin Is Just "Insulating Tape"

It's a living cell + metabolic support + activity-dependent plasticity (adaptive myelination).

### 10.2 Saltation = AP Jumps Over Space

AP **regenerates** at nodes; internode is passive fast electrotonic spread (not literally "skipping").

### 10.3 Unmyelinated = Primitive Useless

Unmyelinated C fibers (pain/autonomic) functionally essential; not inferior.

### 10.4 Myelination Done in Childhood

PFC continues to ~25-30 years.

### 10.5 Myelin Doesn't Affect Computation

Modulates timing → affects synchrony/oscillation/binding (computationally relevant).

---

## 11. Related Concepts

- **Same section**: [Action_Potential](Action_Potential.en.md), [Glia](Glia.en.md), [Ion_Channels](Ion_Channels.en.md)
- **Foundation**: [Brain Energy Metabolism](../00_Foundations/Brain_Energy_Metabolism.en.md), [Neurodevelopment](../00_Foundations/Neurodevelopment.en.md)
- **Disease**: [Multiple_Sclerosis](../08_Neuro_Disorders/Multiple_Sclerosis.en.md)
- **Frontiers**: [DTI Tractography](../07_Neurotech_Frontiers/DTI_Tractography.en.md)

---

## References

1. **Hartline, D. K. & Colman, D. R.** "Rapid conduction and the evolution of giant axons and myelinated fibers." *Curr Biol*, 2007.
2. **Nave, K.-A. & Werner, H. B.** "Myelination of the nervous system: mechanisms and functions." *Annu Rev Cell Dev Biol*, 2014.
3. **Fields, R. D.** "A new mechanism of nervous system plasticity: activity-dependent myelination." *Nat Rev Neurosci*, 2015.
4. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
