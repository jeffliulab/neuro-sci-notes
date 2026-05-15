# Information Theory in Neuroscience

> *Shannon's 1948 information theory is the language for quantifying neural coding: entropy, mutual information, channel capacity, Fisher information. Used to measure how much stimulus info a spike train carries, sensory channel capacity, redundancy. The Information Bottleneck connects to deep learning. It's the mathematical basis of efficient coding and neural coding.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Neural Coding](../00_Foundations/Neural_Coding.en.md), [Efficient Coding](Efficient_Coding.en.md), probability

---

## 1. Basic Quantities

- **Entropy**: $H(X) = -\sum p(x)\log p(x)$ — uncertainty
- **Conditional entropy**: $H(X|Y)$
- **Mutual information**: $I(X;Y) = H(X) - H(X|Y)$
- **Channel capacity**: $C = \max_{p(x)} I(X;Y)$
- Units: bits (log₂)

---

## 2. MI in Neural Coding

$$I(S;R) = \sum_{s,r} p(s,r) \log \frac{p(r|s)}{p(r)}$$

- $S$: stimulus, $R$: neural response (spike count / pattern)
- Measures how much stimulus info a spike train carries
- Upper bound = response entropy (coding capacity)

---

## 3. Estimation Methods + Bias

- Finite samples → MI estimate **positively biased** (Panzeri-Treves correction)
- Methods: direct, binning, KNN (KSG), neural network (MINE)
- High-dim → curse of dimensionality
- Need bias correction + sufficient data

---

## 4. Spike Train Information

- **Spike count**: rate coding information
- **Spike timing**: temporal precision increases MI (Strong 1998)
- **Information rate**: bits/sec
- Fly H1 neuron ~ tens of bits/sec (classic)

---

## 5. PyTorch — Mutual Information Estimation

```python
import torch

def mutual_information_discrete(joint_counts):
    """MI from a joint histogram p(s, r)."""
    p = joint_counts / joint_counts.sum()
    px = p.sum(dim=1, keepdim=True)
    py = p.sum(dim=0, keepdim=True)
    mask = p > 0
    mi = (p[mask] * torch.log2(p[mask] / (px @ py)[mask])).sum()
    return mi  # bits
```

---

## 6. Fisher Information

$$J(\theta) = \mathbb{E}\!\left[\left(\frac{\partial \ln p(r|\theta)}{\partial \theta}\right)^2\right]$$

- Local measure of coding precision
- Cramér-Rao: $\text{Var}(\hat\theta) \geq 1/J(\theta)$
- Population: tuning curve slope determines Fisher info
- Linked to optimal stimulus coding

---

## 7. Information Bottleneck (Tishby)

$$\min \; I(X;T) - \beta I(T;Y)$$

- Compress $X→T$ while preserving $Y$-relevant info
- Explains: sensory systems keep task-relevant, discard redundancy
- Tishby 2017: DL training = two-phase IB (debated)
- Connects to efficient coding (see [Efficient Coding](Efficient_Coding.en.md))

---

## 8. Redundancy + Synergy

- Multi-neuron joint coding:
  - **Redundant**: repeated information (robust)
  - **Synergistic**: joint > sum of parts
  - **Independent**
- Partial Information Decomposition (Williams & Beer)
- Tool for population coding efficiency analysis

---

## 9. Relation to AI

| Information theory | AI |
|---|---|
| Mutual information | InfoNCE, MINE, contrastive learning |
| Information Bottleneck | representation learning, compression |
| Channel capacity | rate-distortion, quantization |
| Entropy | cross-entropy loss, max-entropy RL |
| MI estimation | representation probing |

---

## 10. Common Pitfalls

### 10.1 MI Estimate Unbiased

Finite samples positively biased; bias correction essential.

### 10.2 High MI = Brain Uses All

May include info the brain doesn't read out (decodable ≠ used).

### 10.3 Entropy = Information (Semantic)

Shannon information ≠ meaning; just statistical surprise.

### 10.4 IB Explains DL Settled

Tishby DL-IB claim has replication controversy.

### 10.5 More Bits = Better Coding

Need task-relevant; irrelevant info useless (IB view).

---

## 11. Related Concepts

- **Same section**: [Efficient Coding](Efficient_Coding.en.md), [Predictive Coding](Predictive_Coding.en.md), [Free Energy Principle](Free_Energy_Principle.en.md)
- **Foundation**: [Neural Coding](../00_Foundations/Neural_Coding.en.md), [Statistics in Neuroscience](../00_Foundations/Statistics_in_Neuroscience.en.md)
- **AI**: contrastive learning, representation learning

---

## References

1. **Shannon, C. E.** "A mathematical theory of communication." *Bell Syst Tech J*, 1948.
2. **Borst, A. & Theunissen, F. E.** "Information theory and neural coding." *Nat Neurosci*, 1999.
3. **Panzeri, S. et al.** "Correcting for the sampling bias problem in spike train information measures." *J Neurophysiol*, 2007.
4. **Tishby, N. & Zaslavsky, N.** "Deep learning and the information bottleneck principle." *IEEE ITW*, 2015.
