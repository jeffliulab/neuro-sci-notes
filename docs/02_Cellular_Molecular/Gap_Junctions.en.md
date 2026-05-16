# Gap Junctions & Electrical Synapses

> *Electrical synapses = gap junctions formed by connexins (vertebrates)/innexins (invertebrates), direct cytoplasmic electrical coupling. Features: very fast (no synaptic delay), bidirectional, synchronizing. This is where reticular theory was "partly right" (see [Neuron_Doctrine](../00_Foundations/Neuron_Doctrine.en.md)). Functions: fast reflexes, oscillation synchrony, development, astrocyte networks. Complementary to chemical synapses.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Synapse](Synapse.en.md), [Membrane_Potential](Membrane_Potential.en.md)

---

## 1. Structure

- **Connexon** (hemichannel) = 6 connexins → docks with another cell's to form gap junction channel
- Pore ~ 1-2 nm: passes ions + small molecules (< 1 kDa, e.g., cAMP, IP3)
- Many channels form a plaque
- Invertebrates: innexins (same function, non-homologous)

---

## 2. Electrical vs Chemical Synapse

| | Electrical | Chemical |
|---|---|---|
| Delay | Nearly 0 | ~ 0.5-2 ms |
| Direction | Mostly bidirectional | Unidirectional |
| Gain | Attenuation (no amplification) | Can amplify |
| Plasticity | Less (exists but weak) | Rich |
| Synchrony | Strong | Weak |
| Rectification | Some (rectifying) | — |

---

## 3. PyTorch — Electrical Coupling Synchrony

```python
import torch

def electrical_coupling(v1, v2, g_gap=0.3, steps=50):
    """Gap junction current drives two cells toward synchrony."""
    traj = []
    for _ in range(steps):
        i_gap = g_gap * (v2 - v1)        # bidirectional, ohmic
        v1 = v1 + i_gap + 0.05*torch.randn(1).item()
        v2 = v2 - i_gap + 0.05*torch.randn(1).item()
        traj.append((float(v1), float(v2)))
    return traj   # voltages converge → synchronization
```

---

## 4. Functions

- **Fast reflex / escape**: fish Mauthner cell (millisecond escape)
- **Oscillation synchrony**: interneuron networks (PV interconnected → gamma rhythm, see [Brain Rhythms](../00_Foundations/Brain_Rhythms.en.md))
- **Development**: early widespread electrical coupling (later replaced by chemical synapses)
- **Astrocyte syncytium**: astrocyte network propagates Ca²⁺ waves via gap junctions (see [Astrocyte_Function](Astrocyte_Function.en.md))

---

## 5. Rectification and Regulation

- Some gap junctions **rectify** (unidirectional preference)
- Regulation: voltage, pH, Ca²⁺, phosphorylation, connexin type
- "Electrical synapse plasticity" exists but weaker than chemical
- Gap junctions also in non-neural tissue (cardiac muscle synchrony)

---

## 6. Relation to Neuron Doctrine

- Electrical synapses = cytoplasmic continuity → reticular theory "partly right" in this sense
- But most transmission is chemical → Doctrine still dominant (see [Neuron_Doctrine](../00_Foundations/Neuron_Doctrine.en.md))
- Historical lesson: binary oppositions are often a continuum

---

## 7. Clinical

- **Connexin mutations**: CMTX (GJB1/Cx32 peripheral nerve), deafness (GJB2/Cx26 most common hereditary deafness)
- Arrhythmia (cardiac connexin)
- Gap junctions in ischemic "bystander effect" — damage spread
- Anticonvulsant: gap junction blocker research (synchrony pathology)

---

## 8. Relation to AI

- Bidirectional electrical coupling ↔ symmetric weights + diffusion/smoothing (mean-field coupling-like)
- Synchronization ↔ oscillator networks / coupled oscillators
- Complementary to chemical synapses ↔ fast linear pathway + slow nonlinear pathway
- Mostly synchrony not information transformation (different role)

---

## 9. Developmental Role

- Embryonic/early: widespread electrical coupling → coordinated activity waves (retinal waves etc.) → circuit refinement
- Mature: most regress, retained in specific networks (inferior olive, TRN, interneurons)
- See [Neurodevelopment](../00_Foundations/Neurodevelopment.en.md)

---

## 10. Common Pitfalls

### 10.1 Synapse = Chemical Synapse

Electrical synapses (gap junctions) exist and are functionally important (synchrony/fast reflex).

### 10.2 Electrical Synapse No Direction/No Regulation

Some rectify (unidirectional preference) + regulated by pH/Ca²⁺/phosphorylation.

### 10.3 Electrical Synapse No Plasticity

Exists but weaker than chemical (not "completely fixed").

### 10.4 Only for Fast Reflex

Also key to oscillation synchrony, development, astrocyte network.

### 10.5 Neuron Doctrine All Wrong (Because of Electrical Synapses)

Doctrine dominant (chemical synapses primary); electrical synapses are a supplement, not refutation.

---

## 11. Related Concepts

- **Same section**: [Synapse](Synapse.en.md), [Membrane_Potential](Membrane_Potential.en.md), [Astrocyte_Function](Astrocyte_Function.en.md)
- **Foundation**: [Neuron_Doctrine](../00_Foundations/Neuron_Doctrine.en.md), [Brain Rhythms](../00_Foundations/Brain_Rhythms.en.md), [Neural Circuits](../00_Foundations/Neural_Circuits.en.md)
- **Systems**: [Thalamocortical_System](../03_Systems_Neuroscience/Thalamocortical_System.en.md) (TRN)

---

## References

1. **Bennett, M. V. L. & Zukin, R. S.** "Electrical coupling and neuronal synchronization in the mammalian brain." *Neuron*, 2004.
2. **Connors, B. W. & Long, M. A.** "Electrical synapses in the mammalian brain." *Annu Rev Neurosci*, 2004.
3. **Söhl, G. et al.** "Expression and functions of neuronal gap junctions." *Nat Rev Neurosci*, 2005.
4. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
