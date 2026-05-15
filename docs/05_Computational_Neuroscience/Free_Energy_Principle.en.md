# Free Energy Principle

> *Karl Friston's Free Energy Principle (FEP): any self-maintaining system resists entropy by minimizing variational free energy (an upper bound on sensory surprise). Unifies perception (perceptual inference), learning, action (active inference). Highly ambitious but also highly controversial (unfalsifiability critique). A more general framework than predictive coding.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Predictive Coding](Predictive_Coding.en.md), [Bayesian Brain](Bayesian_Brain.en.md), variational inference

---

## 1. Core Claim

- Organisms must stay in limited states (homeostasis) → resist entropy
- Cannot directly minimize surprise (would need all possible sensations)
- But can minimize its upper bound: **variational free energy** $F$
- Perception + learning + action all minimize $F$

---

## 2. Math

Surprise (negative log evidence): $-\ln p(o)$

Variational free energy:
$$F = \underbrace{D_{KL}[q(s) \| p(s|o)]}_{\geq 0} - \ln p(o)$$

$$F = \underbrace{\mathbb{E}_q[-\ln p(o|s)]}_{\text{accuracy}} + \underbrace{D_{KL}[q(s)\|p(s)]}_{\text{complexity}}$$

- $q(s)$: brain's recognition density (approximate posterior)
- Minimizing $F$ ≈ variational Bayesian inference
- $F \geq -\ln p(o)$ (surprise upper bound)

---

## 3. Three Minimization Routes

| Route | Changes | Corresponds to |
|---|---|---|
| Perception | $q(s)$ | infer hidden states |
| Learning | parameters | slow weight update |
| Action | sensation itself | active inference |

---

## 4. Active Inference

- Not just passive inference → **action changes sensation** to match predictions
- Replaces classic RL reward with prior preference (expected observations)
- "I expect to see food → act to fulfill that prediction"
- Reward = log of prior preference

---

## 5. Relation to Predictive Coding

- Predictive coding is FEP's special case under hierarchical Gaussian models
- Prediction error = gradient of free energy
- FEP more general (includes action, learning)

---

## 6. PyTorch — Variational Free Energy Minimization

```python
import torch

def free_energy(obs, q_mean, q_logvar, prior_mean=0.0, prior_var=1.0,
                likelihood_var=0.1):
    """F = accuracy + complexity (single latent demo)."""
    q_var = q_logvar.exp()
    # Accuracy: expected neg log likelihood (predict obs from q_mean)
    accuracy = ((obs - q_mean) ** 2).mean() / (2 * likelihood_var)
    # Complexity: KL[q || prior]
    kl = 0.5 * ((q_var + (q_mean - prior_mean)**2) / prior_var
                - 1 - q_logvar + torch.log(torch.tensor(prior_var)))
    return accuracy + kl.mean()
```

→ Mathematically isomorphic to VAE's ELBO (F = -ELBO).

---

## 7. Relation to VAE / ELBO

- Variational free energy = negative ELBO (VAE training objective)
- Brain ≈ real-time variational autoencoder (hypothesis)
- But brain uses local message passing (not backprop)

---

## 8. Markov Blanket

- Statistical boundary between system and environment (internal vs external states interact via sensory + active states)
- FEP: any persisting system has a Markov blanket
- Scale-invariant description from cell to brain to ecosystem (a point of contention)

---

## 9. Criticisms

- **Unfalsifiable**: critics say too general → cannot be experimentally refuted (Colombo & Wright)
- **Math ≠ mechanism**: can fit any behavior
- **Reward restatement**: is active inference truly better than RL?
- Supporters: value of unified framework + concrete testable predictions exist (PC experiments)

---

## 10. Common Pitfalls

### 10.1 FEP = Predictive Coding

PC is FEP's special case; FEP includes action + learning.

### 10.2 Free Energy = Thermodynamic Free Energy

It's variational free energy (information-theoretic), a different concept (borrowed term).

### 10.3 Already Confirmed

Still debated; some predictions (PC) supported, but overall unfalsifiability critique unresolved.

### 10.4 Replaces RL

Active inference and RL mathematically inter-translatable; not a simple replacement.

### 10.5 Minimize Surprise = Seek Comfort (Dark Room Problem)

Prior preference + exploration (epistemic value) explains why agents don't hide in a dark room.

---

## 11. Related Concepts

- **Same section**: [Predictive Coding](Predictive_Coding.en.md), [Bayesian Brain](Bayesian_Brain.en.md)
- **Cognition**: [Consciousness](../04_Cognitive_Neuroscience/Consciousness.en.md)
- **AI**: VAE / ELBO, active inference agents

---

## References

1. **Friston, K.** "The free-energy principle: a unified brain theory?" *Nat Rev Neurosci*, 2010.
2. **Friston, K. et al.** "Active inference and learning." *Neurosci Biobehav Rev*, 2016.
3. **Buckley, C. L. et al.** "The free energy principle for action and perception: A mathematical review." *J Math Psychol*, 2017.
4. **Colombo, M. & Wright, C.** "First principles in the life sciences: the free-energy principle, organisms, and machines." *Synthese*, 2021.
