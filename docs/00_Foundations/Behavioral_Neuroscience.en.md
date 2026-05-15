# Behavioral Neuroscience

> *Behavior is the ultimate dependent variable of neuroscience. "Neuroscience needs behavior" (Krakauer 2017): without good behavioral quantification, neural data is meaningless. From classical ethology (Tinbergen's 4 questions) to modern computational ethology (DeepLabCut markerless tracking). Behavioral paradigm design determines what questions can be asked.*
>
> **Difficulty**: Beginner-Intermediate
> **Prerequisites**: [Research Methods](Research_Methods.en.md)

---

## 1. Why Behavior Is Central

- Behavior = brain's output (ultimate dependent variable)
- Neural activity only meaningful relative to behavior
- Krakauer 2017: over-reduction (neuron-first) ignores behavioral structure
- "Understanding the brain = explaining how behavior is produced"

---

## 2. Tinbergen's Four Questions (1963)

Understanding any behavior requires asking 4 levels:
1. **Causation** (mechanism): what triggers it? (proximate)
2. **Development** (ontogeny): how does it form developmentally?
3. **Function** (adaptive value): why advantageous for survival/reproduction? (ultimate)
4. **Evolution** (phylogeny): evolutionary history?

→ Similar to Marr levels (see [Levels of Analysis](Levels_of_Analysis.en.md)).

---

## 3. Classical Ethology

- Lorenz: imprinting
- Tinbergen: supernormal stimuli, fixed action patterns
- von Frisch: bee dance language
- The three shared 1973 Nobel (animal behavior)

---

## 4. Behavioral Paradigms

| Paradigm | Measures |
|---|---|
| Morris water maze | Spatial memory |
| Fear conditioning | Associative learning |
| Operant (Skinner box) | Reinforcement learning |
| Open field | Anxiety / exploration |
| Forced swim | Depression-like (debated) |
| 2AFC / random dots | Perceptual decision |
| Delayed match-to-sample | Working memory |
| Reaching task | Motor control |

---

## 5. Computational Ethology

- **DeepLabCut** (Mathis 2018): markerless pose estimation (transfer learning)
- **SLEAP**, **MoSeq** (unsupervised behavioral motif decomposition)
- Automatic quantification of natural behavior (not simplified tasks)
- Synchronized with neural recording

---

## 6. PyTorch — Behavior Classification (Pose → Behavior)

```python
import torch
import torch.nn as nn

class BehaviorClassifier(nn.Module):
    """Pose keypoints → behavior label (groom/walk/rear)."""
    def __init__(self, n_keypoints=15, n_behaviors=5):
        super().__init__()
        self.lstm = nn.LSTM(n_keypoints*2, 64, batch_first=True)
        self.fc = nn.Linear(64, n_behaviors)
    def forward(self, pose_seq):
        # pose_seq: (B, T, n_keypoints*2)
        h, _ = self.lstm(pose_seq)
        return self.fc(h[:, -1])
```

---

## 7. Psychophysics

- Quantifies stimulus → percept relationship
- **Weber-Fechner law**: $\Delta I / I = $ const
- **Stevens power law**: $\psi = k I^a$
- **Signal detection theory**: d' (sensitivity) + criterion
- Bridges behavior and neural coding

---

## 8. Behavior vs Neural: Causality

- Correlation: neuron firing ↔ behavior
- Need perturbation: optogenetics / lesion to prove causal
- But perturbation also has off-target, compensation
- Behavioral readout must be sensitive (else effects missed)

---

## 9. Natural Behavior vs Lab Tasks

- Lab tasks: controlled but unnatural, over-trained
- Natural behavior: ecologically valid but hard to quantify
- Trend: naturalistic + high-dimensional behavior tracking + neural recording

---

## 10. Common Pitfalls

### 10.1 Neural Data > Behavior

Without good behavioral quantification, neural data has no explanatory power (Krakauer).

### 10.2 Behavior Is Simple

Behavior is high-dimensional + sequential; oversimplified tasks lose information.

### 10.3 Lab Task = Natural Behavior

Over-trained tasks may use non-natural strategies.

### 10.4 Correlation = Mechanism

Need causal perturbation; but perturbation also has confounds.

### 10.5 Forced Swim = Depression

Model validity debated; don't over-interpret animal models.

---

## 11. Related Concepts

- **Same section**: [Research Methods](Research_Methods.en.md), [Levels of Analysis](Levels_of_Analysis.en.md), [Comparative Neuroscience](Comparative_Neuroscience.en.md)
- **Cognition**: [Decision Making](../04_Cognitive_Neuroscience/Decision_Making.en.md)
- **Systems**: [Motor System](../03_Systems_Neuroscience/Motor_System.en.md)
- **AI**: pose estimation, behavior classification

---

## References

1. **Tinbergen, N.** "On aims and methods of ethology." *Z Tierpsychol*, 1963.
2. **Krakauer, J. W. et al.** "Neuroscience needs behavior: correcting a reductionist bias." *Neuron*, 2017.
3. **Mathis, A. et al.** "DeepLabCut: markerless pose estimation of user-defined body parts with deep learning." *Nat Neurosci*, 2018.
4. **Datta, S. R. et al.** "Computational neuroethology: a call to action." *Neuron*, 2019.
