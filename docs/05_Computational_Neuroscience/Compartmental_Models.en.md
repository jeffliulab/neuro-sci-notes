# Compartmental Models

> *Compartmental modeling divides a neuron into multiple isopotential "compartments," each described by the cable equation + Hodgkin-Huxley channels, with electrical coupling between compartments. The standard for biophysically detailed simulation (NEURON, Arbor). From single-compartment LIF to morphologically detailed Blue Brain reconstruction. Bridges [Dendrites](../02_Cellular_Molecular/Dendrites.en.md) with computation.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Hodgkin Huxley](../02_Cellular_Molecular/Hodgkin_Huxley.en.md), [Dendrites](../02_Cellular_Molecular/Dendrites.en.md), differential equations

---

## 1. Basic Idea

- Neuron morphology complex → discretize into N isopotential compartments
- Each compartment: membrane capacitance + ion channels + leak
- Adjacent compartments: axial resistance coupling
- Solve large ODE system

---

## 2. Cable Equation

$$\frac{\partial V}{\partial t} = \frac{a}{2 R_i C_m} \frac{\partial^2 V}{\partial x^2} - \frac{V}{R_m C_m} + \frac{I_{\text{ion}}}{C_m}$$

- $\lambda = \sqrt{a R_m / 2 R_i}$: length constant
- $\tau = R_m C_m$: time constant
- Discretize → compartment equations

---

## 3. Single → Multi-Compartment

| Model | Compartments | Use |
|---|---|---|
| LIF | 1 (point) | Network simulation (fast) |
| Izhikevich | 1 | Rich dynamics + fast |
| Hodgkin-Huxley | 1 | Single-cell AP |
| 2-3 compartment | soma+dendrite | Simplified dendritic computation |
| Morphological | hundreds-thousands | Blue Brain level realism |

---

## 4. Compartment Equation

For each compartment i:
$$C_m \frac{dV_i}{dt} = -I_{\text{ion},i} + \sum_{j \in \text{neighbors}} \frac{V_j - V_i}{R_{ij}} + I_{\text{ext}}$$

- Ion current = HH type (Na, K, Ca, …)
- Coupling term = inter-compartment current

---

## 5. PyTorch — 3-Compartment Simulation

```python
import torch

def three_compartment(T=1000, dt=0.025, I_dend=0.0, I_soma=2.0):
    """soma - prox dendrite - dist dendrite, coupled."""
    V = torch.tensor([-65.0, -65.0, -65.0])  # soma, prox, dist
    g_couple = torch.tensor([0.5, 0.3])       # soma-prox, prox-dist
    trace = []
    for t in range(T):
        leak = -0.3 * (V + 65.0)
        coup = torch.zeros(3)
        coup[0] = g_couple[0] * (V[1] - V[0])
        coup[1] = g_couple[0]*(V[0]-V[1]) + g_couple[1]*(V[2]-V[1])
        coup[2] = g_couple[1] * (V[1] - V[2])
        I = torch.tensor([I_soma, 0.0, I_dend])
        V = V + dt * (leak + coup + I)
        if V[0] > -50:                # simple soma spike + reset
            V[0] = -65.0
        trace.append(V.clone())
    return torch.stack(trace)
```

---

## 6. Simulation Tools

| Tool | Feature |
|---|---|
| **NEURON** | Classic, Hines method, standard |
| **Arbor** | Modern, GPU/HPC, multicore |
| **Brian2** | Python, easy to use |
| **NEST** | Large network point-neuron |
| **GENESIS** | Veteran |
| **CoreNEURON** | NEURON GPU acceleration |

---

## 7. Numerical Methods

- **Hines method**: exploits tree topology → O(N) tridiagonal solve (not O(N³))
- Implicit (Crank-Nicolson) for stability
- Adaptive step size (stiff equations)
- Large networks: parallel + domain decomposition

---

## 8. Blue Brain / Detailed Reconstruction

- Markram Blue Brain: morphologically + biophysically detailed cortical column
- Each neuron hundreds-thousands of compartments + multiple channels
- Debate: are details necessary? vs simplified models
- Lesson: model granularity depends on the question (see [Levels of Analysis](../00_Foundations/Levels_of_Analysis.en.md))

---

## 9. Simplification vs Detail Tradeoff

| | Simple (LIF) | Detailed (multi-comp) |
|---|---|---|
| Speed | Fast (millions of neurons) | Slow |
| Explanatory power | Network level | Single-cell mechanism |
| Parameters | Few | Many (hard to constrain) |
| Use | Large-scale networks | Dendritic computation, pharmacology |

---

## 10. Common Pitfalls

### 10.1 More Detailed = More Correct

Too many parameters → hard to constrain + overfitting; granularity depends on question.

### 10.2 Point Neuron Sufficient

Ignores dendritic computation (see [Dendrites](../02_Cellular_Molecular/Dendrites.en.md)).

### 10.3 Explicit Integration OK

HH is stiff → need implicit / small step, else diverges.

### 10.4 More Compartments More Accurate

Must match length constant; too coarse distorts, too fine wastes.

### 10.5 Simulation = Reality

Parameters + channel distribution uncertain; must constrain + validate experimentally.

---

## 11. Related Concepts

- **Same section**: [Spiking Neural Networks](Spiking_Neural_Networks.en.md), [Reservoir Computing](Reservoir_Computing.en.md)
- **Cellular**: [Hodgkin Huxley](../02_Cellular_Molecular/Hodgkin_Huxley.en.md), [Dendrites](../02_Cellular_Molecular/Dendrites.en.md), [Ion Channels](../02_Cellular_Molecular/Ion_Channels.en.md)
- **Foundation**: [Levels of Analysis](../00_Foundations/Levels_of_Analysis.en.md)

---

## References

1. **Rall, W.** "Cable theory for dendritic neurons." In *Methods in Neuronal Modeling*, 1989.
2. **Hines, M. L. & Carnevale, N. T.** "The NEURON simulation environment." *Neural Comput*, 1997.
3. **Markram, H. et al.** "Reconstruction and simulation of neocortical microcircuitry." *Cell*, 2015.
4. **Koch, C.** *Biophysics of Computation*. Oxford, 1999.
