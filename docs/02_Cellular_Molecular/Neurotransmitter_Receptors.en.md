# Neurotransmitter Receptors

> *Receptors determine transmitter effects, not the transmitter itself. Two classes: ionotropic (ligand-gated ion channels, fast ms) vs metabotropic (GPCR, slow + lasting + amplifying). Same transmitter via different receptors can excite or inhibit (ACh: nAChR excites vs mAChR can inhibit). Receptors are the main battlefield of pharmacology + psychiatric drugs.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Neurotransmitters](Neurotransmitters.en.md), [Synapse](Synapse.en.md)

---

## 1. Two Classes

| | Ionotropic | Metabotropic |
|---|---|---|
| Structure | Ligand-gated ion channel | GPCR (7-transmembrane) |
| Speed | Fast (ms) | Slow (100 ms-s) |
| Duration | Short | Long |
| Mechanism | Directly opens pore | G protein → second messenger cascade |
| Amplification | None | Yes (cascade amplification) |
| Example | nAChR, AMPA, NMDA, GABA-A | mAChR, mGluR, GABA-B, most DA/5-HT |

---

## 2. "Effect Defined by Receptor"

- Same transmitter + different receptor → opposite effects
  - ACh: nAChR (cations, excite) vs mAChR M2 (open K⁺, inhibit heart)
  - Glutamate: AMPA/NMDA (excite) vs mGluR (modulate)
  - GABA: GABA-A (Cl⁻, fast inhibition) vs GABA-B (K⁺/Ca²⁺, slow inhibition)
- → Don't ask "is this transmitter excitatory or inhibitory", ask "which receptor"

---

## 3. Key Ionotropic

| Receptor | Ligand | Ions |
|---|---|---|
| AMPA | Glu | Na⁺ (fast EPSP) |
| NMDA | Glu | Ca²⁺ (Mg²⁺ voltage-gated, coincidence detector, LTP) |
| GABA-A | GABA | Cl⁻ (fast IPSP; benzodiazepine site) |
| Glycine-R | Gly | Cl⁻ (spinal inhibition) |
| nAChR | ACh | Na⁺/Ca²⁺ |
| 5-HT3 | 5-HT | Cations (antiemetic target) |

---

## 4. NMDA — Coincidence Detector

- Requires **glutamate binding + postsynaptic depolarization** (gradual Mg²⁺ unblock) dual condition
- → Ca²⁺ influx → triggers LTP (see [LTP_LTD](LTP_LTD.en.md))
- Molecular-level Hebbian "fire together"
- Explains associative learning + is ketamine/PCP/memory drug target

---

## 5. PyTorch — NMDA Coincidence Detection

```python
import torch

def nmda_current(glutamate_bound, post_voltage, mg_block_v=-40.0):
    """NMDA: needs BOTH ligand AND depolarization (Mg2+ relief)."""
    mg_relief = torch.sigmoid((post_voltage - mg_block_v) / 10.0)
    ca_influx = glutamate_bound * mg_relief        # AND gate
    return ca_influx   # high only if pre-active AND post-depolarized
```

---

## 6. Metabotropic Signaling

```
Ligand → GPCR → G protein (Gs/Gi/Gq)
  Gs → ↑cAMP → PKA
  Gi → ↓cAMP / open K⁺ (GIRK)
  Gq → PLC → IP3+DAG → Ca²⁺ / PKC
→ phosphorylate channels/receptors/transcription factors (CREB) → short + long-term effects
```

Slow, amplifying, plasticity + gene expression (see [Second_Messengers](Second_Messengers.en.md)).

---

## 7. Receptor Regulation

- **Desensitization**: sustained ligand → inactivation
- **Up/downregulation**: chronic drugs → receptor number change (tolerance / withdrawal basis)
- **Allosteric modulation**: benzodiazepines (GABA-A positive allosteric), barbiturates
- Receptor subunit combinations → diverse pharmacology (GABA-A α subunit → sedation vs anxiolysis)

---

## 8. Pharmacology — Main Battlefield

| Drug | Receptor action |
|---|---|
| Benzodiazepine | GABA-A positive allosteric (anxiolytic/sedative) |
| Ketamine | NMDA antagonist (anesthetic/rapid antidepressant) |
| Opioids | μ-opioid GPCR (analgesia/addiction) |
| Antipsychotics | D2 antagonist |
| SSRI | Indirect ↑5-HT (downstream receptor adaptation) |
| Nicotine | nAChR agonist (addiction) |

---

## 9. Relation to AI

- Ionotropic (fast) ≈ ANN instant weighting; Metabotropic (slow/amplifying) ≈ neuromodulation/hyperparameters + slow weights
- NMDA coincidence detection ↔ multiplicative gating / Hebbian + three-factor (see [Synaptic Plasticity Models](../05_Computational_Neuroscience/Synaptic_Plasticity_Models.en.md))
- Receptor diversity ≫ ANN single activation → richer biological computation

---

## 10. Common Pitfalls

### 10.1 Transmitter Determines Excitation/Inhibition

Defined by **receptor**; same transmitter can excite or inhibit (ACh/Glu/GABA).

### 10.2 Ionotropic = All Fast

NMDA is slower + Ca²⁺ signal triggers slow cascade.

### 10.3 Metabotropic Unimportant

Modulation + plasticity + gene expression core; main drug target.

### 10.4 One Receptor One Subtype

Many subunit combinations → diverse pharmacology (dozens of GABA-A subtypes).

### 10.5 SSRI Immediate Effect (Receptor)

5-HT rises instantly, but receptor adaptation + downstream plasticity take weeks (clinical lag).

---

## 11. Related Concepts

- **Same section**: [Neurotransmitters](Neurotransmitters.en.md), [Synapse](Synapse.en.md), [Ion_Channels](Ion_Channels.en.md), [LTP_LTD](LTP_LTD.en.md), [Second_Messengers](Second_Messengers.en.md)
- **Systems**: [Reward_System](../03_Systems_Neuroscience/Reward_System.en.md)
- **Computational**: [Synaptic Plasticity Models](../05_Computational_Neuroscience/Synaptic_Plasticity_Models.en.md)

---

## References

1. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
2. **Traynelis, S. F. et al.** "Glutamate receptor ion channels: structure, regulation, and function." *Pharmacol Rev*, 2010.
3. **Nicoll, R. A.** "A brief history of long-term potentiation." *Neuron*, 2017.
4. **Hille, B.** *Ion Channels of Excitable Membranes*. 3rd ed., 2001.
