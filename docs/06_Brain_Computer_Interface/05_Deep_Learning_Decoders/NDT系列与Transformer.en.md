# NDT Series and Transformer

The **NDT (Neural Data Transformer)** series, released by **Joel Ye** and colleagues between 2021 and 2024, brought the Transformer architecture to neural signal decoding. NDT3 (NeurIPS 2024) marks the critical leap to a **neural foundation model** — a general-purpose BCI foundation model pretrained on 200+ datasets and 500+ hours of data.

## 1. NDT1: BERT for Neurons

**Ye & Pandarinath (2021, NeurIPS)** introduced NDT1:

### Setup

- Input: a spike-rate time series (T, N) — T time steps, N neurons
- Architecture: a standard Transformer encoder
- Task: **masked modeling** — randomly mask time steps and reconstruct

### Comparison with LFADS

| | LFADS | NDT1 |
| --- | --- | --- |
| Structure | VAE + GRU | Transformer |
| Training | Reconstruction ELBO | Masked reconstruction |
| Inference | Limited latent dimension | Whole sequence |
| Performance (NLB) | Good | **Better** |

NDT1 surpasses LFADS on the **Neural Latents Benchmark** — **Transformers are more suited to neural data than RNN-VAEs**.

## 2. NDT2: Cross-Session Alignment

**Ye et al. (2023, BioRxiv → ICLR 2024)** NDT2 tackles the **cross-session channel-misalignment** problem:

### The Problem

"Neuron #42" from the same Utah array across different sessions may not be the same neuron (electrode drift, neuron loss). A naive model **treats each session as a different dataset**.

### The Solution

- **One embedding per channel**: similar to BERT's token embedding
- **Session embedding**: each session gets a learnable vector
- **Context learning**: a small amount of data suffices for fast alignment

### Significance

NDT2 shows that **one model can work across sessions and subjects** — the key prerequisite for a neural foundation model.

## 3. NDT3: Neural Foundation Model

**Azabou & Ye et al. (2024, NeurIPS) NDT3** is a landmark:

### Scale

- **200+ datasets** (BrainGate, Pitt, Shenoy lab, etc.)
- **500+ hours** of electrophysiology data
- **100+ subjects** (monkeys, humans)
- **1B+ parameters** in a Transformer

### Architectural Innovations

- **PerceiverIO-like cross-attention**, fixed-size latent
- **Unit tokenization**: one query per unit, naturally supporting variable-length inputs
- **Rotary position embedding** for the time axis

### Pretrain + Fine-tune

- **Pretraining**: masked autoencoding
- **Fine-tuning**: small amounts of labeled data adapt to specific tasks (gesture, speech, cursor)
- **Zero-shot**: even a new subject can be decoded reasonably without fine-tuning

### Performance

- 10 minutes of data from a new subject reaches the performance of hours of traditional methods
- Zero-shot across tasks: a gesture model transfers directly to cursor tasks
- SOTA on Neural Latents and FALCON benchmarks

NDT3 is **BCI's GPT-3 moment**.

## 4. Why Transformers Excel for BCI

Why are Transformers well-suited to neural data?

1. **Variable-length sequences**: both spike times and neuron counts can vary
2. **Long-range dependencies**: decision and preparation periods can span 500+ ms
3. **Multi-modal fusion**: spikes + LFP + behavioral variables can all be tokenized
4. **Cross-task reuse**: pretrained features generalize across many tasks
5. **Scaling effects**: more data → better performance, following the scaling laws

## 5. Other Neural Transformers

### BrainBERT (Wang 2023)

The ECoG version of BERT: masked spectrogram prediction, learning cross-task-useful ECoG representations.

### Neuroformer (Antoniades 2023)

A multi-modal (vision + neural) autoregressive Transformer predicting neural activity + animal behavior.

### POYO (Azabou 2023 NeurIPS)

A unified "cross-dataset + cross-subject" architecture — see [Neural Foundation Model POYO](神经基础模型_POYO.md).

### MAE for EEG

**EEGPT** (Pu 2024): Masked-Autoencoder-style EEG pretraining.

## 6. Neural Transformers vs. Language Transformers

| Aspect | Language | Neural |
| --- | --- | --- |
| Token | word/subword | spike / bin / channel |
| Vocabulary | Fixed (50K) | Infinite (continuous) |
| Context | 1K–1M | 0.5–10 s |
| Data scale | TB | GB (growing) |
| Cross-distribution | Natural | Requires channel alignment |

The biggest difference: **language has labels (the next token is the label), while neural data requires behavior/task labels or a self-supervised objective**.

## 7. Key Architectural Design Choices

Design lessons from NDT3 and successors:

### Tokenization

- **Per-unit**: one token per neuron (variable length)
- **Per-bin**: one token per time bin (fixed window)
- **Hybrid**: mixing both dimensions

Per-unit is more flexible but increases sequence length; most modern models use a hybrid.

### Positional Encoding

- **Absolute**: suits fixed tasks
- **RoPE (Rotary)**: suits variable-length, cross-task use
- **Learnable per-session**: compensates for session differences

### Attention

- **Standard**: quadratic complexity, suited to short sequences
- **Linear attention / Flash**: for long sequences
- **Cross-attention (Perceiver)**: fixed latent size, good scalability

## 8. Online Deployment

Transformer latency challenges:

- Self-attention is $O(T^2)$
- 500 ms window + 10 ms bin = T=50 is still manageable
- Long windows (2 s+) require flash-attention / KV cache

**NDT3 online inference**: streaming + rolling window, 10 ms latency achievable.

## 9. Commercial Implications of the NDT Series

**Neural foundation models** let BCI follow a "pretrain + fine-tune" pathway like NLP:

- **Neuralink, Synchron**: pretrain themselves or use open-source foundation models
- **Small/mid BCI companies**: no need to train from scratch — download from HuggingFace-style "BrainHubs"
- **Researchers**: a few hundred examples suffice to build a useful BCI

This shifts BCI from "every lab develops on its own" to a "collaborative community ecosystem."

## 10. Logical Chain

1. **NDT1 demonstrates Transformer > RNN on neural data**, displacing LFADS's VAE-GRU.
2. **NDT2 solves cross-session channel alignment**, breaking the limits of single-session models.
3. **NDT3 pretrains on 500+ hours of data**, opening the BCI foundation-model era.
4. **Transformers' variable-length, long-dependency, and scalable nature** are particularly well-matched to neural data.
5. **NDT3 = BCI's GPT-3 moment** — collaborative community, cross-subject transfer, zero-shot adaptation.

## References

- Ye & Pandarinath (2021). *Representation learning for neural population activity with Neural Data Transformers.* NeurIPS.
- Ye et al. (2024). *A unified framework for neural decoding with pretrained transformers (NDT2).* ICLR.
- Azabou, Ye et al. (2024). *NDT3: A foundation model for neural data.* NeurIPS. https://arxiv.org/abs/2407.14668
- Wang et al. (2023). *BrainBERT: self-supervised representation learning for intracranial recordings.* ICLR. https://openreview.net/forum?id=xmcYx_reUn6
- Antoniades et al. (2024). *Neuroformer: multimodal and multitask generative pretraining for brain data.* ICLR.
