# Neural Circuits & Motifs

> *A single neuron doesn't compute behavior; the circuit is the functional unit. A few motifs recur: feedforward, feedback, lateral inhibition, recurrent, winner-take-all, CPG (central pattern generator). Understanding motifs bridges single-cell and systems. Many motifs directly inspired ANN architectures.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Neuron](../02_Cellular_Molecular/Neuron.en.md), [Synapse](../02_Cellular_Molecular/Synapse.en.md)

---

## 1. Why Circuit, Not Neuron

- Single neuron is just an input-output unit
- Function emerges from **connectivity patterns**
- Analogy: transistor vs circuit; ANN unit vs architecture

---

## 2. Core Motifs

| Motif | Function |
|---|---|
| **Feedforward excitation** | Signal transmission, amplification |
| **Feedforward inhibition** | Temporal window sharpening |
| **Feedback / recurrent excitation** | Persistent activity, memory |
| **Feedback inhibition** | Stability, gain control |
| **Lateral inhibition** | Contrast enhancement, WTA |
| **Disinhibition** | Gating (inhibit the inhibitor) |
| **Convergence / divergence** | Integration / broadcast |

---

## 3. Lateral Inhibition

- Neighboring neurons mutually inhibit
- → Contrast enhancement (Mach bands)
- Retina horizontal cells, V1 surround
- ANN equivalent: softmax, normalization

---

## 4. Recurrent Excitation → Attractor

- Self-exciting loop → persistent activity (maintained without input)
- Working memory (DLPFC persistent activity)
- Decision making (evidence accumulation)
- See [Hopfield Networks](../05_Computational_Neuroscience/Hopfield_Networks.en.md)

---

## 5. Feedback Inhibition (Gain Control)

- Pyramidal → interneuron → inhibits pyramidal
- Prevents runaway excitation (otherwise → epilepsy)
- E/I balance critical
- Imbalance → autism / schizophrenia hypotheses

---

## 6. Disinhibition (Gating)

- VIP interneuron inhibits SST interneuron → releases pyramidal
- Gating mechanism for attention / learning
- "Inhibition of inhibition = excitation"

---

## 7. Central Pattern Generator (CPG)

- Autonomously produces rhythmic output (no rhythmic input needed)
- Walking, breathing, chewing, swimming
- Classic: lamprey swimming CPG, lobster stomatogastric ganglion (STG)
- Half-center oscillator model

---

## 8. PyTorch — Winner-Take-All Circuit

```python
import torch

def winner_take_all(x, n_iter=10, inhib=1.2, self_exc=1.0):
    """Recurrent lateral inhibition → WTA (one winner)."""
    y = x.clone()
    for _ in range(n_iter):
        total = y.sum()
        y = torch.relu(self_exc * y - inhib * (total - y) + 0.1 * x)
    return y  # converges to single active unit

print(winner_take_all(torch.tensor([0.3, 0.9, 0.5, 0.2])))
```

---

## 9. Canonical Cortical Microcircuit

- 6-layer cortex has a repeating motif (Douglas & Martin)
- L4 receives thalamic input → L2/3 → L5/6 output
- Recurrent + feedforward + feedback combination
- Hypothesis: cortex repeats the same microcircuit across the brain

---

## 10. AI Correspondence

| Circuit motif | ANN |
|---|---|
| Feedforward | MLP / CNN |
| Recurrent excitation | RNN / attractor |
| Lateral inhibition | softmax / LayerNorm |
| Feedback inhibition | gating / normalization |
| Disinhibition | attention gating |
| Canonical microcircuit | Transformer block (analogy) |

---

## 11. Common Pitfalls

### 11.1 Single Neuron Decides Behavior

Rare (except command neurons); usually circuit-level.

### 11.2 Motif List Complete

It's an abstraction; real circuits are mixed + heterogeneous.

### 11.3 Recurrent = Unstable

Needs inhibition balance; balanced network is stable.

### 11.4 Cortex One Microcircuit

Canonical is a simplification; region + species variations.

### 11.5 CPG Needs Sensory Feedback

CPG produces rhythm even isolated; feedback only modulates.

---

## 12. Related Concepts

- **Same section**: [Neural Coding](Neural_Coding.en.md), [Levels of Analysis](Levels_of_Analysis.en.md), [Connectomics](Connectomics.en.md)
- **Cellular**: [Synapse](../02_Cellular_Molecular/Synapse.en.md), [Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.en.md)
- **Computational**: [Hopfield Networks](../05_Computational_Neuroscience/Hopfield_Networks.en.md), [SNN](../05_Computational_Neuroscience/Spiking_Neural_Networks.en.md)

---

## References

1. **Douglas, R. J. & Martin, K. A. C.** "Neuronal circuits of the neocortex." *Annu Rev Neurosci*, 2004.
2. **Marder, E. & Bucher, D.** "Central pattern generators and the control of rhythmic movements." *Curr Biol*, 2001.
3. **Shepherd, G. M.** *The Synaptic Organization of the Brain*. 5th ed., 2004.
4. **Luo, L.** *Principles of Neurobiology*. 2nd ed., 2020.
