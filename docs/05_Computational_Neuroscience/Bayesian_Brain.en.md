# Bayesian Brain

> *Bayesian Brain hypothesis: the brain is a statistical inference machine, using Bayes' rule to integrate prior + likelihood → posterior. Helmholtz 1860s prototype → Knill, Pouget, Friston modernization. Explains perception, motor, learning across tasks. Connects to Free Energy + Predictive Coding.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Predictive Coding](Predictive_Coding.en.md), [Probability Theory](https://jeffliulab.github.io/ai-notes/00_Computing_Science/01_Math_Foundations/Probability/)

---

## 1. Bayes' Rule

$$P(H | D) = \frac{P(D | H) P(H)}{P(D)}$$

- $P(H)$: prior
- $P(D|H)$: likelihood
- $P(H|D)$: posterior
- $P(D)$: evidence (normalizing)

---

## 2. Brain Applications

### 2.1 Perception

$$P(\text{world state} | \text{sensory}) \propto P(\text{sensory} | \text{world state}) \cdot P(\text{world state})$$

- Prior: based on past experience
- Likelihood: based on sensory model
- Posterior: perception = most likely state

### 2.2 Motor

$$P(\text{movement} | \text{goal}) \propto P(\text{goal achieved} | \text{movement}) \cdot P(\text{movement})$$

Movement plan = Bayesian inference of best action.

### 2.3 Learning

Prior updates: each new experience → new prior.
Bayesian model = lifetime continual learning.

---

## 3. Classic Experimental Evidence

### 3.1 Cue Combination

Two cues (vision + haptic) → combined estimate = optimal weighted by reliability (Ernst & Banks 2002).

$$\hat{x} = \frac{w_v \hat{x}_v + w_h \hat{x}_h}{w_v + w_h}, \quad w = 1/\sigma^2$$

Mathematically equivalent to Bayesian.

### 3.2 Optical Illusions

Many illusions = prior overriding sensory (e.g., light from above → concave/convex judgment).

### 3.3 Motor Adaptation

Force field adaptation rate matches Bayesian update rate.

---

## 4. Free Energy Framework (Friston)

$$F = -\log P(o) + D_{KL}(q(s) || p(s|o))$$

Minimize F ≈ minimize prediction error ≈ Bayesian inference.

→ Unifies perception + learning + action.

---

## 5. Active Inference

Action chosen to minimize **expected** free energy:

$$G(\pi) = \mathbb{E}[F | \pi]$$

- Epistemic: reduce uncertainty (explore)
- Pragmatic: satisfy preferences (exploit)

→ Natural exploration/exploitation balance.

---

## 6. vs AI

| Concept | Bayesian Brain | AI |
|---|---|---|
| Probability | implicit | explicit |
| Inference | exact (theory) / approximate (reality) | variational, MCMC |
| Prior | lifetime learned | pre-trained |
| Computation | spike-based? | GPU matrix |

VAE / Bayesian NN / Probabilistic programming are analogs to Bayesian Brain.

---

## 7. PyTorch — Bayesian Inference Toy

```python
import torch

def bayesian_perception(prior_mu, prior_sigma, sensory_obs, sensory_sigma):
    inv_var_post = 1/prior_sigma**2 + 1/sensory_sigma**2
    mu_post = (prior_mu/prior_sigma**2 + sensory_obs/sensory_sigma**2) / inv_var_post
    sigma_post = (1 / inv_var_post) ** 0.5
    return mu_post, sigma_post

prior_mu, prior_sigma = 100.0, 5.0
sensory_obs, sensory_sigma = 110.0, 3.0
mu_post, sigma_post = bayesian_perception(prior_mu, prior_sigma, sensory_obs, sensory_sigma)
print(f"Posterior: μ={mu_post:.2f}, σ={sigma_post:.2f}")
```

---

## 8. Neural Implementation

How Bayesian computation is implemented by spikes?

- **Probabilistic population coding** (Ma 2006): population firing rate ∝ Poisson likelihood
- **Neural sampling** (Berkes 2011): spikes are posterior samples
- **Predictive coding circuits**: hierarchical Bayesian inference

---

## 9. Pathology

- **Schizophrenia**: abnormal prior → hallucinations (perceived not in stimuli)
- **Autism**: weak prior → over-reliance on sensory detail
- **Depression**: negative prior → biased negative interpretation
- **Anxiety**: overly high threat prior

---

## 10. Limits

- Exact Bayesian computation infeasible in high-dim → approximation
- Real brain may not explicitly represent probability
- Some behavior doesn't fit Bayesian (cognitive bias, heuristic)

---

## 11. Common Pitfalls

### 11.1 Bayesian ≠ Optimal

Just framework; real brain may use heuristics + biases.

### 11.2 Prior Hard to Quantify

Human priors mostly implicit.

### 11.3 Computation Feasibility

Exact Bayesian for high-dim is NP-hard.

### 11.4 With Free Will

Philosophy: if brain is statistical machine, where is "free choice"?

### 11.5 Cultural Variation

Priors are culture-dependent (different cultures, different baseline expectations).

---

## 12. Related Concepts

- **Same section**: [Predictive Coding](Predictive_Coding.en.md), [Spiking NN](Spiking_Neural_Networks.en.md)
- **AI**: [VAE](https://jeffliulab.github.io/ai-notes/02_Deep_Learning/06_Generative_Models/VAE/)

---

## References

1. **Knill, D. C. & Pouget, A.** "The Bayesian brain: the role of uncertainty in neural coding and computation." *Trends Neurosci*, 2004.
2. **Ernst, M. O. & Banks, M. S.** "Humans integrate visual and haptic information in a statistically optimal fashion." *Nature*, 2002.
3. **Friston, K.** "The free-energy principle." *Nat Rev Neurosci*, 2010.
4. **Ma, W. J. et al.** "Bayesian inference with probabilistic population codes." *Nat Neurosci*, 2006.
