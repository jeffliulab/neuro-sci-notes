# Action Potential — The Neuron's Digital Signal

> *The action potential is the core communication unit of neurons: a 1ms all-or-none voltage spike propagating along the axon. This article covers ion mechanisms (Na+/K+) → Hodgkin-Huxley equations → myelinated conduction, fully introducing spike generation and propagation. The "hello world" of neuroscience.*
>
> **Difficulty**: Intermediate

---

## 1. Action Potential Waveform

Classic waveform:
- **Rest**: -70 mV
- **Threshold**: -55 mV
- **Peak**: +30 mV
- **Repolarization**
- **Hyperpolarization**: -80 mV (brief)
- **Return to rest**

Total duration ~ 1 ms.

```
+30 |          /\
    |         /  \
 -55|________/____\____________
    |       (threshold)
 -70|_______________\___________
    |                \________
 -80|________________________(undershoot)
```

---

## 2. Ion Mechanisms

### 2.1 Rest

- K+ leak channels open → close to K+ equilibrium (-90 mV)
- Na+/K+ pump maintains gradient

### 2.2 Phase 1: Depolarization (Rising)

Threshold (-55 mV) triggers voltage-gated Na+ channels open:
- Large Na+ inflow
- Further depolarizes → more Na+ channels open
- **Positive feedback**, ~0.5 ms reaches +30 mV

### 2.3 Phase 2: Peak

- Na+ channels auto-**inactivate** (inactivation gate closes)
- Voltage-gated K+ channels open (slow start)
- K+ outflow begins dominating

### 2.4 Phase 3: Repolarization

- Na+ channels still inactivated
- K+ outflow dominates, voltage drops

### 2.5 Phase 4: Hyperpolarization

- K+ channels close slowly
- Briefly exceeds -80 mV (K+ equilibrium)
- Absolute / relative refractory period — can't / hardly re-fire

### 2.6 Return to Rest

- Na+/K+ pump restores gradients
- Na+ inactivation resets (can re-fire)

---

## 3. Hodgkin-Huxley Equations

1952 Hodgkin & Huxley mathematized in squid axon:

$$C \frac{dV}{dt} = -g_{Na} m^3 h (V - E_{Na}) - g_K n^4 (V - E_K) - g_L (V - E_L) + I_{\text{ext}}$$

where:
- $V$ = membrane potential
- $C$ = membrane capacitance (~ 1 μF/cm²)
- $g_{Na}, g_K, g_L$ = maximum conductances
- $m, h, n$ = gating variables, each with own ODE
- $E_{Na}, E_K, E_L$ = equilibrium potentials
- $I_{\text{ext}}$ = external current

### 3.1 Gating Dynamics

$$\frac{dm}{dt} = \alpha_m(V)(1-m) - \beta_m(V) m$$

(where $\alpha, \beta$ are V-functions). Same form for $h, n$.

→ 5 ODEs fully describe the spike. Nobel 1963.

---

## 4. Threshold and All-or-None

Threshold isn't a fixed voltage but a tipping point of Na+ channel kinetics.
Below threshold → no spike (but sub-threshold integration).
Above → spike size / shape nearly identical (info encoded in **timing + rate**, not amplitude).

---

## 5. Propagation

### 5.1 Unmyelinated Axon

- Spike propagates wave-like along axon
- Speed: 0.5 - 2 m/s
- Slow, suited for short distances

### 5.2 Myelinated Axon

- Myelin is insulating layer, skips intermediate regions
- Spike jumps between **Ranvier nodes** (saltatory conduction)
- Speed: 10 - 100 m/s
- Larger diameter axons faster (e.g. spinal motor 30 m/s)

### 5.3 Demyelinating Diseases (MS)

- Multiple Sclerosis: autoimmune attack on myelin
- Slow / faulty signals → neurological symptoms

---

## 6. PyTorch — Numerical Integration of HH

```python
import torch

def hh_step(V, m, h, n, I_ext, dt=0.01):
    alpha_m = 0.1 * (V + 40) / (1 - torch.exp(-(V + 40) / 10))
    beta_m = 4 * torch.exp(-(V + 65) / 18)
    alpha_h = 0.07 * torch.exp(-(V + 65) / 20)
    beta_h = 1 / (1 + torch.exp(-(V + 35) / 10))
    alpha_n = 0.01 * (V + 55) / (1 - torch.exp(-(V + 55) / 10))
    beta_n = 0.125 * torch.exp(-(V + 65) / 80)
    
    m = m + dt * (alpha_m * (1 - m) - beta_m * m)
    h = h + dt * (alpha_h * (1 - h) - beta_h * h)
    n = n + dt * (alpha_n * (1 - n) - beta_n * n)
    
    g_Na, g_K, g_L = 120, 36, 0.3
    E_Na, E_K, E_L = 50, -77, -54.4
    I_Na = g_Na * m**3 * h * (V - E_Na)
    I_K = g_K * n**4 * (V - E_K)
    I_L = g_L * (V - E_L)
    
    dV = -I_Na - I_K - I_L + I_ext
    V = V + dt * dV
    return V, m, h, n

V, m, h, n = -65, 0.05, 0.6, 0.32
trace = []
for t in range(10000):
    I = 10 if 2000 < t < 5000 else 0
    V, m, h, n = hh_step(V, m, h, n, I)
    trace.append(V)
```

---

## 7. Information Encoding

### 7.1 Rate Coding

Spike frequency (~1-1000 Hz) encodes information.
- Sensory neurons: stronger stimulus → higher rate
- Motor neurons: higher rate → stronger muscle contraction

### 7.2 Temporal Coding

Precise spike timing (1 ms level) more informative:
- Auditory: phase locking for sound localization
- Olfactory: spike patterns encode odors

### 7.3 Population Coding

Multiple neurons coordinate; e.g. motor cortex uses population vector to encode movement direction.

---

## 8. Classic Experiment — Squid Axon

Hodgkin & Huxley 1939 chose squid:
- Giant axon diameter 0.5-1mm, can insert glass electrode
- Voltage clamp: fix V, measure ion current
- Inferred ion conductance V-dependence

→ One of the most elegant neuroscience experiments.

---

## 9. Simplified vs Real Spike

Real cortical neuron spikes:
- Shape has variation
- AP followed by afterdepolarization / afterhyperpolarization
- Burst firing (PFC L5b cells)
- Calcium spikes (long, 100ms+)

LIF is too simplified; HH is good approximation; real neurons more complex.

---

## 10. Common Pitfalls

### 10.1 Threshold Not Fixed

Threshold varies with stimulus history / channel state.

### 10.2 Spike ≠ Binary

Although all-or-none, real spikes have width / amplitude variation.

### 10.3 One Channel Type Insufficient

Real neurons have 10+ channel types (Ca, multiple Na subtypes, leak K, etc.).

### 10.4 Refractory Period Not Absolute

Strong stimulus can fire during relative refractory but amplitude reduced.

### 10.5 Dendrites Also Spike

Dendritic Na/Ca spikes influence integration, not just cable propagation.

---

## 11. Related Concepts

- **History**: [Neuroscience History](../00_Foundations/Neuroscience_History.en.md)

---

## References

1. **Hodgkin, A. L. & Huxley, A. F.** "A quantitative description of membrane current and its application to conduction and excitation in nerve." *J Physiol*, 1952.
2. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
3. **Bear, M. F. et al.** *Neuroscience: Exploring the Brain*. 4th ed., 2015.
4. **Koch, C.** *Biophysics of Computation*. Oxford, 1999.
5. **Trappenberg, T.** *Fundamentals of Computational Neuroscience*. 2nd ed., 2010.
