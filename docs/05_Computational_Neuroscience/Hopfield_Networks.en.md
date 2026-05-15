# Hopfield Networks

> *Hopfield 1982 proposed an energy-based recurrent neural network. Patterns are stored as attractor states; recall = energy descent to fixed point. Inspired CAM (content-addressable memory) + modern Hopfield network (Ramsauer 2020 attention link). Transformer attention's unsung ancestor. Hopfield + Hinton awarded 2024 Nobel Prize in Physics.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [LTP/LTD](../02_Cellular_Molecular/LTP_LTD.en.md), [Hippocampus Anatomy](../01_Neuroanatomy/Hippocampus_Anatomy.en.md)

---

## 1. Basics

- N binary neurons $s_i \in \{-1, +1\}$
- Fully connected + symmetric weights $W_{ij} = W_{ji}$, $W_{ii} = 0$
- Energy function:

$$E = -\frac{1}{2} \sum_{i \neq j} W_{ij} s_i s_j$$

- Recall dynamics: per step, randomly pick neuron to flip, only keep flips lowering E
- Patterns stored as local minima of E

---

## 2. Hebbian Storage

Store P patterns $\{\xi^\mu\}_{\mu=1}^P$:

$$W_{ij} = \frac{1}{N} \sum_{\mu=1}^P \xi_i^\mu \xi_j^\mu$$

- Capacity: $P_{\max} \approx 0.138 N$ (Amit 1985)
- Beyond capacity → spurious mixed states

---

## 3. Recall Process

1. Input noisy / partial pattern as initial $s(0)$
2. Async update: $s_i \leftarrow \text{sign}(\sum_j W_{ij} s_j)$
3. Network converges to nearest attractor
4. Output clean pattern

---

## 4. PyTorch — Hopfield

```python
import torch

class HopfieldNet:
    def __init__(self, N):
        self.W = torch.zeros(N, N)
        self.N = N
    
    def store(self, patterns):
        """Hebbian rule: W = (1/N) Σ ξ ξ^T."""
        for p in patterns:
            self.W += torch.outer(p, p) / self.N
        self.W.fill_diagonal_(0)
    
    def recall(self, s, steps=100):
        """Async update."""
        for _ in range(steps):
            i = torch.randint(0, self.N, (1,)).item()
            s[i] = torch.sign(self.W[i] @ s)
        return s
    
    def energy(self, s):
        return -0.5 * (s @ self.W @ s)
```

---

## 5. Neuroscience Mapping

- **CA3 in Hippocampus**: highly recurrent; often modeled as Hopfield-like attractor
- **Pattern completion**: hippocampus function akin to Hopfield recall
- **Episodic memory**: pattern store + recall
- McNaughton & Morris (1987) proposed CA3 implements attractor network

---

## 6. Modern Hopfield Network (Ramsauer 2020)

- Continuous (not binary) Hopfield network
- Capacity exponentially scaling ($\sim e^N$)
- Update rule **equivalent to Transformer attention**:

$$s^{\text{new}} = X \cdot \text{softmax}(\beta X^T s)$$

- Hopfield as the hidden foundation of Transformer

---

## 7. Hopfield ↔ Boltzmann Machine

- Hopfield (deterministic) + temperature → Boltzmann machine (probabilistic)
- Hinton & Sejnowski 1985 introduced hidden units → RBM → modern DL
- 2024 Nobel Physics to Hopfield + Hinton (foundational contributions)

---

## 8. Spin Glass Analogy

- Hopfield network resembles Ising model
- Statistical physics: free energy landscape
- Replica trick (Amit, Gutfreund, Sompolinsky 1985) yields capacity bound

---

## 9. Limitations

- Binary neurons unrealistic
- Capacity 0.138N too low
- Many spurious states
- Async update slow
- Largely superseded by modern Hopfield + attention

---

## 10. Common Pitfalls

### 10.1 Storing > 0.138N

Too many patterns → total mess, no recall.

### 10.2 W Not Symmetric

Breaks energy guarantee; may not converge.

### 10.3 Energy Monotone Decrease

Only under async + single-flip; sync update may oscillate.

### 10.4 Hopfield = LSTM Error

LSTM is RNN via gated memory; Hopfield is attractor-based associative.

### 10.5 Modern Hopfield ≠ Classical

Continuous version has exponential capacity; links to Transformer.

---

## 11. Related Concepts

- **Same section**: [Predictive_Coding](Predictive_Coding.en.md), [Bayesian_Brain](Bayesian_Brain.en.md)
- **Anatomy**: [Hippocampus](../01_Neuroanatomy/Hippocampus_Anatomy.en.md)
- **Cellular**: [LTP/LTD](../02_Cellular_Molecular/LTP_LTD.en.md)
- **AI**: Transformer attention — https://jeffliulab.github.io/ai-notes/02_Deep_Learning/05_Transformers/

---

## References

1. **Hopfield, J. J.** "Neural networks and physical systems with emergent collective computational abilities." *PNAS*, 1982.
2. **Amit, D. J., Gutfreund, H., Sompolinsky, H.** "Storing infinite numbers of patterns in a spin-glass model of neural networks." *Phys Rev Lett*, 1985.
3. **Ramsauer, H. et al.** "Hopfield networks is all you need." *ICLR*, 2021.
4. **Hertz, Krogh, Palmer** *Introduction to the Theory of Neural Computation*, 1991.
