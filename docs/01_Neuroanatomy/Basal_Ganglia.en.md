# Basal Ganglia — Action Selection and Reinforcement Learning

> *Basal ganglia (BG) are nuclei beneath cortex: caudate, putamen, globus pallidus, substantia nigra, subthalamic nucleus. Responsible for **action selection**: cortex proposes actions → BG decides which to execute. Dopamine is the core modulator — Parkinson's DA degeneration causes movement difficulty.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Cortex](Cortex.en.md), [Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.en.md)

---

## 1. Main Nuclei

- **Striatum**: caudate + putamen — BG main input
- **Globus Pallidus** (internal/external, GPi/GPe): main output
- **Substantia Nigra**:
  - SNc: DA synthesis — projects to striatum
  - SNr: GPi-like output
- **Subthalamic Nucleus (STN)**: excitatory, affects GPi

---

## 2. Direct vs Indirect Pathway

Classic model:

```
Cortex → Striatum
            ↓
         (D1)            (D2)
            ↓               ↓
        Direct          Indirect
            ↓               ↓
          GPi/SNr     GPe → STN → GPi
            ↓                  ↓
        Thalamus (less inhibition → more cortex activity)
```

- **Direct (Go)**: via D1 → inhibit GPi → thalamus disinhibited → promote movement
- **Indirect (No-Go)**: via D2 → long chain → GPi more active → thalamus inhibited → suppress movement

DA balances Go vs No-Go via D1 (excite) and D2 (inhibit).

---

## 3. Relation to Reinforcement Learning

### 3.1 RPE (Reward Prediction Error)

Wolfram Schultz 1997: DA neurons encode reward prediction error.
- Expected reward → no DA change
- Unexpected reward → DA burst
- Expected reward missing → DA dip

→ DA = "temporal difference error" signal, matches RL theory.

### 3.2 Actor-Critic

- **Striatum** = critic (value)
- **DA** = TD error (training signal)
- **Cortex → BG → thalamus → cortex** = actor (action selection)

Direct correspondence to actor-critic RL framework.

---

## 4. Pathology

### 4.1 Parkinson's Disease

- SNc DA neuron degeneration (80%+ death before symptoms)
- Movement difficulty, tremor, rigidity
- Treatment:
  - L-DOPA (DA precursor)
  - DBS (Deep Brain Stimulation) on STN

### 4.2 Huntington's Disease

- Striatum medium spiny neuron degeneration
- Involuntary dance-like movements (chorea)
- Genetic (CAG repeat in HTT gene)

### 4.3 Tourette Syndrome

- BG abnormality → tics
- DA modulation dysregulation

### 4.4 OCD

- Cortico-striato-thalamic loop hyperactivation

---

## 5. Cognitive Roles

Recent findings: BG is not just motor:
- Habit formation — putamen
- Procedural memory
- Decision-making, social behavior
- Multiple parallel loops with cortex

---

## 6. PyTorch — Actor-Critic Analog

```python
import torch
import torch.nn as nn

class BasalGangliaActorCritic(nn.Module):
    def __init__(self, state_dim, action_dim):
        super().__init__()
        self.critic = nn.Linear(state_dim, 1)
        self.actor = nn.Linear(state_dim, action_dim)
        self.action_dim = action_dim
    
    def forward(self, state):
        value = self.critic(state)
        logits = self.actor(state)
        return torch.softmax(logits, -1), value
    
    def td_error(self, r, v_now, v_next, gamma=0.99):
        return r + gamma * v_next - v_now
```

---

## 7. History

- **1817** — Parkinson describes disease
- **1957** — Carlsson discovers dopamine
- **1980s** — Direct / Indirect pathway model (Albin, DeLong)
- **1997** — Schultz DA = RPE
- **2000s** — DBS for Parkinson clinical
- **2010s** — Cortico-BG closed loops multi-loop model
- **2020s** — Optogenetics validates direct/indirect functions

---

## 8. Common Pitfalls

### 8.1 Direct/Indirect Oversimplified

Binary oversimplified; modern reveals hybrid SPNs + complex modulation.

### 8.2 DA Beyond RPE

DA also encodes motivation, novelty, salience.

### 8.3 BG Single Function

Multiple parallel loops — motor / cognitive / limbic.

### 8.4 Parkinson Beyond SNc

Also involves cortex, autonomic system, other nuclei.

### 8.5 Striatum Main Cells

Medium spiny neurons (MSN) 95%, rest are interneurons.

---

## 9. Related Concepts

- **Same section**: [Cortex](Cortex.en.md), [Cerebellum](Cerebellum.en.md), [Hippocampus](Hippocampus_Anatomy.en.md)
- **Neurotransmitters**: [Dopamine](../02_Cellular_Molecular/Neurotransmitters.en.md)
- **AI comparison**: Actor-Critic RL — https://jeffliulab.github.io/ai-notes/04_Reinforcement_Learning/

---

## References

1. **Schultz, W.** "A neural substrate of prediction and reward." *Science*, 1997.
2. **Albin, R. L. et al.** "The functional anatomy of basal ganglia disorders." *Trends Neurosci*, 1989.
3. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
4. **Bromberg-Martin, E. S. et al.** "Dopamine in motivational control." *Neuron*, 2010.
