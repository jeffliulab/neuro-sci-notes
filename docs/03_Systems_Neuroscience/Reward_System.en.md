# Reward System — VTA / NAcc / DA / RL Neural Basis

> *Brain reward system consists of VTA → NAcc projection + multiple cortical modulations. Core NT is dopamine. Schultz 1997 RPE discovery directly mapped DA to reinforcement learning. Central to motivation, addiction, decision.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.en.md), [Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.en.md)

---

## 1. Core Circuit

```
VTA (Ventral Tegmental Area) ─DA→ NAcc (Nucleus Accumbens) → behavior
  │                            │
  └─DA→ PFC (planning) ──────────┐
                                  ↓
                              Amygdala (emotion)
                              Hippocampus (memory)
                                  ↓
                       OFC (Orbitofrontal) integrates value
```

---

## 2. DA Neurons in VTA

- ~25k DA neurons in human VTA
- Wide projection: NAcc / PFC / amygdala / hippocampus
- Tonic firing ~5 Hz
- Phasic burst on reward

---

## 3. RPE (Reward Prediction Error)

Schultz 1997: VTA DA neurons encode:

$$\delta = r + \gamma V(s') - V(s)$$

- Expected reward → no DA change
- Unexpected reward → DA burst
- Expected reward missing → DA dip

→ Mathematically consistent with TD-learning.

---

## 4. vs Actor-Critic RL

| Brain | RL |
|---|---|
| Striatum (medium spiny neurons) | Critic + Actor |
| VTA DA | TD error δ |
| PFC | Policy / value high-level |
| Hippocampus | State representation |
| Amygdala | Emotional value |

---

## 5. NAcc (Nucleus Accumbens)

- Part of ventral striatum
- "Pleasure center"
- DA → motivation / wanting
- Opioids → liking
- Hub of addiction

---

## 6. DA Beyond Reward

- **Salience**: important events (good / bad)
- **Novelty**: new stimuli
- **Effort**: cost estimation
- **Risk**: uncertainty
- **Motor**: SNc → BG → movement

---

## 7. Addiction Mechanism

```
Drug → directly stimulates DA release (cocaine blocks reuptake; opioid → VTA)
     → NAcc very high DA
     → Learn strong association: drug = max reward
     → Compulsive seeking
     → Long-term: DA receptor down-regulation → tolerance
```

→ Reward hijacked.

---

## 8. Affect and Choice

- **OFC (Orbitofrontal cortex)**: computes value
- **vmPFC**: between-option comparison
- **Anterior insula**: risk / loss aversion
- **dlPFC**: inhibits impulsive choice

→ Decision = multi-region competition / integration.

---

## 9. PyTorch — TD Learning ↔ DA

```python
import torch

class DASignal:
    def __init__(self, gamma=0.95, lr=0.1):
        self.values = {}
        self.gamma = gamma
        self.lr = lr
    
    def step(self, state, reward, next_state):
        v_s = self.values.get(state, 0)
        v_next = self.values.get(next_state, 0)
        delta = reward + self.gamma * v_next - v_s
        self.values[state] = v_s + self.lr * delta
        return delta
```

---

## 10. Pathology

- **Addiction**: cocaine, opioid, alcohol hijack DA
- **Depression**: reduced reward sensitivity (anhedonia)
- **Schizophrenia (positive)**: DA excess (hypothesis)
- **Parkinson**: SNc DA degeneration → motor + apathy
- **ADHD**: DA / NE dysregulation

---

## 11. Treatments / Drugs

- **SSRI**: indirectly affects reward
- **Bupropion**: DA + NE reuptake inhibitor
- **Naltrexone**: opioid antagonist (addiction)
- **L-DOPA**: Parkinson
- **DBS NAcc**: experimental for severe OCD / depression

---

## 12. Common Pitfalls

### 12.1 "DA = Pleasure" Oversimplification

DA primarily motivation / wanting, not pleasure itself (opioid gives liking).

### 12.2 RPE Not Unique

DA has multiple dimensions (salience, novelty).

### 12.3 NAcc ≠ Sole Reward Area

Multiple regions (OFC, insula, amygdala).

### 12.4 Addiction ≠ Weak Will

It's brain circuit change, not pure psychology.

### 12.5 Anhedonia Hard to Quantify

Subjective — pleasure hard to objectively measure.

---

## 13. Related Concepts

- **Same section**: [Motor System](Motor_System.en.md), [Hippocampus + Memory](Hippocampus_Memory.en.md)
- **Anatomy**: [Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.en.md)
- **AI comparison**: [Reinforcement Learning](https://jeffliulab.github.io/ai-notes/04_Reinforcement_Learning/)

---

## References

1. **Schultz, W.** "A neural substrate of prediction and reward." *Science*, 1997.
2. **Berridge, K. C. & Robinson, T. E.** "What is the role of dopamine in reward?" *Brain Res Rev*, 1998.
3. **Sutton, R. S. & Barto, A. G.** *Reinforcement Learning: An Introduction*. MIT, 2018.
4. **Volkow, N. D. et al.** "Addiction: pull of pleasure overrides break of self-control." *Nature Rev Drug Discov*, 2017.
