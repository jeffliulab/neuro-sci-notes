# Axonal Transport

> *Neurons are polarized: proteins synthesized in the soma, axons can be up to 1 m → active transport needed. Motor proteins: kinesin (anterograde, + end → terminal), dynein (retrograde → soma), along microtubule tracks. Fast (vesicles ~400 mm/d) vs slow (cytoskeleton ~1 mm/d). Transport failure → ALS/AD/HD ("transportopathy").*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Neuron](Neuron.en.md), [Synaptic_Vesicle_Cycle](Synaptic_Vesicle_Cycle.en.md)

---

## 1. Why Needed

- Protein synthesis mainly in soma (ribosomes); axons reach 1 m (motor neurons)
- Terminal synapse needs: vesicle proteins, ion channels, mitochondria, mRNA
- Passive diffusion too slow (1 m takes decades) → **active motor transport**

---

## 2. Motors + Tracks

| Motor | Direction | Cargo |
|---|---|---|
| **Kinesin** | Anterograde (+ end → terminal) | Vesicles, mitochondria, mRNA |
| **Dynein** | Retrograde (→ soma) | Signaling endosomes (NGF), autophagosomes, degradation |
| Track | **Microtubule** (polarity: + end toward axon terminal) | — |
| Myosin | Short-range, actin (synapse/dendrite) | Local delivery |

---

## 3. Fast vs Slow Transport

| | Fast | Slow |
|---|---|---|
| Speed | ~ 50-400 mm/day | ~ 0.1-3 mm/day |
| Cargo | Membranous organelles, vesicles | Cytoskeleton (tubulin/NF), soluble enzymes |
| Mechanism | Continuous motor | "Stop-and-go" (intermittent fast movement) |

---

## 4. PyTorch — Bidirectional Transport + Net Flux

```python
import torch

def axonal_transport(T=1000, p_antero=0.6, step=1.0):
    """Stochastic bidirectional motor stepping; net anterograde flux."""
    pos = 0.0
    traj = []
    for _ in range(T):
        if torch.rand(1) < p_antero:
            pos += step          # kinesin (anterograde)
        else:
            pos -= step          # dynein (retrograde)
        pos = max(pos, 0.0)
        traj.append(pos)
    return traj   # biased random walk → net delivery to terminal
```

---

## 5. Retrograde Signaling

- Terminal takes up NGF/BDNF → "signaling endosome" → dynein retrograde → nucleus (survival/transcription)
- Injury signals retrograde → regeneration response
- Viruses (rabies, HSV, polio) hijack retrograde transport into CNS
- See [Neurotrophins](Neurotrophins.en.md)

---

## 6. Mitochondrial Transport

- Mitochondria move bidirectionally along axon → positioned by energy demand (synapse/Ranvier node)
- Milton/Miro adaptors + Ca²⁺-regulated docking ("anchored" at high-activity sites)
- Dysfunction → energy crisis (see [Brain Energy Metabolism](../00_Foundations/Brain_Energy_Metabolism.en.md))

---

## 7. Local Translation

- mRNA + ribosomes transported to dendrites/axon → local translation (synapse-specific)
- Supports synapse-specific plasticity (late LTP needs local protein synthesis)
- Related to "synaptic tag and capture" mechanism (see [LTP_LTD](LTP_LTD.en.md))

---

## 8. Pathology ("Transportopathy")

- **ALS**: axonal transport defect early (SOD1/TDP-43/dynein mutations, see [ALS](../08_Neuro_Disorders/ALS.en.md))
- **AD**: tau hyperphosphorylation → microtubule instability → transport failure → axonal degeneration
- **HD**: mutant huntingtin disrupts dynein/kinesin
- **CMT2**: kinesin/dynein/MFN2 mutations (peripheral nerve)
- Distal axon degenerates first ("dying-back") — long axons most vulnerable

---

## 9. Relation to AI / Engineering

- Bidirectional motors + microtubule ↔ active logistics / scheduling (resource allocation to distal)
- Retrograde signaling ↔ long-range credit propagation (analogy; not backprop)
- Local translation ↔ edge computing / local update
- Energy positioning ↔ resource-constrained optimization (see [Brain Energy Metabolism](../00_Foundations/Brain_Energy_Metabolism.en.md))

---

## 10. Common Pitfalls

### 10.1 Diffusion to Terminal Suffices

1 m diffusion takes decades; active motor transport essential.

### 10.2 Kinesin/Dynein Purely Unidirectional

Cargo often bidirectional "tug-of-war" (net flux biased); regulable.

### 10.3 Slow Transport Unimportant

Cytoskeleton/enzyme slow transport maintains axon structure; critical.

### 10.4 Transport Unrelated to Disease

ALS/AD/HD early transport defects ("dying-back").

### 10.5 Microtubule Is Passive Track

Dynamic (growth/shrink) + tau-regulated + polarity determines direction.

---

## 11. Related Concepts

- **Same section**: [Neuron](Neuron.en.md), [Synaptic_Vesicle_Cycle](Synaptic_Vesicle_Cycle.en.md), [Neurotrophins](Neurotrophins.en.md), [LTP_LTD](LTP_LTD.en.md)
- **Foundation**: [Brain Energy Metabolism](../00_Foundations/Brain_Energy_Metabolism.en.md)
- **Disease**: [ALS](../08_Neuro_Disorders/ALS.en.md), [Alzheimer](../08_Neuro_Disorders/Alzheimer.en.md)

---

## References

1. **Hirokawa, N. et al.** "Kinesin superfamily motor proteins and intracellular transport." *Nat Rev Mol Cell Biol*, 2009.
2. **Maday, S. et al.** "Axonal transport: cargo-specific mechanisms of motility and regulation." *Neuron*, 2014.
3. **Millecamps, S. & Julien, J.-P.** "Axonal transport deficits and neurodegenerative diseases." *Nat Rev Neurosci*, 2013.
4. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
