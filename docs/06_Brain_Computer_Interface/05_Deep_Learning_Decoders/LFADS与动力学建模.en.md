# LFADS and Dynamics Modeling

**LFADS (Latent Factor Analysis via Dynamical Systems)**, proposed by Pandarinath et al. in Nature Methods 2018, was the first BCI method to systematically combine **deep learning** with **neural dynamics modeling**. It turned the Churchland-Shenoy theory that "population activity is a low-dimensional dynamical system" into a computable tool.

## 1. Core Idea

Assume neural population activity arises from a **low-dimensional nonlinear dynamical system**:

$$\mathbf{z}_t = f_\theta(\mathbf{z}_{t-1}, \mathbf{u}_t) \quad \text{(latent dynamics)}$$
$$\mathbf{x}_t \sim \text{Poisson}(\exp(W \mathbf{z}_t)) \quad \text{(observation)}$$

where:
- $\mathbf{z}_t \in \mathbb{R}^d$ is the latent state ($d \ll N$, typically 30–128)
- $\mathbf{u}_t$ is the external input
- $\mathbf{x}_t \in \mathbb{R}^N$ is the spike count
- $f_\theta$ is the nonlinear dynamics implemented with a GRU

LFADS uses a **variational autoencoder (VAE)** to infer this model: given spike data, it infers the most likely latent dynamics.

## 2. LFADS Architecture

```
spike x_{1:T} 
  ↓
Encoder (bidirectional GRU)
  ↓
Initial condition z_0 (posterior)
  ↓
Generator (GRU unroll)
  ↓
Latent factors z_{1:T}
  ↓
Linear + exp
  ↓
Poisson rate λ_{1:T}
  ↓
Reconstructed spikes (denoised)
```

An optional **inferred-input channel** lets external drives (sensory input, decision signals) enter the dynamics.

## 3. Training Objective (ELBO)

LFADS maximizes the ELBO:

$$\mathcal{L} = \mathbb{E}_q[\log P(x | z)] - \text{KL}(q(z | x) \| P(z))$$

- Reconstruction term: Poisson negative log-likelihood
- KL term: VAE regularization pulling the posterior toward the prior

## 4. What LFADS Can Do

### 1. Denoising / Smoothing

Converts noisy spike counts into continuous, smooth rates. **Visually close to "seeing the neuron's intent."**

### 2. Single-Trial Recovery

Classical averaging requires aligning many trials; LFADS can infer dynamical trajectories **from a single trial**. This matters for BCI — BCI is fundamentally single-trial decoding.

### 3. Low-Dimensional Visualization

The latent space $z_t$ is typically 30–64 dimensions and can be further reduced to 2–3 dimensions with PCA for visualization — showing **neural state trajectories**.

### 4. Improved Decoding

A decoder operating on $z_t$ (rather than raw spikes) performs substantially better than one operating on spikes directly.

## 5. Milestone Results

**Pandarinath 2018 Nat Methods** in a monkey reach task:
- Single-trial rate estimation R² = 0.85 (classical smoothing 0.4)
- Downstream decoding (position) R² 20–50% higher than Kalman
- Successfully recovered known rotational dynamics

**Keshtkaran 2022 Nat Methods (AutoLFADS)**: automated hyperparameter tuning, lowering the experimenter bar for LFADS.

## 6. LFADS and Neural Manifold Theory

LFADS is **the computational realization of Churchland-Shenoy's manifold-dynamics hypothesis**:

- **Manifold**: the latent space $\mathbb{R}^d$
- **Dynamics**: $f_\theta$ is a GRU that learns rotations, attractors, saddle points, etc.
- **Cross-trial sharing**: the same GRU is used across all trials, reflecting "reuse of brain computation"

This makes LFADS more than a decoder — it is **a tool for inferring neuroscience hypotheses with neural networks**.

## 7. Variants of LFADS

### AutoLFADS

Automates tuning of KL weight, dropout, and 8+ other hyperparameters.

### LFADS + Behavioral Prior

**Sani 2021** constrains LFADS's latent space with behavioral variables — similar in spirit to early CEBRA.

### TNDM

**Hurwitz 2021** extends LFADS to non-stationary systems, supporting cross-session transfer.

### iLQR on Latent Dynamics

**Pei 2021** performs **model predictive control (MPC)** in LFADS's latent space — this is **"model-based RL for BCI."**

## 8. Comparison of LFADS with Transformers

| | LFADS | NDT3 / POYO |
| --- | --- | --- |
| Structure | VAE + GRU | Transformer |
| Training | Single dataset, supervised | Multi-dataset, self-supervised pretraining |
| Latent state | Explicit continuous | Implicit via attention |
| Cross-subject | Poor | Strong (foundation model) |
| Interpretability | High (dynamical system) | Low |
| Compute | Fast | Slow but scalable |

**LFADS remains the top choice for interpretability and dynamics modeling**; Transformers win on scale and cross-subject transfer.

## 9. Open-Source Implementations

- **[lfads-torch](https://github.com/arsedler9/lfads-torch)**: PyTorch version
- **[AutoLFADS](https://github.com/snel-repo/autolfads-tf2)**: TensorFlow 2 with automatic tuning
- **NLB (Neural Latents Benchmark)**: a unified benchmark for evaluating LFADS and related methods

## 10. Legacy for BCI Engineering

LFADS leaves three lasting impacts:

1. **"Latent space = decoding target"**: a decoder need not consume raw signals; it can work in the learned latent space
2. **"Dynamics modeling = prior"**: even with neural networks, the "smooth evolution" prior must be respected
3. **"Denoising is decoding"**: a good generative model is itself a good decoder

These ideas permeate later deep-learning BCI work: NDT, POYO, CEBRA.

## 11. Logical Chain

1. **Classical decoders consume spikes directly**, ignoring the low-dimensional dynamical structure of population activity.
2. **LFADS uses VAE + GRU to explicitly model the latent dynamical system**, enabling single-trial rate estimation.
3. **The Churchland-Shenoy manifold-dynamics hypothesis** becomes a computable tool within LFADS.
4. **Decoders on the latent space substantially outperform those on raw spikes** — the core engineering value of LFADS.
5. **LFADS launched the "dynamics modeling + deep learning" BCI paradigm**, directly spawning NDT and POYO.

## References

- Pandarinath et al. (2018). *Inferring single-trial neural population dynamics using sequential auto-encoders.* Nat Methods. https://www.nature.com/articles/s41592-018-0109-9
- Keshtkaran et al. (2022). *A large-scale neural network training framework for generalized estimation of single-trial population dynamics.* Nat Methods.
- Sani et al. (2021). *Modeling behaviorally relevant neural dynamics enabled by preferential subspace identification.* Nat Neurosci. — PSID
- Pei et al. (2021). *Neural latents benchmark '21: evaluating latent variable models of neural population activity.* NeurIPS. https://arxiv.org/abs/2109.04463
- Hurwitz et al. (2021). *Targeted neural dynamical modeling.* NeurIPS.
