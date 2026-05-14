# Decision-Making — Neuroscience Perspective

> *Decision-making is the brain's process of integrating value + risk + selecting action. Involves OFC, vmPFC, insula, ACC, striatum across regions. Drift-diffusion model (DDM) is the classic math framework. Tightly tied to RL.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Reward System](../03_Systems_Neuroscience/Reward_System.en.md), [Cortex](../01_Neuroanatomy/Cortex.en.md)

---

## 1. Decision Types

- **Perceptual**: "left vs right" visual dots
- **Value-based**: A vs B better
- **Reward-based**: choose high reward (RL)
- **Social**: trust / fairness
- **Strategic**: long-horizon planning

---

## 2. Neural Basis

```
Sensory input → cortex
                   ↓
            OFC / vmPFC (value computation)
                   ↓
            Insula (risk)
                   ↓
            ACC (conflict monitoring)
                   ↓
            Striatum (action selection)
                   ↓
            M1 (motor execution)
```

---

## 3. Drift-Diffusion Model (DDM)

Classic perceptual decision:

$$\frac{dx}{dt} = v + \sigma \eta(t)$$

- $v$: drift rate (evidence accumulation)
- $\sigma$: noise
- Upper boundary → choose A
- Lower boundary → choose B

Predicts RT distribution + accuracy.

---

## 4. OFC / vmPFC

- OFC: computes value (food / visual stimuli)
- vmPFC: compares options (A vs B)
- Damage → decision-making abnormalities (Phineas Gage)

---

## 5. ACC (Anterior Cingulate)

- Conflict monitoring (highly active in Stroop test)
- Error detection
- Effort estimation

---

## 6. Insula

- Negative emotion / loss
- Disgust
- Risk aversion

---

## 7. Decision Biases (Behavioral Economics)

- **Loss aversion**: losses hurt > gains satisfy
- **Sunk cost**: paid cost shouldn't affect future decisions
- **Confirmation bias**: only see supportive info
- **Anchoring**: first number influences judgment

→ Kahneman / Tversky 2002 Nobel Economics.

---

## 8. PyTorch — DDM Simulation

```python
import torch

class DDM:
    def __init__(self, drift=0.3, noise=1.0, threshold=2.0):
        self.drift, self.noise, self.threshold = drift, noise, threshold
    
    def simulate(self, max_steps=10000, dt=0.01):
        x = 0
        for t in range(max_steps):
            dx = self.drift * dt + self.noise * torch.randn(1).item() * (dt ** 0.5)
            x += dx
            if x >= self.threshold:
                return 'A', t * dt
            if x <= -self.threshold:
                return 'B', t * dt
        return 'undecided', max_steps * dt

ddm = DDM()
choice, rt = ddm.simulate()
```

---

## 9. Pathology

- **Frontal lobe damage** (Phineas Gage): personality + decision abnormalities
- **OFC damage**: Iowa Gambling Task failure (Bechara 1994)
- **Addiction**: short-term reward > long-term cost
- **Depression**: indecision + pessimistic bias
- **Pathological gambling**: insula abnormalities

---

## 10. AI Connections

- LLM decision-making: tool use, function calling
- RL with reward shaping
- Inverse RL: reconstructs reward function from behavior

---

## 11. Common Pitfalls

### 11.1 DDM Simplified

Only 1-D evidence; real decisions are multi-dimensional.

### 11.2 vmPFC ≠ "Decision Center"

Distributed network.

### 11.3 Behavioral Biases Aren't Bugs

Many biases are evolved heuristics, rational in some contexts.

### 11.4 RT ≠ Confidence

Short RT doesn't necessarily mean high confidence.

### 11.5 fMRI ≠ Causal

fMRI only correlates; lesion / TMS proves causality.

---

## 12. Related Concepts

- **Same section**: [Language](Language.en.md), [Consciousness](Consciousness.en.md)
- **Systems**: [Reward System](../03_Systems_Neuroscience/Reward_System.en.md)
- **AI**: [RL](https://jeffliulab.github.io/ai-notes/04_Reinforcement_Learning/)

---

## References

1. **Ratcliff, R.** "A theory of memory retrieval (DDM)." *Psychol Rev*, 1978.
2. **Bechara, A. et al.** "Insensitivity to future consequences (Iowa Gambling Task)." *Cognition*, 1994.
3. **Kahneman, D.** *Thinking, Fast and Slow*. 2011.
4. **Glimcher, P. W. & Fehr, E.** *Neuroeconomics*. 2nd ed., 2014.
