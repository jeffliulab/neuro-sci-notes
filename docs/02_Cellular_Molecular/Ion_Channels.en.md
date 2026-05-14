# Ion Channels — The Brain's "Transistors"

> *Ion channels are pore-forming proteins in membranes that selectively let ions through. The brain has ~400 channel types controlling spikes / synapses / neural modulation. Hille's 1976 patch clamp made single channels visible; Nobel 1991 (Neher, Sakmann). This article covers main types + drug / disease links.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Neuron](Neuron.en.md), [Action Potential](Action_Potential.en.md)

---

## 1. Channel Classification

### 1.1 By Gating

- **Voltage-gated**: V changes trigger open/close
- **Ligand-gated**: NT binding triggers
- **Mechanically-gated**: mechanical force (hair cells, touch)
- **Thermal-gated**: TRP channels (temperature / pain)
- **Light-gated**: opsins (retina + optogenetics ChR2)
- **2nd-messenger-gated**: cAMP (HCN), Ca²⁺-activated K

### 1.2 By Selectivity

- Na+, K+, Ca²⁺, Cl-, non-selective cations

---

## 2. Voltage-Gated Na+ Channel (Nav)

Most critical — generates AP rising phase.

### 2.1 Structure

- α subunit (pore)
- β1-β4 subunits (regulatory)
- 4 voltage sensors

### 2.2 States

- Closed (rest): V < -55 mV
- Open: V > -55 mV → ~ 1 ms
- Inactivated: after 1 ms, inactivation gate closes → can't fire (refractory)

### 2.3 Subtypes (Nav 1.1-1.9)

- Nav 1.1: GABA interneurons — mutation → epilepsy (Dravet syndrome)
- Nav 1.7: pain neurons — mutation → insensitivity / extreme pain
- TTX (pufferfish toxin) blocks Nav

---

## 3. Voltage-Gated K+ Channel (Kv)

- Repolarization
- Many subtypes Kv1-Kv12
- Determines spike width + frequency
- 4-AP blocks → widens spikes

---

## 4. Voltage-Gated Ca²⁺ Channel

- N-, P/Q-, L-, T-type
- N/P type needed for synaptic NT release
- T-type needed for neuron burst firing
- L-type in cardiac, smooth muscle

---

## 5. NMDA Receptor

Special — both ligand- and voltage-gated:

- Glu binding + glycine co-agonist
- V depolarized to remove Mg²⁺ block
- Ca²⁺ inflow ↑ → CaMKII activation → LTP

NMDA antagonists:
- Ketamine (anesthetic, antidepressant)
- PCP (street drug)
- Memantine (Alzheimer treatment)

---

## 6. GABA-A Receptor

Main inhibitory — Cl- channel + multiple binding sites:
- GABA site
- Benzodiazepine site (anxiolytics)
- Barbiturate site
- Alcohol shares site
- Anesthetic propofol

---

## 7. HCN Channel ("Pacemaker")

- Opens at hyperpolarization (opposite normal V gating)
- Slow depolarization → spontaneous firing
- In heart SA node + thalamic neurons
- Ivabradine (arrhythmia drug) blocks

---

## 8. PyTorch — Simplified Channel Kinetics

```python
import torch

class VoltageGatedChannel:
    def __init__(self, g_max, E_rev, V_half=-20, k=10):
        self.g_max = g_max
        self.E_rev = E_rev
        self.V_half, self.k = V_half, k
        self.p_open = 0
    
    def p_open_steady(self, V):
        return 1 / (1 + torch.exp(-(V - self.V_half) / self.k))
    
    def step(self, V, tau=2, dt=0.1):
        p_inf = self.p_open_steady(V)
        self.p_open += dt * (p_inf - self.p_open) / tau
        return self.g_max * self.p_open * (V - self.E_rev)
```

---

## 9. Drugs / Toxins

| Substance | Target | Use |
|---|---|---|
| Tetrodotoxin (TTX) | Nav | Pufferfish toxin (lethal) |
| Saxitoxin (STX) | Nav | Shellfish toxin |
| Bicuculline | GABA-A | Experimental |
| Picrotoxin | GABA-A | Experimental |
| Ketamine | NMDA | Anesthesia / antidepressant |
| Carbamazepine | Nav | Antiepileptic |
| Verapamil | Cav (L-type) | Cardiovascular |

---

## 10. Channelopathies

- **Epilepsy**: Nav 1.1 / KCNQ mutations
- **Long QT syndrome**: Kv 1.5 mutation
- **Migraine**: CACNA1A (Ca²⁺) mutation
- **Cystic fibrosis**: CFTR Cl- channel mutation
- **Hyperkalemic periodic paralysis**: Nav 1.4

---

## 11. Modern Research Techniques

- **Patch clamp**: single channel current
- **Heterologous expression**: express in HEK / oocyte
- **Cryo-EM**: atomic structure
- **Optogenetics**: ChR2, Halorhodopsin, etc.
- **Calcium imaging**: Ca²⁺ activity proxy

---

## 12. Common Pitfalls

### 12.1 Subtypes Matter

> 100 KV subtypes, 9 Nav subtypes. Not specifying subtype is meaningless.

### 12.2 In vitro ≠ In vivo

Heterologous-expressed channels differ from native context.

### 12.3 Multi-state

Channels usually have multiple states (not just open/closed).

### 12.4 Pharmacology Specificity

Many "specific" drugs have off-target effects.

### 12.5 Stoichiometry

NMDA receptor is 4 subunits; different combinations have different properties.

---

## 13. Related Concepts

- **Same section**: [Hodgkin-Huxley](Hodgkin_Huxley.en.md), [Action Potential](Action_Potential.en.md), [Neurotransmitters](Neurotransmitters.en.md)

---

## References

1. **Hille, B.** *Ion Channels of Excitable Membranes*. 3rd ed., Sinauer, 2001.
2. **Neher, E. & Sakmann, B.** *Nature*, 1976.
3. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
