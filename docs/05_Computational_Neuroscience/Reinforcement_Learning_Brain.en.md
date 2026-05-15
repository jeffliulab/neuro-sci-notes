# Reinforcement Learning in the Brain

> *The correspondence between RL theory and the dopamine system is one of computational neuroscience's greatest success stories. Schultz 1997: VTA/SNc dopamine neurons encode reward prediction error (RPE), exactly TD learning's δ. Basal ganglia implement actor-critic. Sutton-Barto RL ↔ brain bidirectional inspiration, spawning deep RL.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Reward System](../03_Systems_Neuroscience/Reward_System.en.md), [Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.en.md), MDP basics

---

## 1. RL Framework

- Agent ↔ environment: state $s$, action $a$, reward $r$
- Goal: maximize cumulative reward $\sum \gamma^t r_t$
- Value function $V(s)$, $Q(s,a)$
- TD error: $\delta_t = r_t + \gamma V(s_{t+1}) - V(s_t)$

---

## 2. Dopamine = RPE (Schultz 1997)

Classic experiment (monkey + juice reward):
- **Not yet learned**: reward arrives → DA burst
- **After learning**: cue arrives → DA burst, reward arrives → no response (predicted)
- **Reward omitted**: at expected time → DA dip (negative RPE)

→ Perfectly matches TD error $\delta$.

---

## 3. TD Learning

$$V(s_t) \leftarrow V(s_t) + \alpha \, \delta_t$$
$$\delta_t = r_t + \gamma V(s_{t+1}) - V(s_t)$$

- DA concentration ∝ $\delta_t$
- DA modulates cortico-striatal synaptic plasticity (three-factor rule)

---

## 4. Basal Ganglia = Actor-Critic

- **Critic**: ventral striatum (NAcc) estimates $V(s)$ → generates RPE
- **Actor**: dorsal striatum selects action
- DA RPE trains both critic + actor
- Direct/indirect pathways ↔ Go/NoGo (see [Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.en.md))

---

## 5. Model-free vs Model-based

| | Model-free | Model-based |
|---|---|---|
| Mechanism | TD / habit | Planning / tree search |
| Brain | Dorsolateral striatum | PFC + dorsomedial striatum |
| Trait | Fast, rigid | Slow, flexible |
| Behavior | Habit | Goal-directed |

Human brain has both (Daw 2011 two-step task).

---

## 6. PyTorch — TD Learning (DA model)

```python
import numpy as np

def td_learning(rewards, n_states, alpha=0.1, gamma=0.9, episodes=100):
    V = np.zeros(n_states)
    da_signal = []
    for _ in range(episodes):
        for s in range(n_states - 1):
            r = rewards[s]
            delta = r + gamma * V[s+1] - V[s]   # = dopamine RPE
            V[s] += alpha * delta
            da_signal.append(delta)
    return V, da_signal
```

---

## 7. Beyond Classic RPE

- **Distributional RL** (Dabney 2020 *Nature*): DA neurons heterogeneous → encode reward distribution (not just mean)!
  - Mutually validates with deep RL distributional (C51, QR-DQN)
- **Tonic DA**: average reward rate / vigor
- **Risk / uncertainty**: DA also encodes

---

## 8. Serotonin + Others

- **5-HT**: possibly encodes punishment / temporal discounting / patience
- **ACh**: uncertainty / learning rate
- **NE**: explore-exploit, surprise
- RL not only DA — multiple neuromodulators cooperate

---

## 9. AI ↔ Brain Bidirectional

| RL concept | Brain correspondence |
|---|---|
| TD error | DA RPE |
| Actor-critic | BG dorsal/ventral |
| Distributional RL | DA neuron heterogeneity |
| Eligibility trace | Synaptic tag |
| Exploration | NE / tonic DA |
| Replay (DQN) | Hippocampal replay |

Deep RL (DQN, AlphaGo) directly inspired by brain RL, feeds back to neuroscience.

---

## 10. Common Pitfalls

### 10.1 DA = Pleasure

DA is RPE / wanting, not hedonic pleasure (liking is opioid).

### 10.2 DA Only Encodes Mean Reward

Distributional RL: encodes entire distribution.

### 10.3 Brain Only Model-free

Model-based (PFC planning) coexists.

### 10.4 RPE Explains All Learning

Only reward learning; supervised / unsupervised learning have other mechanisms.

### 10.5 One δ for Whole Brain

RPE is region-specific (different striatum subregions differ).

---

## 11. Related Concepts

- **Same section**: [Bayesian Brain](Bayesian_Brain.en.md), [Predictive Coding](Predictive_Coding.en.md)
- **Systems**: [Reward System](../03_Systems_Neuroscience/Reward_System.en.md)
- **Anatomy**: [Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.en.md)
- **AI**: RL — https://jeffliulab.github.io/ai-notes/04_Reinforcement_Learning/

---

## References

1. **Schultz, W., Dayan, P., Montague, P. R.** "A neural substrate of prediction and reward." *Science*, 1997.
2. **Sutton, R. S. & Barto, A. G.** *Reinforcement Learning: An Introduction*. 2nd ed., 2018.
3. **Dabney, W. et al.** "A distributional code for value in dopamine-based reinforcement learning." *Nature*, 2020.
4. **Daw, N. D. et al.** "Model-based influences on humans' choices and striatal prediction errors." *Neuron*, 2011.
