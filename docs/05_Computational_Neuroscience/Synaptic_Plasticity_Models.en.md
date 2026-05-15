# Synaptic Plasticity Models

> *From Hebb (1949) to STDP, BCM, three-factor rules. Plasticity rules are the mathematical core of brain learning. Problem: pure Hebbian is unstable → needs normalization (Oja), sliding threshold (BCM), neuromodulator gating (three-factor). These local rules are biologically-plausible alternatives to backprop, also inspiring neuromorphic learning.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [LTP/LTD](../02_Cellular_Molecular/LTP_LTD.en.md), [Neuroplasticity](../00_Foundations/Neuroplasticity.en.md)

---

## 1. Hebbian and Its Problem

$$\Delta w = \eta \, x \, y$$

- Co-activation → strengthening
- Problem: **positive feedback → unbounded growth** (w → ∞)
- Needs stabilization

---

## 2. Oja's Rule (Normalization)

$$\Delta w = \eta \, y (x - y \, w)$$

- Hebbian + implicit normalization
- Converges to principal eigenvector of input covariance (PCA!)
- Weight norm naturally bounded

---

## 3. BCM Rule (Sliding Threshold)

$$\Delta w = \eta \, x \, y (y - \theta_M), \quad \theta_M = \langle y^2 \rangle$$

- Threshold $\theta_M$ slides with average activity
- $y > \theta_M$ → LTP; $y < \theta_M$ → LTD
- Stable + explains ocular dominance plasticity
- Corresponds to metaplasticity

---

## 4. STDP (Spike-Timing-Dependent)

- pre before post (~ ms) → LTP
- post before pre → LTD
- Temporally asymmetric window (exponential)
- Causal learning; see [LTP/LTD](../02_Cellular_Molecular/LTP_LTD.en.md), [SNN](Spiking_Neural_Networks.en.md)

$$\Delta w = \begin{cases} A_+ e^{-\Delta t/\tau_+} & \Delta t > 0 \\ -A_- e^{\Delta t/\tau_-} & \Delta t < 0 \end{cases}$$

---

## 5. Three-Factor Rule

$$\Delta w = \eta \, \underbrace{x \, y}_{\text{Hebbian}} \cdot \underbrace{M}_{\text{neuromodulator}}$$

- Third factor $M$: dopamine / ACh / reward signal
- Solves credit assignment (when to learn)
- Synaptic eligibility trace + delayed reward → reward-modulated STDP
- Connects to RL (see [Reinforcement Learning Brain](Reinforcement_Learning_Brain.en.md))

---

## 6. PyTorch — BCM Rule

```python
import torch

def bcm_update(w, x, lr=0.01, tau_theta=100):
    """BCM with sliding threshold."""
    theta = bcm_update.theta if hasattr(bcm_update, 'theta') else 0.0
    y = (w * x).sum()
    dw = lr * x * y * (y - theta)
    # Sliding threshold ~ running average of y^2
    theta = theta + (y**2 - theta) / tau_theta
    bcm_update.theta = theta
    return w + dw
```

---

## 7. Homeostatic Plasticity

- **Synaptic scaling**: global scaling to maintain target firing rate (Turrigiano)
- Slow (hours-days), multiplicative
- Prevents Hebbian runaway, keeps stability
- Complements fast Hebbian (fast learn + slow stabilize)

---

## 8. Bio-Plausible Learning

| Rule | Solves |
|---|---|
| Oja | Stability + PCA |
| BCM | Threshold metaplasticity |
| STDP | Temporal causality |
| Three-factor | Credit assignment / RL |
| Homeostatic | Global stability |
| Feedback alignment | Bio alternative to backprop |
| Predictive coding | Local error → approximates backprop |

---

## 9. Relation to Backprop

- Backprop not bio plausible: weight transport, global error, bidirectional
- Alternatives: feedback alignment, target prop, predictive coding, equilibrium prop
- These approximate gradients with **local** rules
- Key for neuromorphic / on-chip learning

---

## 10. Common Pitfalls

### 10.1 Hebbian Suffices

Pure Hebbian diverges; needs stabilization.

### 10.2 STDP Universal

STDP window form differs by synapse / region; not a single curve.

### 10.3 One Rule Explains All

Different synapses / tasks use different + combined rules.

### 10.4 Plasticity = LTP/LTD Only

Includes intrinsic, structural, homeostatic types.

### 10.5 Three-factor = Backprop

Still local + global scalar; not full gradient.

---

## 11. Related Concepts

- **Same section**: [Spiking Neural Networks](Spiking_Neural_Networks.en.md), [Hopfield Networks](Hopfield_Networks.en.md), [Reinforcement Learning Brain](Reinforcement_Learning_Brain.en.md)
- **Cellular**: [LTP/LTD](../02_Cellular_Molecular/LTP_LTD.en.md), [Synapse](../02_Cellular_Molecular/Synapse.en.md)
- **Foundation**: [Neuroplasticity](../00_Foundations/Neuroplasticity.en.md)
- **AI**: bio-plausible learning, neuromorphic

---

## References

1. **Bienenstock, E. L., Cooper, L. N., Munro, P. W.** "Theory for the development of neuron selectivity (BCM)." *J Neurosci*, 1982.
2. **Oja, E.** "A simplified neuron model as a principal component analyzer." *J Math Biol*, 1982.
3. **Frémaux, N. & Gerstner, W.** "Neuromodulated spike-timing-dependent plasticity and theory of three-factor learning rules." *Front Neural Circuits*, 2016.
4. **Turrigiano, G. G.** "The self-tuning neuron: synaptic scaling of excitatory synapses." *Cell*, 2008.
