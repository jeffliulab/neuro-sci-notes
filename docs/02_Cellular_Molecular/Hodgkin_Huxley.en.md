# Hodgkin-Huxley Equations — Biophysical Neuron Model

> *In 1952, Alan Hodgkin and Andrew Huxley derived **precise mathematical equations** describing the action potential on squid giant axon — 4 ODEs fully capture Na+/K+ channel dynamics. Still the gold standard of computational neuroscience. Nobel 1963.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Action Potential](Action_Potential.en.md), [Neuron](Neuron.en.md), differential equations

---

## 1. Equation Overview

Membrane voltage V evolves:

$$C \frac{dV}{dt} = I_{\text{ext}} - g_{Na} m^3 h (V - E_{Na}) - g_K n^4 (V - E_K) - g_L (V - E_L)$$

3 gating variables each satisfy:

$$\frac{dm}{dt} = \alpha_m(V)(1-m) - \beta_m(V) m$$

(similar for h, n)

---

## 2. Physical Meaning

- $g_{Na} m^3 h$: Na+ channel open probability
  - $m^3$: 3 activation gates
  - $h$: 1 inactivation gate
- $g_K n^4$: K+ channel — 4 n gates
- Nernst: $E_{Na} = +50$ mV, $E_K = -77$ mV

---

## 3. Dynamics

Time constant + steady-state:

$$\tau_X = \frac{1}{\alpha_X + \beta_X}, \quad X_\infty = \frac{\alpha_X}{\alpha_X + \beta_X}$$

- $\tau_m$ very fast (< 1 ms)
- $\tau_h, \tau_n$ slow (~ ms)

Spike phases:
1. Input I → V rises
2. $V \uparrow \to m_\infty \uparrow$ → Na+ inflow
3. Positive feedback
4. h slowly inactivates Na+
5. n opens K+, K+ outflow
6. V drops → repolarization

---

## 4. PyTorch Numerical Integration

```python
import torch

def hh_dynamics(V, m, h, n, I_ext, dt=0.01):
    am = 0.1*(V+40) / (1 - torch.exp(-(V+40)/10))
    bm = 4*torch.exp(-(V+65)/18)
    ah = 0.07*torch.exp(-(V+65)/20)
    bh = 1/(1 + torch.exp(-(V+35)/10))
    an = 0.01*(V+55) / (1 - torch.exp(-(V+55)/10))
    bn = 0.125*torch.exp(-(V+65)/80)
    
    I_Na = 120 * m**3 * h * (V - 50)
    I_K = 36 * n**4 * (V - (-77))
    I_L = 0.3 * (V - (-54.4))
    
    dV = (I_ext - I_Na - I_K - I_L) / 1.0
    V_new = V + dt * dV
    m_new = m + dt * (am*(1-m) - bm*m)
    h_new = h + dt * (ah*(1-h) - bh*h)
    n_new = n + dt * (an*(1-n) - bn*n)
    return V_new, m_new, h_new, n_new

V, m, h, n = -65.0, 0.05, 0.6, 0.32
trace = []
for t in range(20000):
    I = 10 if 5000 < t < 6000 else 0
    V, m, h, n = hh_dynamics(V, m, h, n, I)
    trace.append(V)
```

---

## 5. Simplifications

- **FitzHugh-Nagumo (2D)**: HH simplified to 2D phase plane
- **Hindmarsh-Rose**: 3D, supports bursting
- **Leaky Integrate-and-Fire (LIF)**: simplest, no ion mechanism

---

## 6. Applications

- Accurate single-neuron simulation
- Blue Brain / Human Brain Project
- Pharmacology — drug effects on specific channels
- Pathology — epilepsy / arrhythmia channel mutations

---

## 7. Limits

- Only Na/K/leak; real neurons have Ca, M-type K, HCN etc.
- Single compartment (axon homogeneous); real neurons have dendrites + soma
- Computationally slow (large networks use LIF)

---

## 8. History

- **1939** — Hodgkin & Huxley squid axon voltage clamp
- **1949** — Inferred Na+/K+ mechanism
- **1952** — Complete equations + paper
- **1963** — Nobel Prize
- **1970s+** — Extended to other channels (Hille)

---

## 9. Common Pitfalls

### 9.1 dt Selection

Too large dt → numerical divergence. HH needs dt ≤ 0.01 ms.

### 9.2 Initial m, h, n

Should be steady-state values (else transient error).

### 9.3 Unit Consistency

mV, mS/cm², μF/cm² — mixing causes bugs.

### 9.4 Real-time Simulation Expensive

10k+ neuron HH needs GPU for real-time.

### 9.5 Limited Behavior Coverage

HH only describes axon spike; synaptic / network dynamics need extension.

---

## 10. Related Concepts

- **Same section**: [Action Potential](Action_Potential.en.md), [Neuron](Neuron.en.md)
- **AI comparison**: https://jeffliulab.github.io/ai-notes/01_AI/03_Frontiers/03_World_Models/RSSM_PlaNet/

---

## References

1. **Hodgkin, A. L. & Huxley, A. F.** *J Physiol*, 1952.
2. **Koch, C.** *Biophysics of Computation*. Oxford, 1999.
3. **Trappenberg, T.** *Fundamentals of Computational Neuroscience*. 2010.
4. **Izhikevich, E. M.** *Dynamical Systems in Neuroscience*. MIT, 2007.
