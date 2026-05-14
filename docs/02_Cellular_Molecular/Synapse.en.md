# Synapse — Chemical / Electrical Junctions Between Neurons

> *Synapses are functional connection points between two neurons. **Chemical synapses** (majority) transmit signals via neurotransmitter release; **electrical synapses** (gap junctions) directly pass ions. A pyramidal neuron has 10³-10⁴ synaptic inputs. Synaptic plasticity (LTP/LTD) is the cellular basis of learning and memory. This article covers structure, mechanism, plasticity.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Neuron](Neuron.en.md), [Action Potential](Action_Potential.en.md)

---

## 1. Chemical Synapse Structure

```
Pre-synaptic terminal ─ synaptic cleft (20-40nm) ─ Post-synaptic membrane
   vesicles + Ca channels                          receptors + ion channels
```

### 1.1 Pre-synaptic Side

- **Vesicles** (40-200nm diameter): contain neurotransmitter (NT), ~1000-10000 molecules each
- **Active zone**: vesicle fusion release site
- **Voltage-gated Ca²⁺ channels**: action potential arrives → open → Ca²⁺ inflow

### 1.2 Synaptic Cleft

20-40 nm gap, contains ECM (extracellular matrix).

### 1.3 Post-synaptic Side

- **Receptors**:
  - **Ionotropic**: ligand directly opens ion channel (AMPA, NMDA, GABA-A)
  - **Metabotropic**: GPCR + secondary messenger (slow)
- **PSD (postsynaptic density)**: rich in scaffold proteins (PSD-95)
- Dendritic spine (~1μm head + 0.1μm neck) is main excitatory receptor site

---

## 2. Synaptic Transmission Step-by-step

```
1. AP arrives at axon terminal
2. Voltage Ca channels open, Ca²⁺ inflow
3. Ca²⁺ triggers vesicle fusion with active zone (SNARE protein mediated)
4. NT released into cleft (μs diffusion)
5. NT binds post-synaptic receptor
6. Ionotropic: ion flow → EPSP / IPSP
   Metabotropic: signaling cascade → slow modulation
7. NT reuptake / degradation / diffusion
```

Total latency ~ 0.5-2 ms.

---

## 3. EPSP vs IPSP

### 3.1 Excitatory Post-Synaptic Potential (EPSP)

- Glutamate → AMPA/NMDA → Na+ inflow → depolarize
- Single EPSP: ~ 0.1-1 mV
- Tens-hundreds EPSPs summed temporally/spatially → soma reaches threshold → spike

### 3.2 Inhibitory Post-Synaptic Potential (IPSP)

- GABA → GABA-A → Cl- inflow → hyperpolarize
- Suppresses spike generation

### 3.3 Summation

- **Temporal**: same input fast consecutive → large EPSP
- **Spatial**: multiple inputs synchronized → large EPSP

---

## 4. Synaptic Plasticity — Cellular Mechanism of Learning

### 4.1 LTP (Long-Term Potentiation)

Strong / high-frequency stimulation → synaptic strength **long-term enhanced** (hours-days)
- Bliss & Lømo 1973 first observed in hippocampus
- Involves NMDA receptor (Ca dependence + simultaneous pre/post activity)
- "Cells that fire together, wire together" (Hebb 1949)

### 4.2 LTD (Long-Term Depression)

Weak / low-frequency stimulation → synaptic strength long-term reduced.

### 4.3 STDP (Spike-Timing-Dependent Plasticity)

- Pre spike slightly **before** post → LTP
- Pre spike slightly **after** post → LTD
- Tens of ms window

### 4.4 vs AI Backprop

- Backprop: global gradient, needs forward + backward + full labels
- STDP: local spike-timing rule, biologically realizable
- They can be mathematically equivalent in some settings (Lillicrap 2020 review)

---

## 5. Electrical Synapse (Gap Junction)

- Connexin proteins form channels, direct ion / small molecule passage
- Bidirectional, fast (no delay)
- Mainly in interneurons, glia, cardiac muscle
- ~ 1-5% of total synapses

---

## 6. Synapse Count and Connectivity

- Pyramidal neuron: ~ 10,000 synapses (5,000 input + 5,000 output)
- Whole human brain: ~ 10¹⁴ synapses
- Density varies by cortex layer (L2/3 high, L1 low)

---

## 7. PyTorch — Synapse Model

```python
import torch, torch.nn as nn

class ChemicalSynapse:
    """Simple kinetic model."""
    def __init__(self, tau_rise=1, tau_decay=5, weight=1.0):
        self.r = 0
        self.s = 0
        self.tau_rise, self.tau_decay, self.weight = tau_rise, tau_decay, weight
    
    def step(self, pre_spike, dt=0.1):
        if pre_spike:
            self.r = 1
        self.s += (self.r - self.s) / self.tau_rise * dt
        self.r *= (1 - dt / self.tau_decay)
        return self.weight * self.s

syn = ChemicalSynapse()
psc = []
for t in range(1000):
    spike = 1 if t % 100 == 0 else 0
    psc.append(syn.step(spike))
```

---

## 8. Synaptic Defects / Pathology

- **Alzheimer's**: synapse loss precedes neuron loss
- **Autism**: abnormal synaptic pruning
- **Schizophrenia**: NMDA hypofunction hypothesis
- **Parkinson's**: dopaminergic synapse degeneration
- **Addiction**: synaptic plasticity hijacked by drugs

---

## 9. Modern Research Tools

- **Patch clamp**: single-synapse electrophysiology
- **Imaging**: calcium imaging (Ca²⁺ as activity proxy)
- **Optogenetics**: light-controlled specific synapses
- **CLARITY / iDISCO**: tissue-clearing whole-brain imaging
- **Single-synapse RNA**: synapse-specific mRNA

---

## 10. Common Pitfalls

### 10.1 Synapse "Static" Misconception

Synapses constantly turnover, LTP/LTD throughout life.

### 10.2 Excitatory-Only Focus

Inhibitory equally important, balance is key.

### 10.3 Count = Function Misconception

Some synapses are silent (NMDA-only), don't strongly affect firing.

### 10.4 Not Simple Mapping to AI Weights

Synaptic weights are dynamic, probabilistic, neuromodulator-dependent.

### 10.5 Glia Participate

Past 20 years revealed astrocytes directly influence synaptic transmission.

---

## 11. Related Concepts

- **AI comparison**: https://jeffliulab.github.io/ai-notes/02_Deep_Learning/01_Intro/MLP/, https://jeffliulab.github.io/ai-notes/01_AI/03_Frontiers/03_World_Models/WM_Predictive_Coding/

---

## References

1. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
2. **Bliss, T. V. P. & Lømo, T.** "Long-lasting potentiation of synaptic transmission in the dentate area of the anaesthetized rabbit following stimulation of the perforant path." *J Physiol*, 1973.
3. **Bi, G. & Poo, M.-M.** "Synaptic Modifications in Cultured Hippocampal Neurons (STDP)." *J Neurosci*, 1998.
4. **Bear, M. F. et al.** *Neuroscience: Exploring the Brain*. 4th ed., 2015.
5. **Lillicrap, T. et al.** "Backpropagation and the brain." *Nat Rev Neurosci*, 2020.
