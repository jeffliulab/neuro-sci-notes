# Introduction to Brain-Computer Interface

## Why 2024–2026 Is the Tipping Point for BCI × AI

For sixty years, **Brain-Computer Interface (BCI)** has remained in the "laboratory miracle" stage: Hans Berger recorded the first human EEG in 1924; Wolpaw coined the term "direct brain-computer interface" in the early 1990s; the BrainGate team enabled a paralyzed patient to move a cursor by thought in 2006; in 2012 Cathy Hutchinson used her thoughts to drive a robotic arm to drink her first sip of coffee in twenty years. Each advance was striking, yet none broke out of strict clinical-trial confines.

**2024–2026 is the turning point.** Three forces are reaching critical mass simultaneously:

| Force | Critical-point event | Time |
| --- | --- | --- |
| **Algorithms** | Neural foundation models (NDT3, POYO, CEBRA) achieve few-shot cross-subject transfer | 2023–2024 |
| **Commercial** | Neuralink PRIME implants 12+ subjects, Synchron COMMAND completes, Precision Layer 7 receives FDA clearance, Neuracle × Tsinghua receives China NMPA approval (world's first commercialized invasive BCI) | 2024–2026 |
| **Legislation** | Chilean Constitution (2021), Colorado (2024), Minnesota *Cognitive Liberty Act* (2024), UNESCO recommendation on neurotechnology ethics (2024), EU AI Act (2025) | 2021–2025 |

These three forces arriving together mean BCI is no longer an isolated medical-device discipline — it is a multidisciplinary frontier interwoven with large models, embodied robotics, cognitive science, and constitutional legislation.

---

## Central Narrative: Intention-to-Action

If the main thread of this chapter must be stated in one sentence, it is:

> **Extract intent from neural signals, use learned decoders and LLM planning to drive external devices or robots to complete actions.**

This "Intention-to-Action (I2A) pipeline" has three segments:

1. **Neural signal → Intent (intent decoding):** Extract *what the user wants to do* from spikes/LFP/ECoG/EEG, rather than low-level muscle control parameters.
2. **Intent → Action plan (shared autonomy / LLM planning):** Use probabilistic reasoning (POMDP), hierarchical planning (BCI → LLM → ROS2), or RL policies to translate discrete/high-level intents into concrete action sequences.
3. **Action → Sensory feedback (sensory writing / closed loop):** Write tactile, proprioceptive, or visual signals back to the cortex via ICMS, closing the loop.

Traditional BCI only covered segment 1: low-level kinematic decoding (decoding velocity, position, force). Modern BCI plugs LLM / world-model / RL into segment 2 and uses foundation models to make segment 1 few-shot, cross-subject, and transferable. **This is the fundamental difference between 2024–2026 and the prior decade.**

---

## Relationship to "Human-Like Intelligence"

This chapter and [Human-Like Intelligence](https://jeffliulab.github.io/ai-notes/01_AI/03_Frontiers/index.md) are **sister chapters.** They represent two paths toward AGI and embodied intelligence:

| Dimension | Human-Like Intelligence chapter | This chapter (BCI) |
| --- | --- | --- |
| Angle | Construct mind from the algorithm/architecture side | Establish direct pathway from the biological neural side |
| Core concepts | Predictive coding, world models, causality, meta-learning | Neural manifolds, I2A pipeline, closed-loop control, shared autonomy |
| Key figures | LeCun, Friston, Tenenbaum, Bengio, Fei-Fei Li | Shenoy, Churchland, Willett, Shanechi, Collinger, Andersen |
| Representative systems | JEPA, AMI Labs, World Labs | BrainGate, Neuralink N1, Stentrode, Pitt arm |

The two chapters explicitly meet in **[10 Link to Embodied Intelligence](10_Embodied_Intelligence_Link/index.md)**: motor cortex as a dynamical system; neural manifolds are essentially the latent space of RL policies; BCI lets us, for the first time, *read out* a working world model from a **biological system**.

---

## Five-Tier Progression

This chapter has 14 sub-sections organized in five tiers:

```
Tier 1 (Physical foundations)   01 Foundations → 02 Neurophysiology → 03 Signal Acquisition
Tier 2 (Algorithms)             04 Classical Decoding → 05 Deep Learning Decoders
Tier 3 (AI frontier) ⭐          06 Intention-to-Action → 07 Brain-to-Language → 08 Brain-to-Image
Tier 4 (Bidirectional/sensory)  09 Sensory Writing & Bidirectional → 10 Link to Embodied Intelligence
Tier 5 (Ecosystem/ethics)       11 Commercial/Clinical → 12 Consumer Non-Invasive → 13 Ethics/Neurorights → 14 Datasets/Tools
```

Recommended reading paths:

- **Readers with AI/algorithm background:** Enter at chapter 06 (Intention-to-Action), read back to chapters 04 and 05 for algorithmic foundations; then read 07 and 08 to see how LLMs and diffusion models embed into the BCI pipeline.
- **Readers with neuroscience background:** Proceed 02 → 03 → 04 in order; focus on neural manifolds in 02 and the dynamical-systems dialogue in 10.
- **Readers with product/business background:** Enter at chapter 11 (Commercial/Clinical), organized by company; pair with chapter 13 (Neurorights) to understand the regulatory environment.
- **Readers with ethics/policy background:** Enter at chapter 13, supplemented by chapters 07 and 08 to grasp the concrete technical risks brought by "LLM reading the brain."

---

## Key Figures at a Glance

| Person | Core contribution | Representative system/lab |
| --- | --- | --- |
| Krishna Shenoy (deceased) | Motor cortex as dynamical system; neural latent-space modeling | Stanford Neural Prosthetics Lab |
| Mark Churchland | Rotational dynamics; preparatory subspace | Columbia Zuckerman Institute |
| Leigh R. Hochberg | Clinical translation of implanted BCI | BrainGate consortium |
| Frank Willett | High-performance handwriting and speech BCI | Stanford NPTL |
| Jennifer Collinger | Pittsburgh robotic arm; ICMS somatosensory feedback | U. Pittsburgh |
| Maryam Shanechi | Adaptive BCI; DPAD; emotional BCI | USC Viterbi |
| Richard Andersen | High-level intent decoding in posterior parietal | Caltech |
| Edward Chang | Speech cortex decoding | UCSF |
| Edward Chang team + Sean Metzger | 2023 speech avatar | UCSF |
| Elon Musk / DJ Seo | High-throughput flexible electrodes and surgical robot | Neuralink |
| Thomas Oxley | Stentrode transvascular BCI | Synchron |
| Ben Rapoport | Layer 7 thin-film microelectrodes | Precision Neuroscience |
| Hong Bo | Tsinghua NEO semi-invasive BCI | Tsinghua University × Neuracle |

---

## Reading Prerequisites

This chapter assumes readers have:

- **Deep-learning fundamentals** (see [02_Deep_Learning](https://jeffliulab.github.io/ai-notes/02_Deep_Learning/01_Intro/index.md)): CNN, RNN, Transformer, diffusion-model basics
- **Reinforcement-learning fundamentals** (see [04_Reinforcement_Learning](https://jeffliulab.github.io/ai-notes/04_Reinforcement_Learning/02_Classic_RL/index.md)): MDP, policy gradient, POMDP
- **Some probability and signal processing:** Bayesian inference, Kalman filter, Fourier analysis

No neuroscience prerequisite is required — chapter 02 supplies the necessary neurophysiology concepts.

---

## Logical Chain

1. **BCI is a "read-brain + write-brain" physical pathway,** whose physical principles are constrained by how neural signals are generated and captured by electrodes.
2. **Neural signals are encoded as distributed population activity.** Classical BCI used linear models; modern BCI must capture nonlinear dynamics.
3. **Deep learning and foundation models brought BCI from "single-session calibration" into the "cross-subject transfer" era** — the biggest paradigm shift of 2023–2024.
4. **A true BCI is not a kinematic decoder, but an I2A pipeline:** intent extraction + LLM planning + robot control + sensory feedback.
5. **Sensory writing (ICMS) makes closed-loop possible** — the necessary path for BCI to evolve from "remote-controlled arm" to "embodied self."
6. **Motor cortex as a dynamical system** links BCI to human-like-intelligence research: neural manifolds are the latent space of biological policies.
7. **The commercial tipping point has arrived:** multiple FDA/NMPA approvals; the world's first commercialized invasive BCI launched in China in March 2026.
8. **But the combination of brain-reading and LLMs brings unprecedented privacy risks** — neurorights legislation is a necessary prerequisite.

## References

- Hochberg et al. (2006). *Neuronal ensemble control of prosthetic devices by a human with tetraplegia.* Nature. https://www.nature.com/articles/nature04970
- Willett et al. (2023). *A high-performance speech neuroprosthesis.* Nature. https://www.nature.com/articles/s41586-023-06377-x
- Musk, E. & Neuralink (2024). *PRIME Study Progress Update.* https://neuralink.com/updates/prime-study-progress-update/
- Brain Foundation Models Survey (2025). arXiv 2503.00580. https://arxiv.org/html/2503.00580v1
- Bloomberg (2026). *China approves first brain implant for commercial use.* https://www.bloomberg.com/news/articles/2026-03-13/china-approves-first-brain-implant-for-commercial-use
