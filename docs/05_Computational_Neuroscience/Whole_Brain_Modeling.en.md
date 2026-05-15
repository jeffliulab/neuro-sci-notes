# Whole-Brain Modeling

> *Whole-brain modeling uses neural mass / mean-field models + connectome to simulate large-scale brain dynamics. The Virtual Brain (TVB), Blue Brain, Human Brain Project are representatives. Goal: link structural connectome with fMRI/EEG functional signals, model disease + personalization (virtual brain twin). Granularity tradeoff is the core debate.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Connectomics](../00_Foundations/Connectomics.en.md), [Neural Population Dynamics](Neural_Population_Dynamics.en.md)

---

## 1. Why Whole-Brain

- Cognition = distributed large-scale network interaction
- Single-region / single-neuron models can't explain global dynamics
- Link connectome (structure) → functional connectivity (fMRI)
- Disease = network-level disruption (connectopathy)

---

## 2. Modeling Levels

| Level | Unit | Example |
|---|---|---|
| Detailed | morphological neuron | Blue Brain |
| Spiking network | LIF/Izhikevich | large SNN |
| **Neural mass** | population mean | Wilson-Cowan, Jansen-Rit |
| **Mean-field** | probability distribution | DMF (dynamic mean field) |
| Whole-brain | region nodes + connectome | TVB |

---

## 3. Neural Mass Model

One population per brain region:
$$\tau \dot{r} = -r + S\!\left(\sum_j C_{ij} r_j + I\right)$$

- $C_{ij}$: from DTI connectome
- $S$: sigmoid activation
- Within-region simplified to mean activity
- Add delay (conduction) + noise

---

## 4. The Virtual Brain (TVB)

- Nodes = brain regions (AAL/Desikan parcellation)
- Edges = DTI tractography strength + conduction delay
- Each node a neural mass model
- Output → forward model → simulate fMRI BOLD / EEG
- Fit individual empirical FC

---

## 5. PyTorch — Simplified Whole-Brain Network

```python
import torch

def whole_brain_sim(SC, T=2000, dt=0.1, G=0.5, tau=10.0, noise=0.05):
    """SC: (N,N) structural connectome. Wilson-Cowan-like nodes."""
    N = SC.shape[0]
    r = torch.rand(N) * 0.1
    BOLD_proxy = []
    for t in range(T):
        coupling = G * (SC @ r)
        drive = -r + torch.sigmoid(coupling - 1.0)
        r = r + dt / tau * drive + noise * torch.randn(N) * (dt**0.5)
        r = r.clamp(0, 1)
        BOLD_proxy.append(r.clone())
    sim = torch.stack(BOLD_proxy)
    FC = torch.corrcoef(sim.T)            # simulated functional connectivity
    return sim, FC
```

→ Tune G so simulated FC best matches empirical FC.

---

## 6. Structure → Function

- Resting-state fMRI FC partially predicted by SC
- But SC≠FC (function can bypass direct structural connections)
- Working point: G tuned to criticality → best FC fit
- Explains resting-state networks (DMN etc.)

---

## 7. Clinical Applications

- **Epilepsy**: Epileptor model + individual connectome → predict seizure spread → surgery planning (EPINOV trial)
- **Stroke**: lesion → simulate network reorganization
- **Alzheimer / psychiatric**: connectopathy simulation
- **Personalization**: virtual brain twin (digital twin)

---

## 8. Blue Brain / HBP Debate

- Blue Brain: bottom-up detailed reconstruction (expensive, necessary?)
- Human Brain Project (2013-2023): ambition vs delivery criticism
- Lesson: granularity must match scientific question (see [Levels of Analysis](../00_Foundations/Levels_of_Analysis.en.md))
- Mean-field often more practical at whole-brain scale

---

## 9. Relation to AI

- Whole-brain model = large dynamical system (RNN-like at scale)
- Connectome-constrained RNN (use real connectome as W prior)
- Digital twin ↔ simulation-based inference
- But far from AGI; mechanistic model, not an agent

---

## 10. Common Pitfalls

### 10.1 More Detailed Better

Whole-brain detailed reconstruction parameter explosion; mean-field often more constrainable.

### 10.2 SC = FC

Functional connectivity ≠ structure; dynamics produce indirect FC.

### 10.3 Fitting FC = Understanding

High fit may be degenerate; need out-of-sample + perturbation validation.

### 10.4 Neural Mass = Real Population

Strong simplification (ignores spikes, heterogeneity).

### 10.5 Virtual Brain = Consciousness / AGI

It's a mechanistic simulation tool; no sentience.

---

## 11. Related Concepts

- **Same section**: [Neural Population Dynamics](Neural_Population_Dynamics.en.md), [Attractor Networks](Attractor_Networks.en.md)
- **Foundation**: [Connectomics](../00_Foundations/Connectomics.en.md), [Levels of Analysis](../00_Foundations/Levels_of_Analysis.en.md)
- **Frontiers**: [fMRI BOLD](../07_Neurotech_Frontiers/fMRI_BOLD.en.md)
- **Disease**: [Epilepsy](../08_Neuro_Disorders/Epilepsy.en.md)

---

## References

1. **Sanz Leon, P. et al.** "The Virtual Brain: a simulator of primate brain network dynamics." *Front Neuroinform*, 2013.
2. **Deco, G. et al.** "Resting-state functional connectivity emerges from structurally and dynamically shaped slow linear fluctuations." *J Neurosci*, 2013.
3. **Jirsa, V. K. et al.** "The Virtual Epileptic Patient." *NeuroImage*, 2017.
4. **Breakspear, M.** "Dynamic models of large-scale brain activity." *Nat Neurosci*, 2017.
