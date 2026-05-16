# Neuron Types & Cell Taxonomy

> *Neurons are not unitary: multidimensional classification by morphology (pyramidal/stellate/Purkinje) × neurochemistry (glutamatergic/GABAergic) × electrophysiology (regular/fast-spiking) × transcriptome (single-cell RNA-seq). Excitatory (~80%) vs inhibitory interneurons (~20%, PV/SST/VIP three classes). BICCN cell census → thousands of types. Basis for understanding circuits + AI "neuron ≠ perceptron".*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Neuron](Neuron.en.md), [Dendrites](Dendrites.en.md)

---

## 1. Multidimensional Classification

| Dimension | Example |
|---|---|
| **Morphology** | Pyramidal, stellate, Purkinje, basket, granule |
| **Neurochemistry** | Glutamatergic (excite), GABAergic (inhibit), cholinergic, monoaminergic |
| **Electrophysiology** | Regular-spiking, fast-spiking, bursting, accommodating |
| **Projection** | Projection neurons (long-range) vs local interneurons |
| **Transcriptome** | scRNA-seq marker gene clusters (BICCN) |

→ Modern: multimodal (Patch-seq) integrated typing.

---

## 2. Excitatory vs Inhibitory

- **Excitatory (~80%)**: glutamatergic, mostly pyramidal (cortex) / granule
- **Inhibitory (~20%)**: GABAergic interneurons, **extremely diverse**
- E/I balance is core to circuit stability (imbalance → epilepsy/autism/SCZ)

---

## 3. Three Major Interneuron Classes (Cortex)

| Type | Marker | Target + function |
|---|---|---|
| **PV** (parvalbumin) | basket/chandelier | Soma/AIS; fast inhibition, gamma oscillation |
| **SST** (somatostatin) | Martinotti | Dendrites; modulate input integration |
| **VIP** | — | Inhibits SST → **disinhibition** (gating, see [Neural Circuits](../00_Foundations/Neural_Circuits.en.md)) |

→ "Canonical" inhibitory microcircuit.

---

## 4. Iconic Projection Neurons

- **Cortical pyramidal**: L5 (subcortical projection), L2/3 (intracortical) — see [Cortex](../01_Neuroanatomy/Cortex.en.md)
- **Purkinje**: cerebellum's sole output, GABA, huge dendrites (see [Cerebellum](../01_Neuroanatomy/Cerebellum.en.md))
- **Hippocampal CA1/CA3 pyramidal, DG granule**
- **Striatal MSN** (GABA, see [Basal_Ganglia](../01_Neuroanatomy/Basal_Ganglia.en.md))
- **Dopamine (SNc/VTA), 5-HT (raphe), motor neurons (α/γ)**

---

## 5. PyTorch — E-phys "Fingerprint" Classification

```python
import torch

def classify_firing_type(spike_train, dt=1.0):
    """Crude e-phys typing from ISI statistics (RS vs FS vs bursting)."""
    isi = torch.diff(torch.nonzero(spike_train).squeeze().float()) * dt
    cv = isi.std() / (isi.mean() + 1e-6)        # ISI coefficient of variation
    rate = spike_train.sum() / (len(spike_train) * dt) * 1000
    if rate > 50 and cv < 0.4:  return "fast-spiking (PV-like)"
    if cv > 1.0:                return "bursting"
    return "regular-spiking (pyramidal-like)"
```

---

## 6. Single-Cell Transcriptome Revolution

- scRNA-seq: unsupervised clustering by gene expression → thousands of "transcriptomic types"
- **BICCN/BRAIN Initiative Cell Census**: mouse/human cortex cell atlas
- **Patch-seq**: e-phys + morphology + transcriptome same cell (see [Patch_Clamp](../07_Neurotech_Frontiers/Patch_Clamp.en.md))
- Debate: are types discrete or a continuum?

---

## 7. Type = Functional Specificity

- Different types → different circuit roles (PV gamma vs SST dendritic gating)
- Disease cell specificity: PV interneurons → SCZ (see [Schizophrenia](../08_Neuro_Disorders/Schizophrenia.en.md)); DA → PD; motor neurons → ALS
- Cell-type-specific tools (Cre-line) = basis of modern causal experiments (see [Optogenetics_Advanced](../07_Neurotech_Frontiers/Optogenetics_Advanced.en.md))

---

## 8. Relation to AI

- "Neuron = perceptron" extreme simplification: biology has thousands of types × dendritic computation (see [Dendrites](Dendrites.en.md))
- Excitatory/inhibitory + disinhibition ↔ gating / normalization (see [Normalization Models](../05_Computational_Neuroscience/Normalization_Models.en.md))
- Cell type diversity = source of computational diversity (ANN units homogeneous)

---

## 9. Evolutionary Conservation

- Major types (pyramidal/PV/SST/VIP) conserved across mammals
- Transcriptomically defined types partly cross-species (also human-specific expansions)
- See [Comparative Neuroscience](../00_Foundations/Comparative_Neuroscience.en.md)

---

## 10. Common Pitfalls

### 10.1 Neurons Homogeneous

Thousands of types × multidimensional; function highly specific.

### 10.2 Inhibitory Neurons Single

≥ three major classes (PV/SST/VIP) + many subtypes, functionally distinct.

### 10.3 Morphology Determines All

Need multimodal (morphology + e-phys + transcriptome) joint typing.

### 10.4 Types Discretely Clear

Partly continuum; discrete vs continuous still debated.

### 10.5 = Perceptron

Ignores type diversity + dendritic + E/I/disinhibition structure.

---

## 11. Related Concepts

- **Same section**: [Neuron](Neuron.en.md), [Dendrites](Dendrites.en.md), [Synapse](Synapse.en.md)
- **Anatomy**: [Cortex](../01_Neuroanatomy/Cortex.en.md), [Cerebellum](../01_Neuroanatomy/Cerebellum.en.md), [Basal_Ganglia](../01_Neuroanatomy/Basal_Ganglia.en.md)
- **Foundation**: [Neural Circuits](../00_Foundations/Neural_Circuits.en.md), [Comparative Neuroscience](../00_Foundations/Comparative_Neuroscience.en.md)
- **Frontiers**: [Patch_Clamp](../07_Neurotech_Frontiers/Patch_Clamp.en.md)

---

## References

1. **Zeng, H. & Sanes, J. R.** "Neuronal cell-type classification: challenges, opportunities and the path forward." *Nat Rev Neurosci*, 2017.
2. **Tasic, B. et al.** "Shared and distinct transcriptomic cell types across neocortical areas." *Nature*, 2018.
3. **Tremblay, R. et al.** "GABAergic interneurons in the neocortex: from cellular properties to circuits." *Neuron*, 2016.
4. **BICCN** "A multimodal cell census and atlas of the mammalian primary motor cortex." *Nature*, 2021.
