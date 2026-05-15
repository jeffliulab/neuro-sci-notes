# 贝叶斯大脑 (Bayesian Brain)

> *Bayesian Brain 假说:大脑是 statistical inference 机器,用 Bayesian rule 整合 prior + likelihood → posterior。Helmholtz 1860s 原型 → Knill, Pouget, Friston 现代化。解释 perception, motor, learning 等多任务。与 Free Energy + Predictive Coding 连。*
>
> **难度**:Advanced
> **前置知识**:[Predictive Coding](Predictive_Coding.md)、[概率论](https://jeffliulab.github.io/ai-notes/00_Computing_Science/01_Math_Foundations/Probability/)

---

## 1. Bayes 法则

$$P(H | D) = \frac{P(D | H) P(H)}{P(D)}$$

- $P(H)$: prior (先验)
- $P(D|H)$: likelihood
- $P(H|D)$: posterior
- $P(D)$: evidence (normalizing)

---

## 2. 大脑应用

### 2.1 Perception

$$P(\text{world state} | \text{sensory}) \propto P(\text{sensory} | \text{world state}) \cdot P(\text{world state})$$

- Prior:基于先前经验
- Likelihood:基于 sensory 模型
- Posterior:perception 即 most likely state

### 2.2 Motor

$$P(\text{movement} | \text{goal}) \propto P(\text{goal achieved} | \text{movement}) \cdot P(\text{movement})$$

Movement plan = Bayesian inference of best action。

### 2.3 Learning

Prior 更新:每 new experience → 新 prior。
Bayesian model = lifetime continual learning。

---

## 3. 经典实验证据

### 3.1 Cue Combination

Two cues (vision + haptic) → combined estimate = optimal weighted by reliability (Ernst & Banks 2002)。

$$\hat{x} = \frac{w_v \hat{x}_v + w_h \hat{x}_h}{w_v + w_h}, \quad w = 1/\sigma^2$$

数学等价 Bayesian。

### 3.2 Optical Illusions

Many illusions = prior overriding sensory (e.g., light from above → 凹凸判断)。

### 3.3 Motor Adaptation

Force field adaptation 速度 matches Bayesian update rate。

---

## 4. Free Energy 框架 (Friston)

$$F = -\log P(o) + D_{KL}(q(s) || p(s|o))$$

Minimize F ≈ minimize prediction error ≈ Bayesian inference。

→ 统一 perception + learning + action。

---

## 5. Active Inference

Action chosen to minimize **expected** free energy:

$$G(\pi) = \mathbb{E}[F | \pi]$$

- Epistemic: 减少 uncertainty (explore)
- Pragmatic: 满足 preferences (exploit)

→ 自然 exploration/exploitation balance。

---

## 6. 与 AI 对比

| 概念 | Bayesian Brain | AI |
|---|---|---|
| Probability | implicit | explicit |
| Inference | exact (theory) / approximate (reality) | variational, MCMC |
| Prior | lifetime learned | pre-trained |
| Computation | spike-based? | GPU matrix |

VAE / Bayesian NN / Probabilistic programming 与 Bayesian Brain 类比。

---

## 7. PyTorch — Bayesian Inference Toy

```python
import torch

def bayesian_perception(prior_mu, prior_sigma, sensory_obs, sensory_sigma):
    """1-D Bayesian estimation."""
    # Bayes update (Gaussian)
    inv_var_post = 1/prior_sigma**2 + 1/sensory_sigma**2
    mu_post = (prior_mu/prior_sigma**2 + sensory_obs/sensory_sigma**2) / inv_var_post
    sigma_post = (1 / inv_var_post) ** 0.5
    return mu_post, sigma_post

# Example
prior_mu, prior_sigma = 100.0, 5.0  # historical avg height
sensory_obs, sensory_sigma = 110.0, 3.0  # noisy measurement
mu_post, sigma_post = bayesian_perception(prior_mu, prior_sigma, sensory_obs, sensory_sigma)
print(f"Posterior: μ={mu_post:.2f}, σ={sigma_post:.2f}")
```

---

## 8. Neural Implementation

如何 Bayesian compute 由 spikes 实现?

- **Probabilistic population coding** (Ma 2006): population firing rate ∝ Poisson likelihood
- **Neural sampling** (Berkes 2011): spikes 是 posterior sample
- **Predictive coding circuits**: hierarchical Bayesian inference

---

## 9. 病理

- **Schizophrenia**: prior 异常 → 幻觉 (perceived 不在 stimuli)
- **Autism**: prior 弱 → 过依赖 sensory detail
- **Depression**: negative prior → 偏 negative interpretation
- **Anxiety**: 过高 threat prior

---

## 10. Limits

- Exact Bayesian computation 在 high-dim 不可行 → 近似
- Real brain 不一定显式 represent probability
- Some behavior 不 fit Bayesian (cognitive bias, heuristic)

---

## 11. Common Pitfalls

### 11.1 Bayesian ≠ optimal

只是 framework;real brain 可能用 heuristic + bias。

### 11.2 Prior 难量化

人 prior 多 implicit。

### 11.3 Computation feasibility

Exact Bayesian for high-dim 是 NP-hard。

### 11.4 与 free will

哲学:if brain 是 statistical machine,where is "free choice"?

### 11.5 Cultural variation

Prior 文化依赖 (不同 culture 不同 baseline expectation)。

---

## 12. Related Concepts

- **同节**:[Predictive Coding](Predictive_Coding.md)、[Spiking NN](Spiking_Neural_Networks.md)
- **AI**:[VAE](https://jeffliulab.github.io/ai-notes/02_Deep_Learning/06_Generative_Models/VAE/)

---

## References

1. **Knill, D. C. & Pouget, A.** "The Bayesian brain: the role of uncertainty in neural coding and computation." *Trends Neurosci*, 2004.
2. **Ernst, M. O. & Banks, M. S.** "Humans integrate visual and haptic information in a statistically optimal fashion." *Nature*, 2002.
3. **Friston, K.** "The free-energy principle." *Nat Rev Neurosci*, 2010.
4. **Ma, W. J. et al.** "Bayesian inference with probabilistic population codes." *Nat Neurosci*, 2006.
