# Brain Energy Metabolism

> *The brain is 2% of body weight but uses 20% of energy. Almost entirely on glucose + O₂ oxidation (no fat storage). Most ATP maintains ion gradients (Na/K pump). The astrocyte-neuron lactate shuttle is the supply model. Energy constraints shaped sparse coding + neural computational efficiency. The fMRI BOLD signal is itself based on energy demand.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Membrane Potential](../02_Cellular_Molecular/Membrane_Potential.en.md), basic metabolism

---

## 1. Numbers

| Item | Value |
|---|---|
| Brain mass | ~ 2% body weight |
| Energy use | ~ 20% resting metabolism |
| Power | ~ 20 W |
| Glucose | ~ 120 g/day |
| O₂ | ~ 20% whole-body consumption |
| ATP turnover | Very high, no storage |

---

## 2. Energy Destinations

- **~ 50-70%**: Na⁺/K⁺-ATPase (restoring ion gradients)
- Rest: synaptic transmission, neurotransmitter recycling, axonal transport, housekeeping
- Conclusion: maintaining excitability itself is extremely costly

---

## 3. Main Fuel — Glucose

```
Glucose → Glycolysis → Pyruvate
              ↓
        Mitochondria (TCA + OxPhos)
              ↓
        ~ 30-32 ATP / glucose
```

- Brain almost only uses glucose (ketone bodies in starvation)
- Cannot use fatty acids (blood-brain barrier)

---

## 4. Astrocyte-Neuron Lactate Shuttle (ANLS)

- Pellerin & Magistretti 1994 hypothesis
- Glutamate release → astrocyte uptake → triggers astrocyte glycolysis → lactate
- Lactate → neuron as fuel
- Still debated, but explains fMRI-energy coupling

---

## 5. Neurovascular Coupling

- Neural activity → local blood flow ↑ (delivers glucose + O₂)
- This is the physical basis of **fMRI BOLD**
- Delay ~ 1-2 sec (hemodynamic response)
- See [fMRI BOLD](../07_Neurotech_Frontiers/fMRI_BOLD.en.md)

---

## 6. Energy Constraint → Computational Principles

- Sparse coding saves energy (fewer spikes, less ATP)
- Attneave / Barlow: efficient coding hypothesis
- Spike itself is expensive → brain optimizes spike count
- Levy & Baxter 1996: energy-optimal → sparse firing rate ~ few Hz

---

## 7. PyTorch — Energy-Constrained Sparse Loss

```python
import torch

def energy_constrained_loss(activations, target, lambda_energy=0.01):
    """Task loss + metabolic (sparsity) cost — efficient coding."""
    task_loss = ((activations.sum(1) - target) ** 2).mean()
    # Each spike costs energy → L1 penalty on activation
    energy_cost = activations.abs().mean()
    return task_loss + lambda_energy * energy_cost
```

→ Analogy with brain energy budget: L1 sparsity = metabolic constraint.

---

## 8. Pathology

- **Stroke**: ischemia → energy collapse → excitotoxicity → neuron death
- **Hypoglycemia**: low blood sugar → loss of consciousness (no brain glucose storage)
- **Mitochondrial disease**: Leigh syndrome etc.
- **Alzheimer's**: glucose hypometabolism (FDG-PET early marker)
- **Aging**: declining metabolic efficiency

---

## 9. Measurement

- **FDG-PET**: radioactive glucose analog → metabolic map
- **fMRI BOLD**: indirect (blood oxygen)
- **MRS**: lactate / NAA / ATP
- **Calorimetry**: whole-brain energy use

---

## 10. Common Pitfalls

### 10.1 More Thinking Uses Much More Energy

The difference is small (~ 5%); brain baseline already very high (Raichle "dark energy").

### 10.2 Brain Uses Fat

Cannot; BBB blocks fatty acids; uses ketones in starvation.

### 10.3 ANLS Settled

Still debated; some neurons use glucose directly.

### 10.4 BOLD = Neural Activity

It's an energy/blood-flow proxy, delayed and nonlinear.

### 10.5 More Neurons = Smarter

Energy constraints limit brain size; efficiency > count.

---

## 11. Related Concepts

- **Same section**: [Neural Coding](Neural_Coding.en.md), [Research Methods](Research_Methods.en.md)
- **Cellular**: [Membrane Potential](../02_Cellular_Molecular/Membrane_Potential.en.md), [Glia](../02_Cellular_Molecular/Glia.en.md)
- **Frontiers**: [fMRI BOLD](../07_Neurotech_Frontiers/fMRI_BOLD.en.md)

---

## References

1. **Attwell, D. & Laughlin, S. B.** "An energy budget for signaling in the grey matter of the brain." *J Cereb Blood Flow Metab*, 2001.
2. **Pellerin, L. & Magistretti, P. J.** "Glutamate uptake into astrocytes stimulates aerobic glycolysis." *PNAS*, 1994.
3. **Raichle, M. E. & Mintun, M. A.** "Brain work and brain imaging." *Annu Rev Neurosci*, 2006.
4. **Magistretti, P. J. & Allaman, I.** "A cellular perspective on brain energy metabolism." *Neuron*, 2015.
