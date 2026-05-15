# Attractor Networks

> *Attractor networks form stable activity states (attractors) via recurrent connections. Point attractor (Hopfield memory), line/ring attractor (head direction, eye position), continuous attractor (grid/place cells). A unified computational framework for working memory, decision, spatial representation. Recurrent dynamics is core to brain computation, also inspiring RNNs.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Hopfield Networks](Hopfield_Networks.en.md), dynamical systems basics

---

## 1. Attractor Types

| Type | Geometry | Example |
|---|---|---|
| Point attractor | Discrete points | Hopfield memory, decision |
| Line attractor | 1D continuous line | Eye position integration (oculomotor) |
| Ring attractor | Ring | Head direction (HD cells) |
| Plane / torus | 2D | Grid cells, space |
| Limit cycle | Periodic orbit | CPG rhythm |

---

## 2. Dynamical Systems View

$$\tau \dot{\mathbf{r}} = -\mathbf{r} + f(W\mathbf{r} + \mathbf{I})$$

- Fixed point: $\dot{\mathbf{r}} = 0$
- Stable fixed point = attractor
- Energy landscape: attractor = valley bottom
- Basin of attraction

---

## 3. Point Attractor — Working Memory

- Recurrent excitation maintains persistent activity (held without input)
- DLPFC delay-period firing (see [Working Memory](../04_Cognitive_Neuroscience/Working_Memory.en.md))
- Multiple discrete attractors = multiple memory states
- Decision: two attractors compete (winner)

---

## 4. Ring Attractor — Head Direction

- Drosophila ellipsoid body "bump" (Seelig & Jayaraman 2015 directly observed!)
- Neurons arranged ring-wise by heading + local excitation + global inhibition
- Bump position = current heading
- Self-motion → bump moves (path integration)

---

## 5. Continuous Attractor — Grid/Place

- 2D continuous attractor maintains position representation
- Grid cells: torus attractor
- Place cells: bump on 2D sheet
- See [Grid Cells](Grid_Cells.en.md)

---

## 6. PyTorch — Ring Attractor

```python
import torch

def ring_attractor(N=100, steps=200, drift=0.0):
    """1D ring attractor: bump tracks heading."""
    theta = torch.linspace(0, 2*torch.pi, N)
    # Mexican-hat connectivity: local excite, global inhibit
    diff = theta.unsqueeze(0) - theta.unsqueeze(1)
    W = torch.exp(torch.cos(diff)) - 0.6
    r = torch.exp(torch.cos(theta - torch.pi))  # initial bump
    for _ in range(steps):
        r = torch.relu(W @ r / N + drift)
        r = r / (r.sum() + 1e-6) * N * 0.3      # normalization
    return theta, r  # stable bump = attractor state
```

---

## 7. Balance / Constraint

- Pure excitatory recurrent → explosion
- Need inhibition balance (E/I balance)
- Mexican-hat connectivity (near-excite, far-inhibit) → localized bump

---

## 8. Decision = Attractor Competition

- Wong & Wang 2006: two populations mutually inhibit → winner-take-all
- Evidence accumulation → cross separatrix → fall into one attractor
- Explains reaction time distribution, speed-accuracy tradeoff

---

## 9. Relation to RNN

- Trained RNNs often spontaneously form attractors / line attractors to solve tasks
- Sussillo & Barak 2013: reverse-engineer RNN → fixed point analysis
- Attractor is a shared computational primitive of RNN and brain
- Modern: dynamical systems view of neural computation (Vyas 2020)

---

## 10. Common Pitfalls

### 10.1 Attractor = Hopfield Only

Hopfield is just point attractor; also line/ring/continuous.

### 10.2 Persistent Activity = Only WM Mechanism

There's also activity-silent (short-term synaptic) WM theory.

### 10.3 Recurrent → Necessarily Unstable

E/I balance → stable attractors.

### 10.4 Attractors Static

Can move (ring bump tracks heading); continuous attractors dynamic.

### 10.5 Always Explicit Attractors

Some computation is transient dynamics (non-attractor); both coexist.

---

## 11. Related Concepts

- **Same section**: [Hopfield Networks](Hopfield_Networks.en.md), [Grid Cells](Grid_Cells.en.md), [Neural Population Dynamics](Neural_Population_Dynamics.en.md)
- **Cognition**: [Working Memory](../04_Cognitive_Neuroscience/Working_Memory.en.md), [Decision Making](../04_Cognitive_Neuroscience/Decision_Making.en.md)
- **Foundation**: [Neural Circuits](../00_Foundations/Neural_Circuits.en.md)

---

## References

1. **Amari, S.** "Dynamics of pattern formation in lateral-inhibition type neural fields." *Biol Cybern*, 1977.
2. **Seelig, J. D. & Jayaraman, V.** "Neural dynamics for landmark orientation and angular path integration." *Nature*, 2015.
3. **Wong, K.-F. & Wang, X.-J.** "A recurrent network mechanism of time integration in perceptual decisions." *J Neurosci*, 2006.
4. **Khona, M. & Fiete, I. R.** "Attractor and integrator networks in the brain." *Nat Rev Neurosci*, 2022.
