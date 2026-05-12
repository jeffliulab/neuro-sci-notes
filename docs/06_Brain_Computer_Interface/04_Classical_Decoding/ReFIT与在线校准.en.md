# ReFIT and Online Calibration

**ReFIT (Recalibrated Feedback Intention-Trained Kalman Filter)**, introduced by Gilja et al. in 2012, is one of the most important engineering improvements in BCI decoder design — it fundamentally changed the methodology of "how to train a BCI decoder."

## 1. The Training Problem of Classical Kalman

Standard Kalman training requires **paired data**: neural activity $\mathbf{y}_t$ and the corresponding movement intent $\mathbf{x}_t$. But paralyzed patients **have no real movement trajectory** to align with.

The classical workaround: an **observation task (OL, open-loop)** — the user watches the cursor move automatically toward the target, under the assumption that the user's neural activity encodes the "toward the target" intent.

**The problem**: the user is only **watching** the cursor, not **actively intending** to control it; neural activity distributions differ markedly from the closed-loop setting.

## 2. ReFIT's Core Insight

**What is the user's intent when actually controlling?**

When the cursor is at $\mathbf{p}_t$, the target at $\mathbf{g}$, and the user intends to reach the target — **the intent direction is always** $\mathbf{g} - \mathbf{p}_t$, regardless of where the cursor actually goes.

This means:
- Even when inaccurate decoding drives the cursor off course, **the user's intent is still toward the target**
- Training should use the **intent vector** (target direction), not the **actual trajectory**

## 3. ReFIT Two-Stage Training

```
Stage 1: Standard Kalman closed loop
  The user controls the cursor with the initial decoder
  Record [neural activity y_t, actual trajectory x_t]

Stage 2: Intent relabeling
  For each t, relabel intent = direction toward the current target
  x_t^refit = normalize(g - p_t)
  
  Retrain Kalman on [y_t, x_t^refit]
  → new H, Q/R matrices
```

### The Power of Assumption Correction

This "assume the user did it right" prior is extremely strong:

- Even if the cursor wandered during training, intent after relabeling still points in the correct direction
- The new decoder learns the "neural activity → correct intent" mapping
- Closed-loop performance rises substantially

## 4. Gilja 2012 Results

**Gilja et al. (2012, Nature Neurosci)** validated in monkeys:

- Standard Velocity-Kalman: ~3 bps
- ReFIT-Kalman: **>5 bps**
- Task completion time halved

**Jarosiewicz et al. 2015** applied ReFIT to paralyzed human patients (BrainGate):
- Allowed stable use for over 2 years
- No need for re-calibration
- Daily home use is feasible

## 5. CLDA: Closed-Loop Decoder Adaptation

**CLDA (Closed-Loop Decoder Adaptation)** generalizes the ReFIT idea — **the decoder keeps learning during closed-loop use**.

### SmoothBatch

**Orsborn et al. 2014 Neuron** proposed **SmoothBatch**:
- Update the decoder every 100–500 ms using the latest [y_t, x_t^intent]
- Smooth parameter updates (to avoid oscillation)
- User and decoder **co-evolve**

This is the embryonic form of **co-adaptation**.

### RAX / OFC

**Shanechi et al. 2016** modeled CLDA as an **optimal-feedback-control** problem:
- The user is an OFC (Optimal Feedback Control) agent
- The decoder learns to help the user's OFC policy reach the target
- Mathematically, this is a **bi-level optimization**

## 6. Limitations and Extensions of ReFIT

### Scenarios Where the Assumption Fails

ReFIT assumes "the user always moves toward the target," but:
- Users may take detours, pause, or change their mind
- Complex tasks (drawing, opening a door) have no clear "target"
- Free-form movement (no target) cannot apply ReFIT

### Directions for Extension

**Dangi et al. 2013** proposed using the **fastest path** rather than a straight line as intent;
**Jarosiewicz et al. 2013** introduced **intent-uncertainty** weighting;
**Zhang et al. 2018** replaced ReFIT with **reinforcement learning**, letting user intent emerge automatically via reward.

## 7. ReFIT and Modern Deep-Learning Decoders

ReFIT's core idea — **assume the user did it right + closed-loop recalibration** — reappears in modern deep-learning BCI in new forms:

### Self-Supervised Alignment

**CEBRA / LFADS** do not require explicit intent labels; they learn a latent space automatically via **behavioral contrastive learning** or **reconstruction loss**.

### Online Fine-Tuning

**NDT3 + online fine-tune**: fine-tunes with current-session data every few minutes; latent-state alignment lets new sessions avoid re-calibration from scratch.

### Human-in-the-Loop RL

**Willett 2023 speech BCI** introduced **user re-utterance** correction — the user can repeat a misrecognized word, and the model learns the fix.

## 8. ReFIT's Long-Term Impact on BCI Engineering

ReFIT demonstrated three important lessons:

1. **"Intent labels" in training data carry more information than "behavior labels"**
2. **Closed-loop use is itself training data** — static datasets are insufficient
3. **Users and decoders co-evolve** — the system must be designed to support this dynamic process

These lessons laid the engineering cornerstones for all modern BCI calibration.

## 9. State-of-the-Art Online Calibration Today

### Neuralink PRIME (2024–2025)

Noland Arbaugh's electrode threads retracted post-op → only ~15% of electrodes remained usable. Neuralink:
- **Latent-space remapping**: project remaining channels' activity back to the original latent space
- **Online ReFIT variant**: continuously fine-tune during daily use
- **User adaptation**: the user learns to use fewer channels

Final ITR stayed >8 bps, demonstrating that **modern CLDA can compensate for electrode failure**.

### Willett 2023 Speech BCI

- 10 minutes of calibration at the start of each session
- **HMM + RNN** two-layer training
- User re-utterance of misrecognized words as fine-tune data

## 10. Logical Chain

1. **Classical Kalman training data has a problem** — the observation task's distribution does not match closed-loop intent.
2. **ReFIT's "intent relabeling"** uses the "user moves toward the target" prior to correct the data.
3. **CLDA/SmoothBatch generalizes ReFIT to continuous online adaptation**.
4. **ReFIT shows that closed-loop use is itself a training signal** — this is the starting point of BCI co-adaptation.
5. **Modern deep-learning BCI carries the ReFIT spirit forward**: self-supervised alignment + online fine-tuning + user-in-the-loop learning.

## References

- Gilja et al. (2012). *A high-performance neural prosthesis enabled by control algorithm design.* Nat Neurosci. https://www.nature.com/articles/nn.3265
- Jarosiewicz et al. (2015). *Virtual typing by people with tetraplegia using a self-calibrating intracortical brain-computer interface.* Sci Transl Med.
- Orsborn et al. (2014). *Closed-loop decoder adaptation shapes neural plasticity for skillful neuroprosthetic control.* Neuron. https://www.cell.com/neuron/fulltext/S0896-6273(14)00871-1
- Shanechi et al. (2016). *Rapid control and feedback rates enhance neuroprosthetic control.* Nat Commun.
- Dangi et al. (2013). *Design and analysis of closed-loop decoder adaptation algorithms for brain-machine interfaces.* Neural Comp.
