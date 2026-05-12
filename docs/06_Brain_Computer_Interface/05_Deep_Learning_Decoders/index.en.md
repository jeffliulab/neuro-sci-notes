# Deep Learning Decoders

After 2018, deep learning took over BCI decoding across the board. This chapter presents the transition in chronological order: from task-specific CNNs like EEGNet, to dynamical modeling such as LFADS, to neural foundation models like NDT / POYO / CEBRA. The endpoint of this trajectory is a large model pre-trained on massive neural data that can **few-shot transfer** to new subjects and new tasks.

**Relationship to Chapter 04.** Classical decoding (Chapter 04) solves "precise decoding for one subject, one task." Deep-learning decoders solve "generalization across subjects, tasks, and electrode arrays." This is the biggest paradigm shift in BCI over the past five years: where every new patient used to require months of recalibration, foundation models such as CEBRA / POYO now show meaningful zero-shot or few-shot transfer between monkey and human, between different Utah arrays, and across spike / LFP / ECoG modalities. If this path matures, BCI gains the same "pre-train once, deploy everywhere" engineering footing that NLP gained after GPT-2.

**Recommended reading order.** Read in three stages — task-specific → dynamics → cross-domain foundation models: start with *EEGNet and CNN Methods* to enter the design space of compact CNNs on EEG BCI; then move to *LFADS and Dynamics Modeling* to understand the pivotal turn in Pandarinath 2018, which brought latent dynamics into decoding; next, *NDT Series and Transformer* tracks the evolution NDT1 → 2 → 3; *CEBRA and Contrastive Learning* showcases the elegance of behavior-aligned embedding; and finally, *Neural Foundation Model POYO* closes the chapter by surveying how POYO+ / Neuroformer / BrainBERT converge toward a "GPT for neural data."

**In this chapter:**

- **[EEGNet and CNN Methods](EEGNet与CNN方法.md)** — the design space of compact CNNs; DeepConvNet / ShallowConvNet
- **[LFADS and Dynamics Modeling](LFADS与动力学建模.md)** — Pandarinath 2018 Nature Methods; sequential VAE capturing latent dynamics
- **[NDT Series and Transformer](NDT系列与Transformer.md)** — the evolution of NDT1 / NDT2 / NDT3
- **[CEBRA and Contrastive Learning](CEBRA与对比学习.md)** — Mathis lab Nature 2023; behavior-aligned embedding
- **[Neural Foundation Model POYO](神经基础模型_POYO.md)** — survey of POYO, POYO+, Neuroformer, BrainBERT
