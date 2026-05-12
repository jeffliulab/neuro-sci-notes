# Neural Foundation Model POYO

**POYO (Azabou et al., 2023 NeurIPS)** is the first neural foundation model pretrained at scale **across datasets and across subjects**, and together with **NDT3 (2024)** inaugurated the foundation-model era for neural BCI. Structurally it parallels NLP's transition from BERT to GPT-3.

## 1. The "Pretrain-Fine-tune" Paradigm for Neural Data

Lessons from NLP:
- BERT/GPT pretrain on massive unsupervised text
- Downstream tasks need only small-scale labeled fine-tuning
- Performance + generalization + cross-task transfer

**The neural-data counterpart**:
- Pretraining: large amounts of unsupervised spike/LFP data
- Fine-tuning: small amounts of labels (gesture, cursor, speech)
- Goal: transfer **across subjects / tasks / recording modalities**

POYO was the first model to make this paradigm actually work.

## 2. POYO Architecture

### Core Design

```
Input: {(unit_i, time_j, spike_count)} — sparse tokens

① Per-unit Embedding
② Cross-attention (PerceiverIO-style)
   Query: fixed latent bank (e.g., 256)
   Key/Value: input spike tokens
③ Several layers of latent self-attention
④ Task head (swappable)
```

### Key Innovations

1. **Spike-as-token**: each spike is one token (unit, time), akin to words
2. **PerceiverIO**: fixed latent size, **decoupled from input length** — supports variable-length data
3. **Rotary position encoding**: for the time axis
4. **Per-unit embedding**: each neuron gets an independent vector, allowing cross-session alignment

## 3. Training Data and Scale

**POYO-1** (2023):
- ~160 hours of electrophysiology
- 40+ task types
- 27 subjects (mostly monkeys)

**POYO+** (2024):
- 500+ hours
- Multiple animal species + humans
- Cross-modal (spike + LFP + behavior)

## 4. Experimental Results

### Zero-Shot Transfer

On **unseen subjects**, POYO's zero-shot decoding accuracy reaches 65–80% (vs. 40% trained from scratch).

### Few-Shot Fine-Tuning

After fine-tuning with just **5 minutes** of data from a new subject, POYO beats the baseline trained from scratch on 30 minutes of data.

### Cross-Task Transfer

Pretrained on monkey gesture data, transferred to handwriting → outperforms direct training on handwriting data.

## 5. POYO vs. NDT3

| | POYO | NDT3 |
| --- | --- | --- |
| Year | 2023 NeurIPS | 2024 NeurIPS |
| Scale | ~160 h | ~500 h |
| Architecture | PerceiverIO | Perceiver + extensions |
| Tokenization | per-spike | per-unit-bin |
| Multi-modal | Limited | Complete |
| Open source | Partial | Released 2024 |

The two are **sister works** (Azabou is a shared lead author) — POYO lays the groundwork; NDT3 extends it.

## 6. Other Neural Foundation Models

### BrainBERT (Wang 2023)

An ECoG-specific foundation model using masked prediction.

### Neuroformer (Antoniades 2024)

Vision + neural multi-modal pretraining.

### EEGPT (Pu 2024)

EEG foundation model with millions of pretraining examples.

### LaBraM (Jiang 2024, ICLR)

Large Brain Model, VQ discrete tokenization + Transformer, cross-dataset EEG pretraining.

### BFM (Brain Foundation Model, 2024 arXiv Survey)

A survey reviewing 10+ neural foundation-model works from 2023–2024.

## 7. Why Foundation Models Work on Neural Data

Although neural recordings vary enormously in **channel count, subjects, and tasks**, they share deep common structure:

1. **Biological similarity**: motor and visual cortex are functionally similar across human and monkey brains
2. **Conserved manifold geometry**: cross-subject neural manifolds are similarly shaped (Gallego 2020, mentioned in Chapter 02)
3. **Reusable task structure**: vision-motor-attention processes share cross-task commonalities
4. **No ceiling on self-supervision**: as long as spike data are available, pretraining is possible

These let foundation models learn **truly shared computational representations** in "dirty data" environments.

## 8. Downstream Tasks of Foundation Models

POYO / NDT3 foundation models can serve many downstream tasks:

1. **Motor decoding**: cursor, robotic arm
2. **Speech decoding**: ECoG version
3. **Brain-to-language**: linked with LLMs
4. **Cognitive state**: fatigue, attention, error monitoring
5. **Stimulus design**: inverse — generate stimuli from a target percept

One pretrained model + multiple task heads = **a platform BCI system**.

## 9. Scaling Laws for Neural Foundation Models

Preliminary observations (NDT3, POYO+) suggest:

- **Data doubling → error ~-20%** (akin to NLP's Chinchilla law)
- **Parameter doubling → error ~-15%**
- **10× downstream task data** → substantial fine-tune gains

**Conclusion**: BCI foundation models are still in the early scaling phase, and large-scale pretraining is expected to keep pushing SOTA over the **next five years**.

## 10. Open Challenges

1. **Ethics and data sharing**: neural data are highly sensitive; cross-institutional aggregation faces privacy hurdles
2. **Electrode heterogeneity**: Utah, Neuropixels, and Neuralink output in different formats, and unified tokenization is still being explored
3. **Closed-loop adaptation**: how pretrained foundation models can **keep learning** during user operation
4. **Interpretability**: foundation models are typically black boxes, but clinical use demands interpretability
5. **Safety**: large models can be attacked or misused — LLM alignment issues apply to BCI as well

## 11. Connection to Human-Like Intelligence

POYO / NDT3 and **JEPA / LLM** are philosophically aligned:

- **JEPA**: pretrained visual latent space → world model
- **NDT3**: pretrained neural latent space → "neural foundation model"
- **LLM**: pretrained language representation → general language capability

A shared theme: **large-scale self-supervision + task conditioning** — trading data for generality. See [Chapter 10 Link to Embodied Intelligence](../10_Embodied_Intelligence_Link/index.md).

## 12. Logical Chain

1. **NLP's pretrain-fine-tune paradigm inspires BCI** — but channel heterogeneity must be overcome.
2. **POYO uses PerceiverIO + per-unit embedding** to achieve cross-subject, cross-dataset pretraining.
3. **POYO+ / NDT3 scale to 500+ hours**, reaching the "neural GPT-3" scale.
4. **Foundation models substantially outperform traditional methods in zero-shot, few-shot, and cross-task settings**.
5. **Neural-data scaling laws are still in the early phase** — models are expected to keep improving over the next five years.

## References

- Azabou et al. (2023). *A unified, scalable framework for neural population decoding.* NeurIPS. https://arxiv.org/abs/2310.16046
- Azabou, Ye et al. (2024). *Multi-session, multi-task neural decoding from distinct cell-types and brain regions.* NeurIPS.
- Jiang et al. (2024). *Large Brain Model for learning generic representations with tremendous EEG data in BCI.* ICLR. https://openreview.net/forum?id=QzTpTRVtrP
- Wang et al. (2023). *BrainBERT: self-supervised representation learning for intracranial recordings.* ICLR.
- Brain Foundation Models Survey (2025). arXiv:2503.00580.
