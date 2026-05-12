# Closed-loop Control and CLDA

**Closed-loop** operation is what most fundamentally separates a BCI from traditional offline neural decoding — the user sees the result, adjusts their neural activity, and the decoder keeps learning during the interaction. This process of **user and decoder co-evolving** is called **co-adaptation**, and its technical realization is **CLDA (Closed-Loop Decoder Adaptation)**.

## 1. Why Closed-Loop Is Required

### Problems of open-loop BCIs

Offline training + static decoder = **open-loop BCI**:

- the user's neural activity changes due to electrode drift, fatigue and learning
- the decoder parameters stay fixed → performance degrades over time
- the user cannot adjust their strategy because they do not know what would be decoded better

### Advantages of closing the loop

- **the user sees feedback** and can learn "how to think" so the decoder reads it correctly
- **the decoder keeps updating** to track the changing neural activity
- **the two co-evolve**, producing a stable neuroprosthetic skill

**Analogy:** when learning to type, keyboard feedback tells you whether you hit the right key — a BCI needs the same feedback loop.

## 2. The Key Role of Neural Plasticity

Over years of work, Nicolelis observed that **the brain allocates representational resources to the BCI** — even when the implant location is not "ideal", with training the neural activity adapts to the BCI task.

This phenomenon is called **neuroprosthetic plasticity**:

- neuron tuning changes to match the decoder
- unused degrees of freedom are "pruned"
- frequently used BCI tasks grow dedicated neural networks

**Engineering implication:** **good BCI training is not the same as good offline performance** — what matters is how well the user can do with the closed loop.

## 3. Three CLDA Strategies

### 1. Batch Retraining

Periodically (each day, each week) retrain the decoder with new data.

- simple, but the updates are discontinuous
- the user **feels performance decay** between retraining events
- used in early BrainGate

### 2. Incremental Update (SmoothBatch)

**Orsborn et al. 2014 Neuron** proposed:

$$\theta_{t+1} = (1-\alpha) \theta_t + \alpha \theta_{\text{new}}$$

- $\theta_t$ are the decoder parameters
- $\theta_{\text{new}}$ is the solution on new data
- $\alpha \in [0.01, 0.1]$ is the smoothing step
- updated every 100–500 ms

**Pros:** smooth, continuous, and feels stable to the user.

### 3. Reinforcement-learning driven

**Shanechi lab** treats decoder adaptation as an RL problem:

- state: decoder parameters
- action: parameter update
- reward: task success, user satisfaction

Parameters are optimised by POMDP or policy gradient.

## 4. Intent Assumptions

CLDA needs intent labels, but the user cannot directly tell the system "this is what I want." Three common assumptions:

### 1. Goal-directed

Assume the user's intent is "toward the current goal" — **the ReFIT assumption** (see [ReFIT and Online Recalibration](../04_Classical_Decoding/ReFIT与在线校准.md)).

### 2. Posterior labeling

After the user reaches the target, retroactively label "during that window the intent was X."

### 3. Reinforcement signal

The user provides a + reward on task completion and − on failure, and RL drives the parameter update.

**SPIKEIRL** (Sani 2023) is a representative example.

## 5. Three Stages of User Learning

Orsborn and follow-up studies found that users learn a BCI in three stages:

### Stage 1: exploration

- days 1–3
- high variability in neural activity
- low task success rate
- the decoder is still adapting

### Stage 2: stabilization

- weeks 1–4
- the user finds a stable strategy
- success rate rises
- the decoder converges

### Stage 3: expertise

- 2+ months
- neural activity becomes highly consistent
- "automatisation" even appears — the user no longer consciously controls it
- decoder parameters are fixed

**Long-term BCI users** such as BrainGate's Cathy Hutchinson reach Stage 3 after 5+ years of use, with near-**muscle-memory** neural control.

## 6. Closed-loop Latency Budget

The key to a stable closed loop is **end-to-end latency**:

