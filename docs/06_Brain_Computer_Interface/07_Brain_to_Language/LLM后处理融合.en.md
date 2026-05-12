# LLM Post-processing Fusion

**LLM post-processing / rescoring** is a core component of modern speech and handwriting BCIs. The BCI outputs phoneme or character probabilities directly, and **the LLM re-ranks the "plausible candidates" into the "most reasonable sentence."** This is the key to Willett 2023's WER drop from 23% to 9.1%, and the most concrete form of the BCI × LLM convergence.

## 1. Why BCI Needs LLMs

### Inherent limits of BCI signals

- Spike noise: real firing rates fluctuate within every 20 ms bin
- ECoG spatial blur: crosstalk between adjacent electrodes
- EEG: skull filtering

**Accuracy ceiling of direct decoding** is around 70–80%. Reaching a "natural conversation" level (>95%) requires an **external prior**.

### LLM as a strong prior

An LLM knows:
- **Vocabulary legality**: "xqzr" is not a word
- **Grammar**: subject-verb-object
- **Common sense**: coffee is **hot** vs iced
- **Context**: what the previous sentence was about

These priors let the LLM pick the **most likely correct** option from **multiple candidates**.

## 2. Three Levels of Rescoring

### Level 1: n-gram LM

The simplest: 3-gram counts + Kneser-Ney smoothing.

$$P(\text{word}_t | \text{word}_{t-2}, \text{word}_{t-1})$$

- **Willett 2023** uses a 3-gram for the first pass
- **Speed**: extremely fast, <10 ms
- **Effect**: 30% reduction in word-level errors

### Level 2: neural LM (GPT-2 class)

GPT-2 scores each candidate with log-likelihood:

$$\log P(\text{sentence}) = \sum_t \log P(\text{word}_t | \text{context})$$

- **Willett 2023** uses GPT-2 for a second-pass rescoring
- **Latency**: ~100 ms (small model)
- **Effect**: WER 23% → 9.1%

### Level 3: large LLM (GPT-4 / Claude)

Send Top-K candidates to the LLM and let it choose:

```
Prompt: "Based on the context '...previous sentence...', 
which of these is most likely?
A. 'the weather is nice today'
B. 'the weatherice night today'
C. 'he whether is night today'
Select A, B, or C."
```

- **Effect**: near 0% WER for common phrases
- **Latency**: 500 ms+ (API call)
- **Poor real-time performance** — suitable for offline or latency-tolerant scenarios

## 3. Classic Beam Search + LM Formula

Modern BCIs combine a neural acoustic model with an LM via **beam search**:

$$\text{score}(w) = \lambda_1 \log P_{\text{neural}}(w) + \lambda_2 \log P_{\text{LM}}(w) + \lambda_3 \cdot |w|$$

- $P_{\text{neural}}$: output probability of the BCI decoder
- $P_{\text{LM}}$: LM probability
- $|w|$: length penalty (to prevent overly short outputs)
- $\lambda_{1,2,3}$: weights (typically tuned on a validation set)

Beam size is typically **50–500** — larger is more accurate but slower.

## 4. Two-Stage Decoding Architecture

The design shared by Willett 2023 and Card 2024:

```
Stage 1: RNN + CTC → phoneme probabilities
Stage 2a: CTC beam search + word LM → Top-K sentence candidates
Stage 2b: GPT-2 rescoring → pick the best
```

Key advantages of this design:
- Stage 1 is fast (real-time)
- Stage 2 is accurate (but the user has to wait a bit)
- A combination of streaming + refinement

## 5. LLM Dialogue Layer

In the **BCI + LLM + robot** stack (see [Hierarchical Planning — BCI, LLM, Robot](../06_Intention_to_Action/分层规划_BCI_LLM_机器人.md)), the LLM is more than a rescorer — it also plays the role of **dialogue manager**:

### Clarification

User says "get thing." LLM:
> "What do you want? A. Water  B. Phone  C. Remote"

User selects A via BCI.

### Completion

User says "Give me..." → LLM completes: "...a cup of water? Yes/No"

User nods (BCI-detected) → execute.

### Memory

LLM remembers user preferences and daily patterns:
- "You usually want coffee in the morning. Shall I get one?"
- Reduces repeated intent inputs

