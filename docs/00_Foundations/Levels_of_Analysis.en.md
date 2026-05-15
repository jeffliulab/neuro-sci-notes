# Marr's Levels of Analysis

> *David Marr (1982) proposed that understanding any information-processing system (brain or AI) requires 3 levels: computational (what & why), algorithmic (how — representation + process), implementational (physical substrate). This is the shared methodological framework of cognitive science + neuroscience + AI. Level confusion underlies many neuro-AI debates.*
>
> **Difficulty**: Beginner-Intermediate
> **Prerequisites**: none

---

## 1. Three Levels

| Level | Question | Example (vision) |
|---|---|---|
| **Computational** | What problem? Why? | Recover 3D structure from 2D image |
| **Algorithmic** | What representation + algorithm? | Edge detection → surface reconstruction |
| **Implementational** | How physically realized? | V1 simple cells, neurons |

---

## 2. Computational Level

- **What** the system computes + **why** (goals + constraints)
- Independent of implementation
- E.g.: an adder's goal is "do addition," whether abacus or CPU
- Marr stressed: this level most overlooked but most important

---

## 3. Algorithmic Level

- **Representation**: how is information encoded?
- **Algorithm**: how to go from input to output?
- One computation can have many algorithms
- E.g.: sorting via quicksort or mergesort

---

## 4. Implementational Level

- Physical substrate: neurons / silicon / gears
- One algorithm can have many implementations
- Much of neuroscience works here (electrophysiology, imaging)

---

## 5. Why Levels Matter

- Explanations at different levels are **complementary**, not competing
- "How a neuron fires" (impl) ≠ "what problem the brain solves" (comp)
- Cross-level confusion → futile debates
- Marr: studying only implementation can't reveal function ("understanding feathers ≠ understanding flight")

---

## 6. AI Correspondence

| Marr | AI |
|---|---|
| Computational | Task definition + loss function |
| Algorithmic | Model architecture + training algorithm |
| Implementational | GPU / TPU / hardware |

LLM: computational = next-token prediction; algorithmic = Transformer; impl = CUDA.

---

## 7. PyTorch — Three Levels Illustration

```python
import torch

# Computational: WHAT — minimize prediction error (the goal)
def loss_fn(pred, target):
    return ((pred - target) ** 2).mean()

# Algorithmic: HOW — gradient descent on a linear model (representation+process)
class LinearModel(torch.nn.Module):
    def __init__(self): 
        super().__init__()
        self.w = torch.nn.Linear(10, 1)
    def forward(self, x): return self.w(x)

# Implementational: physical — runs on CPU or CUDA
device = 'cuda' if torch.cuda.is_available() else 'cpu'
model = LinearModel().to(device)
```

---

## 8. History

- **Marr & Poggio 1976**: stereo vision, first used the framework
- **Marr 1982** *Vision*: full exposition (posthumous)
- Influenced all of cognitive science
- Later: Poggio added "learning" as 4th level (2012)

---

## 9. Modern Discussion

- **Connectomics**: criticized as "only implementation level"
- **Deep learning ↔ brain**: often conflates algorithmic with implementational
- **Normative models**: emphasize computational level (why designed this way)
- Krakauer 2017: neuroscience over-relies on implementation, lacks behavior (computational)

---

## 10. Common Pitfalls

### 10.1 Levels Have a Hierarchy

3 levels complementary, no superiority; different problems need different levels.

### 10.2 Implementation Determines All

Knowing every neuron ≠ understanding function (need computational).

### 10.3 Computational = Math

It's an abstract description of "goals + constraints," not just formulas.

### 10.4 One Algorithm One Implementation

Many-to-many mapping; same algorithm many hardware, same hardware many algorithms.

### 10.5 Marr Framework Obsolete

Still the core cross-disciplinary framework for neuro-AI communication.

---

## 11. Related Concepts

- **Same section**: [Research Methods](Research_Methods.en.md), [Neuroscience History](Neuroscience_History.en.md)
- **Computational**: [Predictive Coding](../05_Computational_Neuroscience/Predictive_Coding.en.md), [Bayesian Brain](../05_Computational_Neuroscience/Bayesian_Brain.en.md)
- **AI**: Task vs architecture vs hardware

---

## References

1. **Marr, D.** *Vision*. W.H. Freeman, 1982.
2. **Marr, D. & Poggio, T.** "From understanding computation to understanding neural circuitry." *AI Memo*, 1976.
3. **Poggio, T.** "The levels of understanding framework, revised." *Perception*, 2012.
4. **Krakauer, J. W. et al.** "Neuroscience needs behavior." *Neuron*, 2017.
