# Efficient Coding Hypothesis

> *Barlow 1961: sensory systems evolved to efficiently represent natural statistics — remove redundancy, maximize information, energy constraints. Predicts retina/V1 receptive fields, sparse coding, natural scene statistics adaptation. A normative theory exemplar: deriving "what" (neural properties) from "why" (information theory). Directly linked to autoencoders, infomax, sparse representations.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Neural Coding](../00_Foundations/Neural_Coding.en.md), information theory basics

---

## 1. Core Hypothesis

- Sensory signals have massive **redundancy** (natural image pixels highly correlated)
- Neural resources limited (spikes cost energy — see [Brain Energy Metabolism](../00_Foundations/Brain_Energy_Metabolism.en.md))
- → Evolutionary pressure: encode most information with fewest spikes
- Barlow: "redundancy reduction" principle

---

## 2. Infomax Principle

Maximize input-output mutual information:
$$\max_{f} \; I(X; Y), \quad Y = f(X)$$

- Linsker 1988 infomax
- Under constraints (noise, energy) → predicts receptive fields
- Equivalent (under Gaussian) to decorrelation + whitening

---

## 3. Predicted Neural Properties

| Prediction | Observation |
|---|---|
| Retina center-surround | ✓ whitens natural image 1/f² spectrum |
| V1 oriented Gabor | ✓ sparse coding (Olshausen 1996) |
| Sparse firing | ✓ low average rate |
| Adaptation | ✓ remaps to current statistics |
| Color opponent | ✓ decorrelates LMS cones |

---

## 4. Sparse Coding (Olshausen & Field 1996)

Objective: reconstruct natural images + sparsity:
$$\min \; \|I - \sum_i a_i \phi_i\|^2 + \lambda \sum_i |a_i|$$

- Learned $\phi_i$ = localized, oriented, bandpass — **like V1 simple cells**!
- Emerges purely from natural image statistics + sparsity constraint

---

## 5. Redundancy Reduction vs Exploitation

- Early Barlow: remove redundancy (independent components)
- Later revision: redundancy is also useful (noise robustness, error correction)
- Modern: efficient ≠ complete redundancy removal; it's the optimum under task + noise constraints

---

## 6. PyTorch — Sparse Coding (Olshausen)

```python
import torch

def sparse_coding_step(images, dictionary, lam=0.1, lr=0.01, n_inf=50):
    """Infer sparse codes a, then update dictionary phi."""
    a = torch.zeros(dictionary.shape[1], images.shape[1], requires_grad=True)
    opt = torch.optim.SGD([a], lr=lr)
    for _ in range(n_inf):
        opt.zero_grad()
        recon = dictionary @ a
        loss = ((images - recon)**2).sum() + lam * a.abs().sum()
        loss.backward(); opt.step()
    # Dictionary update (Hebbian-like)
    with torch.no_grad():
        resid = images - dictionary @ a
        dictionary += lr * resid @ a.t()
        dictionary /= dictionary.norm(dim=0, keepdim=True) + 1e-6
    return dictionary, a.detach()
```

---

## 7. Adaptation

- Neurons dynamically recalibrate to current input statistics
- Contrast adaptation, light adaptation
- = online efficient recoding (maximize effective dynamic range)
- Explains aftereffects (motion aftereffect etc.)

---

## 8. Relation to AI

| Efficient coding | AI |
|---|---|
| Infomax | InfoMax / InfoNCE, contrastive learning |
| Sparse coding | sparse autoencoder, L1, dictionary learning |
| Redundancy reduction | ICA, whitening, BatchNorm |
| Predictive redundancy removal | predictive coding, next-token |
| Natural scene statistics | self-supervised pretraining |

---

## 9. Normative Methodology

- "Normative": derive neural properties from optimality principles (not pure description)
- Contrast with mechanistic (how implemented)
- Efficient coding is a Marr computational-level exemplar (see [Levels of Analysis](../00_Foundations/Levels_of_Analysis.en.md))

---

## 10. Common Pitfalls

### 10.1 Complete Redundancy Removal

Redundancy useful for robustness + error correction; not fully removed.

### 10.2 Infomax Sole Objective

Task-relevant info (not all info) more accurate (IB — information bottleneck).

### 10.3 Explains All RFs

Strong constraint + assumptions; not applicable to all regions (higher areas better task-driven).

### 10.4 Sparser = Better

Too sparse → information loss; there's an optimal λ.

### 10.5 Normative = Real Mechanism

Explains "why," doesn't guarantee brain uses that algorithm.

---

## 11. Related Concepts

- **Same section**: [Predictive Coding](Predictive_Coding.en.md), [Bayesian Brain](Bayesian_Brain.en.md), [Free Energy Principle](Free_Energy_Principle.en.md)
- **Foundation**: [Neural Coding](../00_Foundations/Neural_Coding.en.md), [Brain Energy Metabolism](../00_Foundations/Brain_Energy_Metabolism.en.md)
- **Systems**: [Visual System](../03_Systems_Neuroscience/Visual_System.en.md)
- **AI**: self-supervised, contrastive learning

---

## References

1. **Barlow, H. B.** "Possible principles underlying the transformation of sensory messages." 1961.
2. **Olshausen, B. A. & Field, D. J.** "Emergence of simple-cell receptive field properties by learning a sparse code for natural images." *Nature*, 1996.
3. **Linsker, R.** "Self-organization in a perceptual network." *Computer*, 1988.
4. **Simoncelli, E. P. & Olshausen, B. A.** "Natural image statistics and neural representation." *Annu Rev Neurosci*, 2001.