## 6. Real-Time Optimization

Solutions to the latency of large-LLM rescoring:

### 1. Predictive decoding

The LLM keeps processing **partial output** without waiting for the user to finish:
- User says "the w..."
- LLM is already predicting "weather," "water," "wolf"
- Upon receiving the next phoneme "e," it rules out "wolf"

### 2. Speculative decoding

Small model generates quickly, large model verifies and corrects:
- Small model: 2 tokens/ms
- Large model verifies every 4 tokens
- **3–5× speedup**

### 3. Edge LLM

- Llama-3 8B / Phi-3 3.8B run locally
- Latency 50–100 ms
- Cloud GPT-4 as fallback

### 4. Caching

BCI → text mappings for frequent phrases can be cached — most daily conversation is templated.

## 7. LLM-Assisted Learning

LLMs help the BCI not only at inference time but also during training:

### Automatic data augmentation

The LLM generates similar training sentences, expanding BCI training data.

### Active learning

The LLM flags samples the BCI decoded with uncertainty, asks the user to confirm, and feeds back for reinforcement.

### User modeling

The LLM learns the user's **speech style** (formal/casual, vocabulary preferences) for personalized rescoring.

## 8. Risks of LLM Fusion

### 1. Over-correction

The LLM may **speak for the user** — the user wanted to say "iced water" but the LLM, biased toward the common phrase, produces "hot coffee."

### 2. Intent drift

The LLM may "misunderstand" the user and shift intent.

### 3. Privacy

Calling an LLM API means **user thoughts are processed in the cloud** — see [Neurorights](../13_Ethics_Neurorights/index.md).

### 4. Alignment

LLMs may refuse certain outputs (sensitive topics) — does a paralyzed patient still have **full freedom of speech**?

## 9. Open-Source and Closed-Source Options

| Option | Model | Pros and cons |
| --- | --- | --- |
| **GPT-4 API** | Closed | Strongest, but privacy and latency concerns |
| **Claude API** | Closed | Strong, good alignment |
| **Llama-3 local** | Open | Better privacy, slightly lower performance |
| **Phi-3 3.8B** | Open | Feasible on edge devices |
| **Custom SLM** | Self-trained | BCI-specific, requires data |

**BCI companies' choices**:
- **Neuralink**: currently uses an open-source edge LLM + optional cloud
- **Synchron**: partners with OpenAI
- **Precision**: not publicly disclosed

## 10. From Rescoring to Generation

Next step: **let the LLM not merely rescore, but directly generate the output**.

### Neural embedding as soft prompt

- BCI → neural embedding (CEBRA, NDT3)
- Embedding fed into the LLM as input
- LLM directly generates text

### End-to-end training

- Paired neural + text data
- Joint training of the neural encoder + LLM
- **NeuroLM (2024)** is an early attempt in this direction

This turns BCI from **discrete** "neural → text" decoding into **continuous** generation — analogous to moving from ASR toward diffusion-based speech.

## 11. Logical Chain

1. **BCI direct-decoding accuracy is capped at ~80%** and needs LLMs to close the gap.
2. **n-gram → GPT-2 → GPT-4** is a three-stage rescoring hierarchy with progressive gains.
3. **Beam search + LM weighting** is the current standard for BCI decoding.
4. **LLMs at the dialogue layer** extend BCI functions — clarification, completion, memory.
5. **Real-time optimizations** (speculative decoding, edge LLMs) make large LLMs usable.
6. **LLM fusion introduces new risks** — over-correction, privacy, free speech.
7. **The future direction**: neural embeddings as LLM input in end-to-end fusion.

## References

- Willett et al. (2023). *A high-performance speech neuroprosthesis.* Nature. — GPT-2 rescoring
- Bourlard & Morgan (1994). *Connectionist speech recognition: a hybrid approach.* Springer. — the classic ASR + LM treatment
- Leviathan et al. (2022). *Fast inference from transformers via speculative decoding.* arXiv. — speculative decoding
- Willett et al. (2024). Follow-up on BCI + LM integration. NEJM.
- Metzger et al. (2023). *A high-performance neuroprosthesis for speech decoding and avatar control.* Nature. — multi-stream LLM fusion
