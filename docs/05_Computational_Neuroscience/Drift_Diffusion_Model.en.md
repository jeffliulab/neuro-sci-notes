# Drift-Diffusion Model

> *DDM (Ratcliff 1978) is the standard computational model of perceptual decision: evidence randomly accumulates to a boundary → decision. Simultaneously fits choice + reaction time distributions. Neural correspondence: LIP/FEF ramping activity (Shadlen & Newsome). Representative of the sequential sampling family, complementary to attractor and RL decision models.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Decision Making](../04_Cognitive_Neuroscience/Decision_Making.en.md), stochastic process basics

---

## 1. Model

Decision variable $x$ accumulates over time:
$$dx = v \, dt + \sigma \, dW$$

- $v$: drift rate (evidence strength)
- $\sigma dW$: Wiener noise
- Two boundaries: $+a$ (choose A), $0$ or $-a$ (choose B)
- Boundary hit → decision + time taken

---

## 2. Key Parameters

| Parameter | Meaning |
|---|---|
| $v$ (drift) | Evidence quality / difficulty |
| $a$ (boundary) | Caution (speed-accuracy) |
| $z$ (start) | Prior bias |
| $t_0$ (non-decision) | Encoding + motor delay |

---

## 3. Phenomena Explained

- **Choice probability**: $P(\text{correct}) = \frac{1}{1+e^{-2va/\sigma^2}}$
- **RT distribution**: right-skewed (long tail) — DDM naturally predicts
- **Speed-accuracy tradeoff**: adjust $a$
- **Error RT**: fast / slow errors

---

## 4. Neural Correspondence

- **LIP / FEF**: ramping firing rate ≈ accumulated evidence (Shadlen & Newsome 2001 random-dot motion)
- Hit threshold firing → saccade
- **SC (superior colliculus)**: triggers action
- Roitman & Shadlen 2002: firing trajectory ≈ DDM x(t)

---

## 5. Optimality via SPRT

- DDM = continuous version of Sequential Probability Ratio Test
- SPRT: fewest samples for given accuracy (Wald)
- → DDM is statistically optimal decision rule in 2AFC
- Brain approximates optimal evidence accumulation

---

## 6. PyTorch — DDM Simulation

```python
import torch

def simulate_ddm(v=0.3, a=1.0, z=0.5, sigma=1.0, dt=0.001, max_t=3.0):
    """Return choice (1/0) and RT."""
    x = z * a
    t = 0.0
    while 0 < x < a and t < max_t:
        x += v * dt + sigma * torch.randn(1).item() * (dt ** 0.5)
        t += dt
    choice = 1 if x >= a else 0
    return choice, t

# Aggregate → choice prob + RT distribution
```

---

## 7. Extended Models

- **LCA** (Leaky Competing Accumulator, Usher & McClelland): leak + mutual inhibition
- **Race model**: multiple accumulators racing
- **Collapsing bounds**: boundary decreases over time (urgency)
- **Attractor model** (Wong-Wang): biophysical implementation of DDM
- Multi-option: multi-hypothesis SPRT

---

## 8. Applications

- Perceptual decision (motion, contrast)
- Value decision (food choice — vmPFC)
- Memory retrieval (recognize / don't)
- Clinical: ADHD, aging, depression → drift rate ↓ / boundary changes
- Fitting tools: HDDM, PyDDM, EZ-diffusion

---

## 9. Relation to RL / Attractor

- DDM: abstract sequential sampling
- Wong-Wang attractor: biophysical mechanism implementing DDM-like
- RL: value determines drift; DDM determines how accumulation
- Three complementary, not competing

---

## 10. Common Pitfalls

### 10.1 DDM Only 2AFC

Extensions allow multi-option; but classic is binary.

### 10.2 Boundary Fixed

Collapsing bound / urgency often fits data better.

### 10.3 Ramping = Single-Neuron Accumulation

Population level; single-neuron stepping interpretation debated (Latimer 2015).

### 10.4 Parameters Uniquely Identifiable

Parameter trade-offs; need enough RT data + constraints.

### 10.5 DDM = Brain's Exact Mechanism

It's an algorithmic-level description; biophysical implementation is attractor.

---

## 11. Related Concepts

- **Same section**: [Attractor Networks](Attractor_Networks.en.md), [Neural Population Dynamics](Neural_Population_Dynamics.en.md), [Reinforcement Learning Brain](Reinforcement_Learning_Brain.en.md)
- **Cognition**: [Decision Making](../04_Cognitive_Neuroscience/Decision_Making.en.md), [Attention](../04_Cognitive_Neuroscience/Attention.en.md)
- **AI**: sequential decision, early-exit

---

## References

1. **Ratcliff, R.** "A theory of memory retrieval." *Psychol Rev*, 1978.
2. **Gold, J. I. & Shadlen, M. N.** "The neural basis of decision making." *Annu Rev Neurosci*, 2007.
3. **Roitman, J. D. & Shadlen, M. N.** "Response of neurons in the lateral intraparietal area during a combined visual discrimination reaction time task." *J Neurosci*, 2002.
4. **Bogacz, R. et al.** "The physics of optimal decision making." *Psychol Rev*, 2006.
