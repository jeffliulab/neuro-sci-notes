# Second Messengers & Signal Transduction

> *Extracellular signal (first messenger: transmitter/hormone) → receptor → **second messenger** (cAMP, Ca²⁺, IP3, DAG, cGMP, NO) → cascade amplification → channels/enzymes/genes. The molecular basis of metabotropic signaling, plasticity, long-term memory (CREB), neuromodulation. Core to Sutherland (cAMP) + Kandel (Aplysia learning) Nobel work.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Neurotransmitter_Receptors](Neurotransmitter_Receptors.en.md), [LTP_LTD](LTP_LTD.en.md)

---

## 1. Signaling Cascade

```
First messenger (transmitter/hormone, extracellular)
   ↓ receptor (GPCR / RTK)
G protein / enzyme
   ↓
Second messenger (cAMP/Ca²⁺/IP3/DAG/cGMP)
   ↓ kinases (PKA/PKC/CaMKII)
Phosphorylation: channels, receptors, enzymes, transcription factors (CREB)
   ↓
Short-term (minutes) + long-term (gene expression, hours-days)
```

Each step **amplifies** (1 receptor → many G proteins → many cAMP → ...).

---

## 2. Major Second Messengers

| Messenger | Source | Downstream |
|---|---|---|
| **cAMP** | AC (adenylyl cyclase) | PKA |
| **Ca²⁺** | Channels/IP3-R release | CaMKII, calmodulin, calcineurin |
| **IP3** | PLC cleaves PIP2 | ER Ca²⁺ release |
| **DAG** | PLC cleaves PIP2 | PKC |
| **cGMP** | GC | PKG; visual transduction |
| **NO** | nNOS (Ca²⁺-dependent) | Diffusible retrograde messenger |

---

## 3. Gs/Gi/Gq Pathways

- **Gs** → AC↑ → cAMP↑ → PKA (e.g., β-adrenergic, D1)
- **Gi** → AC↓ → cAMP↓; or open GIRK (K⁺) (e.g., D2, μ-opioid, GABA-B)
- **Gq** → PLC → IP3 + DAG → Ca²⁺ + PKC (e.g., mGluR1/5, α1)

→ Same messenger different pathway → diverse effects (see [Neurotransmitter_Receptors](Neurotransmitter_Receptors.en.md)).

---

## 4. PyTorch — Cascade Amplification

```python
import torch

def signaling_cascade(ligand, gain_per_step=8.0, n_steps=4):
    """Each tier amplifies: receptor→G→cAMP→PKA→targets."""
    signal = ligand
    trace = [signal]
    for _ in range(n_steps):
        signal = torch.tanh(gain_per_step * signal)   # saturating amplification
        trace.append(signal)
    return trace   # tiny ligand -> large downstream (sensitivity + saturation)
```

---

## 5. Ca²⁺ — Universal Second Messenger

- Resting intracellular ~100 nM, ↑10-100× during signaling (steep gradient → high SNR)
- Sources: voltage/ligand-gated channels + ER (IP3-R/RyR) release
- Decoders: CaMKII (LTP), calcineurin (LTD) — amplitude/duration-dependent bidirectional
- Buffering/pumps rapidly reset → spatiotemporal microdomains

---

## 6. Relation to Learning Memory — CREB

- Sustained/strong signal → PKA/CaMKII enter nucleus → phosphorylate **CREB**
- CREB → transcribe "late" genes → synaptic structural change → **long-term memory**
- Short-term memory (phosphorylation, no transcription needed) vs long-term (needs transcription/translation)
- Kandel's Aplysia classic (Nobel 2000; see [LTP_LTD](LTP_LTD.en.md))

---

## 7. Retrograde Messengers + Diffusion

- **NO / endocannabinoids**: produced postsynaptically → act retrogradely on presynaptic (modulate release)
- Volume transmission: non-synaptic local diffusion
- Explains LTP presynaptic component + heterosynaptic plasticity

---

## 8. Pharmacology / Clinical

- **Caffeine**: phosphodiesterase (PDE) inhibition → cAMP↑ (partial mechanism)
- **Sildenafil**: PDE5 inhibition → cGMP↑
- **Lithium (bipolar)**: inhibits IMPase / GSK-3β (inositol/cAMP pathway hypothesis)
- Cholera toxin (Gs locked), pertussis toxin (Gi inactivated) = classic tools
- Signaling pathway abnormality → various diseases

---

## 9. Relation to AI

- Cascade amplification ↔ nonlinear gain + sensitivity tuning
- cAMP/Ca²⁺ "slow variables" ↔ slow weights / eligibility trace
- The "third factor" of three-factor plasticity = neuromodulator → second messenger (see [Synaptic Plasticity Models](../05_Computational_Neuroscience/Synaptic_Plasticity_Models.en.md))
- Short-term (phosphorylation) vs long-term (transcription) ↔ fast/slow weights

---

## 10. Common Pitfalls

### 10.1 Second Messengers = Slow and Minor

Core to metabotropic + plasticity + long-term memory (amplification + persistence).

### 10.2 Ca²⁺ Single Effect

Amplitude/duration/location-dependent bidirectional (CaMKII↑LTP vs calcineurin↑LTD).

### 10.3 cAMP Only Excites

Depends on downstream; Gi lowers cAMP → inhibition.

### 10.4 Short-term = Long-term Same Mechanism

Short-term=phosphorylation (no transcription needed); long-term needs gene expression (CREB).

### 10.5 Signaling Linear

Each tier amplifies + saturates + crosstalk, highly nonlinear.

---

## 11. Related Concepts

- **Same section**: [Neurotransmitter_Receptors](Neurotransmitter_Receptors.en.md), [LTP_LTD](LTP_LTD.en.md), [Synapse](Synapse.en.md), [Ion_Channels](Ion_Channels.en.md)
- **Computational**: [Synaptic Plasticity Models](../05_Computational_Neuroscience/Synaptic_Plasticity_Models.en.md)
- **Disease**: [Bipolar_Disorder](../08_Neuro_Disorders/Bipolar_Disorder.en.md) (lithium)

---

## References

1. **Sutherland, E. W.** "Studies on the mechanism of hormone action (cAMP)." *Science*, 1972 (Nobel).
2. **Kandel, E. R.** "The molecular biology of memory storage." *Science*, 2001.
3. **Greengard, P.** "The neurobiology of slow synaptic transmission." *Science*, 2001.
4. **Berridge, M. J.** "Inositol trisphosphate and calcium signalling." *Nature*, 1993.
