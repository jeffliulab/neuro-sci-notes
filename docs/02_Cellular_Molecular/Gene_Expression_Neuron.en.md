# Activity-Dependent Gene Expression

> *Neural activity → Ca²⁺/cAMP → transcription factors (CREB) → immediate early genes (IEG: c-fos, Arc, Zif268) → late effector genes → long-term memory + structural plasticity. Distinguishes short-term (phosphorylation, no transcription) vs long-term (needs transcription/translation). Epigenetics (methylation/histone) regulates long-range. c-fos is the gold-standard "activation marker" for imaging.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Second_Messengers](Second_Messengers.en.md), [LTP_LTD](LTP_LTD.en.md)

---

## 1. Signal → Gene Cascade

```
Activity → Ca²⁺ influx (NMDA/VGCC) + cAMP
   ↓ CaMKIV / PKA / MAPK enter nucleus
phosphorylate CREB (Ser133)
   ↓ + CBP coactivator
Immediate early gene (IEG) transcription: c-fos, Arc, Egr1/Zif268, Npas4
   ↓
Late response genes → receptors/scaffold/synaptic proteins
   ↓
Long-term memory + structural plasticity (hours-days)
```

---

## 2. Short-Term vs Long-Term (Molecular Boundary)

| | Short-term memory | Long-term memory |
|---|---|---|
| Mechanism | Existing protein phosphorylation | New transcription + translation |
| Timescale | Minutes-hours | Hours-lifetime |
| Block | — | Transcription/translation inhibitors block |
| Kandel Aplysia | Short-term facilitation | Long-term facilitation (needs CREB) |

→ "Long-term memory needs new protein synthesis" (classic anisomycin experiment).

---

## 3. Immediate Early Genes (IEG)

| IEG | Role |
|---|---|
| **c-fos / c-jun** | Transcription factor (AP-1) → downstream genes |
| **Arc/Arg3.1** | Effector IEG; AMPA receptor internalization, synaptic scaling |
| **Egr1 (Zif268)** | Transcription factor; LTP maintenance |
| **Npas4** | Activity-induced; regulates excitation/inhibition balance |

IEGs transcribed without new protein (existing TF phosphorylation triggers).

---

## 4. PyTorch — Activity-Threshold Triggered Transcription

```python
import torch

def activity_dependent_transcription(ca_signal, threshold=0.6, T=100):
    """Sustained Ca2+ -> CREB -> IEG; transient -> no transcription."""
    creb_p = 0.0
    ieg = []
    for t in range(T):
        creb_p += 0.1 * (ca_signal[t] - 0.3 * creb_p)   # integrate + decay
        # IEG transcribed only if CREB phosphorylation crosses threshold
        ieg.append(1.0 if creb_p > threshold else 0.0)
    return ieg   # only strong/sustained activity flips gene program
```

---

## 5. c-fos Imaging — Activation Marker

- Neural activity → c-fos expression → antibody staining/reporter → "which neurons just activated"
- **TRAP/FosTRAP**: c-fos-driven inducible Cre → tag + reactivate memory engram (see [Hippocampus_Memory](../03_Systems_Neuroscience/Hippocampus_Memory.en.md))
- Tonegawa engram manipulation classic (optogenetics + c-fos tagging)
- Limitations: slow (1-2 h), threshold-dependent, not all activity

---

## 6. Epigenetic Regulation

- **DNA methylation** (DNMT), **histone acetylation** (HAT/HDAC) → long-range gene accessibility
- Learning alters epigenetic marks → persistent memory (months-years)
- HDAC inhibitors enhance memory (experimental); stress/early experience → persistent epigenetic imprints
- Transgenerational epigenetics (debated)

---

## 7. Relation to Memory Consolidation

- **Cellular consolidation**: transcription/translation window (hours) → engram stabilization
- **Systems consolidation**: hippocampus→cortex (months, see [Memory_Systems](../04_Cognitive_Neuroscience/Memory_Systems.en.md))
- "Synaptic tag and capture": weak synapses "tagged" capture soma-synthesized proteins → explains associative memory enhancement (see [LTP_LTD](LTP_LTD.en.md))

---

## 8. Clinical

- **CREB pathway**: learning-memory drug target (PDE inhibitors etc.)
- **Rett** (MECP2, epigenetic regulator), **Rubinstein-Taybi** (CBP) → ID
- **Addiction**: ΔFosB accumulation (stable IEG variant) → persistent behavioral change (see [Addiction](../08_Neuro_Disorders/Addiction.en.md))
- **Depression/stress**: epigenetic + BDNF transcription changes (see [Depression](../08_Neuro_Disorders/Depression.en.md))

---

## 9. Relation to AI

- Short-term (fast weights/activation) vs long-term (slow weights/write) ↔ dual-timescale learning
- Activity-gated transcription ↔ thresholded consolidation / offline integration after replay
- ΔFosB accumulation ↔ slow integrator variable (addiction persistence)
- Epigenetics ↔ meta-plasticity / learning to learn the learning rate

---

## 10. Common Pitfalls

### 10.1 Memory = Instant Synaptic Weight Change

Long-term memory needs **gene expression + new protein** (transcription inhibitors can block).

### 10.2 c-fos = All Activity

Threshold + slow (1-2 h); nonlinear, not full tagging.

### 10.3 IEGs All Transcription Factors

Arc is an **effector** IEG (directly regulates AMPA), not a TF.

### 10.4 Epigenetics Irreversible

Reversible + dynamic (HDAC/DNMT bidirectional); but some persistent.

### 10.5 Short-term/Long-term Same Mechanism

Short-term=phosphorylation; long-term=transcription/translation (clear molecular boundary).

---

## 11. Related Concepts

- **Same section**: [Second_Messengers](Second_Messengers.en.md), [LTP_LTD](LTP_LTD.en.md), [Neurotrophins](Neurotrophins.en.md), [Synapse](Synapse.en.md)
- **Systems**: [Hippocampus_Memory](../03_Systems_Neuroscience/Hippocampus_Memory.en.md)
- **Cognition**: [Memory_Systems](../04_Cognitive_Neuroscience/Memory_Systems.en.md)
- **Disease**: [Addiction](../08_Neuro_Disorders/Addiction.en.md), [Depression](../08_Neuro_Disorders/Depression.en.md)

---

## References

1. **Kandel, E. R.** "The molecular biology of memory storage: a dialogue between genes and synapses." *Science*, 2001.
2. **West, A. E. & Greenberg, M. E.** "Neuronal activity-regulated gene transcription in synapse development and cognitive function." *Cold Spring Harb Perspect Biol*, 2011.
3. **Tonegawa, S. et al.** "Memory engram cells have come of age." *Neuron*, 2015.
4. **Day, J. J. & Sweatt, J. D.** "Epigenetic mechanisms in cognition." *Neuron*, 2011.
