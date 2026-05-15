# Dendrites

> *Dendrites are the neuron's input device — not passive cables. They feature dendritic spikes, NMDA spikes, Ca²⁺ plateaus, and active conductances. A pyramidal neuron has thousands of synapses on its dendrites. Spines are key sites of plasticity. "Dendritic computation" shows that a single neuron can perform complex computation, challenging the point-neuron assumption.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Neuron](Neuron.en.md), [Synapse](Synapse.en.md), [Action Potential](Action_Potential.en.md)

---

## 1. Morphological Classes

```
Cortical pyramidal:
        apical dendrite ──┐
                          ├── tuft branches
        soma  ──┐         │
                ├── basal dendrites
        axon  ──┘
```

- **Pyramidal cell**: apical + basal, thousands of spines
- **Stellate cell**: many radial branches
- **Purkinje cell**: extremely branched (~ 200,000 spines)
- **Granule cell**: simple

---

## 2. Passive Cable Theory (Rall 1960)

- Cable equation: $\tau \partial V / \partial t = \lambda^2 \partial^2 V / \partial x^2 - V$
- **Length constant** $\lambda$: signal decay distance
- **Time constant** $\tau = R_m C_m$
- Distal input attenuates (typically 10×)

---

## 3. Active Dendritic Channels

| Channel | Location | Role |
|---|---|---|
| Nav | Distal | Dendritic spike |
| Cav (L,N,T) | Throughout | Ca²⁺ plateau |
| NMDA | Spines | NMDA spike |
| HCN (Ih) | Far from soma | Normalize inputs |
| Kv (A-type) | Apical | Shunt back-prop AP |

→ Dendrites are not passive; they perform active computation.

---

## 4. Three Dendritic Spike Types

### 4.1 Na+ spike

- Similar to axonal AP
- Fast, local

### 4.2 NMDA spike

- Triggered by clustered input on a dendritic segment
- Ca²⁺ + Na+ influx
- Lasts ~ 50 ms
- Key for plasticity

### 4.3 Ca²⁺ plateau

- Apical tuft
- ~ 100 ms long plateau
- Linked to burst firing (Larkum 2013)

---

## 5. Dendritic Spines

- 0.5-2 μm protrusions, ~ 95% excitatory synapses
- Spine head + neck
- **LTP** → spine grows
- **LTD** → spine shrinks
- Spine count = learning capacity proxy
- Schizophrenia, Fragile X → abnormal spines

---

## 6. Coincidence Detection

- Pyramidal: Apical tuft + basal simultaneously active → strong output
- Larkum's "BAC firing"
- Linked to perception (cortical column)

---

## 7. PyTorch — Multi-Compartment Toy

```python
import torch

class MultiCompartmentNeuron(torch.nn.Module):
    """3 compartments: distal dendrite, proximal dendrite, soma."""
    def __init__(self):
        super().__init__()
        self.g_distal_proximal = 0.5
        self.g_proximal_soma = 0.8
    
    def forward(self, V_distal_input, V_proximal_input, dt=0.1, T=100):
        V_d, V_p, V_s = -70.0, -70.0, -70.0
        spikes = []
        for t in range(T):
            V_d = V_d + dt * (-V_d - 70 + V_distal_input - self.g_distal_proximal * (V_d - V_p))
            V_p = V_p + dt * (-V_p - 70 + V_proximal_input - self.g_proximal_soma * (V_p - V_s) + self.g_distal_proximal * (V_d - V_p))
            V_s = V_s + dt * (-V_s - 70 + self.g_proximal_soma * (V_p - V_s))
            if V_s > -55:
                spikes.append(t)
                V_s = -70
        return spikes
```

---

## 8. Single Neuron as Deep Network (Beniaguev 2021)

- Deep CNN fits a single pyramidal neuron → needs 5-8 CNN layers
- Implies: single biological neuron ≈ mini-network
- Challenges "neuron = perceptron" simplification

---

## 9. Synaptic Integration Modes

- **Linear**: distant input, passive
- **Sublinear**: nearby input, shunt
- **Supralinear**: dendritic spike, boost
- Real neurons combine all three

---

## 10. AI Connection

- **Capsule Networks (Hinton)**: analogous to dendritic compartments
- **Poirazi & Mel 2003**: dendrite ~ 2-layer NN
- Simulating dendrites can yield more efficient ANNs

---

## 11. Common Pitfalls

### 11.1 Dendrite = Passive Cable

Wrong; active channels markedly change integration.

### 11.2 NMDA = Glutamate Receptor

NMDA is a glutamate receptor, but dendrite-level NMDA spike involves cluster activation, not single synapse.

### 11.3 Spine = Immutable

Wrong; spines form / vanish on minute timescales.

### 11.4 Point Neuron Suffices

ANN ReLU completely ignores dendritic computation.

### 11.5 Pyramidal Only

Cerebellar Purkinje, basket cells are equally complex.

---

## 12. Related Concepts

- **Same section**: [Neuron](Neuron.en.md), [Synapse](Synapse.en.md), [Action Potential](Action_Potential.en.md), [LTP/LTD](LTP_LTD.en.md)
- **Anatomy**: [Cortex](../01_Neuroanatomy/Cortex.en.md)
- **Computational**: [Hopfield Networks](../05_Computational_Neuroscience/Hopfield_Networks.en.md)

---

## References

1. **Rall, W.** "Theory of physiological properties of dendrites." *Ann NY Acad Sci*, 1962.
2. **London, M. & Häusser, M.** "Dendritic computation." *Annu Rev Neurosci*, 2005.
3. **Larkum, M. E.** "A cellular mechanism for cortical associations." *Trends Neurosci*, 2013.
4. **Beniaguev, D., Segev, I., London, M.** "Single cortical neurons as deep artificial neural networks." *Neuron*, 2021.
