# Synaptic Vesicle Cycle

> *Core of chemical synaptic transmission: vesicle loading → docking → priming → Ca²⁺-triggered fusion (exocytosis) → endocytic recycling. SNARE proteins (syntaxin/SNAP-25/synaptobrevin) are the fusion machine, synaptotagmin is the Ca²⁺ sensor. 2013 Nobel (Südhof/Rothman/Schekman). Quantal release (Katz) is the basis. Botulinum/tetanus toxins cleave SNAREs.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Synapse](Synapse.en.md), [Action_Potential](Action_Potential.en.md)

---

## 1. Cycle Steps

```
1. Loading (transporter fills with transmitter)
2. Translocation + docking (active zone)
3. Priming (SNARE partial assembly)
4. AP → Ca²⁺ influx (P/Q/N-type channels)
5. Synaptotagmin senses Ca²⁺ → SNARE zippering → fusion (< 1 ms)
6. Transmitter released into cleft
7. Endocytic recycling (clathrin / kiss-and-run)
8. Reloading
```

---

## 2. SNARE Fusion Machine

- **t-SNARE**: syntaxin-1 + SNAP-25 (plasma membrane)
- **v-SNARE**: synaptobrevin/VAMP (vesicle)
- Four-helix bundle "zipper" assembly → pulls membranes together → fusion
- NSF/SNAP disassemble for reuse
- Botulinum (BoNT)/tetanus toxin = proteases cleaving SNAREs → block release

---

## 3. Synaptotagmin — Ca²⁺ Sensor

- C2 domains bind Ca²⁺ → trigger fast synchronous release
- Low affinity, fast (matches ms-scale Ca²⁺ microdomain)
- Knockout → loss of synchronous fast release (asynchronous residual)
- Determines "why release happens when Ca²⁺ enters"

---

## 4. Quantal Release (Katz)

- Transmitter released in **quanta** (single vesicle ~ thousands of molecules)
- mEPP (miniature end-plate potential) = single spontaneous vesicle
- EPP = integer multiple of mEPP (quantal summation)
- Pr (release probability) × N (sites) × q (quantal size) = synaptic strength

---

## 5. PyTorch — Quantal Release (Binomial)

```python
import torch

def quantal_release(n_sites=10, p_release=0.3, q=1.0, trials=1000):
    """Binomial vesicle release; EPSP = k * q (Katz)."""
    k = torch.distributions.Binomial(n_sites, p_release).sample((trials,))
    epsp = k * q
    # Short-term plasticity: Pr changes (facilitation/depression)
    return epsp.mean(), epsp.var()   # mean & variance → quantal analysis
```

---

## 6. Release Probability + Short-Term Plasticity

- **Facilitation**: residual Ca²⁺ → transient Pr↑ (tens of ms)
- **Depression**: vesicle depletion → Pr↓
- High-Pr synapses prone to depression; low-Pr prone to facilitation
- Short-term plasticity = dynamic filter (high/low-pass, see [Synapse](Synapse.en.md))

---

## 7. Endocytic Recycling

- **Clathrin-mediated**: classic, slow (~ 10 s)
- **Kiss-and-run**: transient open/close, fast recycling
- **Bulk endocytosis**: high activity
- Recycling maintains sustained transmission (vesicle pool limited)
- Vesicle pools: readily releasable / recycling / reserve

---

## 8. Clinical

- **Botulinum toxin (Botox)**: cleaves SNAP-25 → muscle relaxation (cosmetic/spasticity/migraine treatment)
- **Tetanus toxin**: cleaves synaptobrevin (inhibitory interneurons) → rigidity
- **Lambert-Eaton**: anti-P/Q Ca²⁺ channel → ↓ release → muscle weakness
- **α-latrotoxin** (black widow): forced release
- Synaptic vesicle protein gene mutations → epilepsy/ID/Parkinson (SYT, SV2A — levetiracetam target)

---

## 9. Relation to AI

- Quantal + stochastic release ↔ synaptic noise / Dropout analogy (random deactivation regularization)
- Short-term plasticity ↔ dynamic weights / adaptive gain (non-static W)
- Limited vesicle pool ↔ resource constraint (energy/bandwidth, see [Brain Energy Metabolism](../00_Foundations/Brain_Energy_Metabolism.en.md))

---

## 10. Common Pitfalls

### 10.1 Release Is Deterministic

Stochastic (quantal + binomial Pr); single AP may not release.

### 10.2 SNARE Just "Docks"

It's a fusion **mechanical machine** (zipper); not a passive anchor.

### 10.3 Pr Fixed

Short-term plasticity dynamically varies (facilitation/depression).

### 10.4 One Vesicle Pool

At least RRP/recycling/reserve multiple pools.

### 10.5 Botox Kills Neurons

It's reversible SNARE block (recovery in months); doesn't kill neurons.

---

## 11. Related Concepts

- **Same section**: [Synapse](Synapse.en.md), [Neurotransmitters](Neurotransmitters.en.md), [Action_Potential](Action_Potential.en.md), [Ion_Channels](Ion_Channels.en.md)
- **Foundation**: [Brain Energy Metabolism](../00_Foundations/Brain_Energy_Metabolism.en.md)
- **Computational**: [Synaptic Plasticity Models](../05_Computational_Neuroscience/Synaptic_Plasticity_Models.en.md)

---

## References

1. **Südhof, T. C.** "The molecular machinery of neurotransmitter release (Nobel Lecture)." *Angew Chem*, 2014.
2. **Katz, B.** *The Release of Neural Transmitter Substances*. 1969.
3. **Jahn, R. & Fasshauer, D.** "Molecular machines governing exocytosis of synaptic vesicles." *Nature*, 2012.
4. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