```
neural → acquisition (5 ms) → preprocessing (5 ms) → decoding (10 ms) → actuation (10 ms) → feedback (20 ms)
                                                                                                 ↑
                                                                                     user visual perception ~30 ms
```

**Total latency < 100 ms** is required to feel "natural." Above 100 ms the user senses "lag" and closed-loop learning suffers.

The latency of deep-learning decoders is a key challenge — the $O(n^2)$ complexity of Transformer self-attention needs optimisation.

## 7. Neural-Machine Co-adaptation

Several mathematical views of **co-adaptation**:

### Game-theoretic view

The user (U) and the decoder (D) play a cooperative game:

- U picks a neural policy $\pi_U$
- D picks decoder parameters $\theta_D$
- joint objective: maximise task performance

Nash equilibrium = a stable co-adaptation state.

### Bayesian view

- the user maintains a belief over decoder parameters, $P(\theta_D | \text{history})$
- the decoder maintains a belief over user intent, $P(u | \text{neural})$
- iterating the closed loop causes the two beliefs to converge

### Control-theoretic view

The decoder is the controller, the user is the plant. Co-adaptation is an **adaptive-control** problem — Dangi 2013 gives a formal framework.

## 8. A Classic Experiment: Orsborn 2014

The core experiment of **Orsborn et al. 2014 Neuron**:

1. implant a Utah Array into M1 of a monkey
2. compare three groups:
   - (a) static decoder (trained open-loop, then frozen)
   - (b) SmoothBatch CLDA
   - (c) ReFIT (one-shot recalibration)
3. test for one month

**Results:**

- (a) performance keeps declining
- (c) high initially, then decays
- (b) **keeps improving and ends with the best performance**

This showed **continuous adaptation > one-shot recalibration > static**.

## 9. CLDA Meets Modern Deep Learning

### Traditional CLDA

Updates linear Kalman parameters — simple and stable.

### Deep CLDA

Updates deep-network parameters — more complex:

- gradient updates can oscillate
- regularisation is needed
- may suffer "catastrophic forgetting"

Solutions:

- **LoRA**-style low-rank updates
- **Elastic Weight Consolidation**
- **replay buffer** (mix old and new data)

### CLDA on foundation models

**NDT3 + online fine-tuning**: a pretrained foundation model + a small LoRA update per session — combining the stability of pretraining with the adaptivity of online learning.

## 10. CLDA in Clinical Practice

- **Neuralink PRIME**: co-adaptation stabilises 1–2 months after surgery
- **BrainGate**: long-term (5+ year) use, with the decoder updated once a month
- **Pitt ICMS**: updates both read (M1) and write (S1) encoders simultaneously

**Clinical lesson:** CLDA is not a "bonus", it is a **necessary condition** for a BCI to stay usable over the long term.

## 11. Chain of Reasoning

1. **An open-loop BCI must degrade as neural activity changes** — closing the loop is the only solution.
2. **CLDA lets decoder parameters keep updating** alongside user learning.
3. **SmoothBatch / ReFIT / RL-driven** are the three main CLDA strategies.
4. **User learning goes through three stages** and can eventually reach "muscle-memory" level automation.
5. **Deep-learning CLDA needs new tools** (LoRA, EWC, replay) to handle forgetting and instability.
6. **CLDA is the engineering that takes BCIs from the lab into the clinic.**

## References

- Orsborn et al. (2014). *Closed-loop decoder adaptation shapes neural plasticity for skillful neuroprosthetic control.* Neuron. https://www.cell.com/neuron/fulltext/S0896-6273(14)00871-1
- Dangi et al. (2013). *Design and analysis of closed-loop decoder adaptation algorithms for brain-machine interfaces.* Neural Comp.
- Shenoy & Carmena (2014). *Combining decoder design and neural adaptation in brain-machine interfaces.* Neuron.
- Sani et al. (2023). *Reinforcement learning for closed-loop adaptation of brain-computer interfaces.* arXiv.
- Jarosiewicz et al. (2015). *Virtual typing by people with tetraplegia using a self-calibrating intracortical BCI.* Sci Transl Med.
