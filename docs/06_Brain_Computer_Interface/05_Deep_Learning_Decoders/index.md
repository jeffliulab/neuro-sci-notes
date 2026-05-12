# 深度学习解码器

2018 年之后，深度学习全面接管 BCI 解码。本章按时间顺序展示这一转变：从 EEGNet 这样的任务专用 CNN，到 LFADS 这样的动力学建模，到 NDT / POYO / CEBRA 这样的神经基础模型。这条技术路径的终点是：一个在海量神经数据上预训练的大模型，能够**少样本迁移**到新被试、新任务。

**与第 04 章的关系。** 经典解码（第 04 章）解决的是"单个被试、单个任务"的精确解码；深度学习解码器解决的是"跨被试、跨任务、跨电极阵列"的泛化问题。这是 BCI 领域过去 5 年最大的范式转移：以往每个新患者都要重新校准几个月，现在 CEBRA / POYO 这类基础模型在猴和人之间、在不同 Utah 阵列之间、在 spike / LFP / ECoG 之间，都展示出了显著的零样本或少样本迁移能力。这条路径如果走通，BCI 就具备了 NLP 在 GPT-2 之后那种"一次预训练到处用"的工程化基础。

**学习路径。** 按"任务专用 → 动力学 → 跨域基础模型"三阶段读：先用「EEGNet 与 CNN 方法」入门紧凑型 CNN 在 EEG BCI 上的设计空间；然后用「LFADS 与动力学建模」理解 Pandarinath 2018 把潜在动力学引入解码的关键转折；接下来「NDT 系列与 Transformer」追踪 NDT1→2→3 的演进；「CEBRA 与对比学习」展示 behavior-aligned embedding 的优雅；最后用「神经基础模型 POYO」收尾，看 POYO+ / Neuroformer / BrainBERT 如何朝"神经版 GPT"的方向收敛。

**本章内容：**

- **[EEGNet 与 CNN 方法](EEGNet与CNN方法.md)** — 紧凑型 CNN 的设计空间；DeepConvNet / ShallowConvNet
- **[LFADS 与动力学建模](LFADS与动力学建模.md)** — Pandarinath 2018 Nature Methods；序列 VAE 捕捉潜在动力学
- **[NDT 系列与 Transformer](NDT系列与Transformer.md)** — NDT1 / NDT2 / NDT3 的演进
- **[CEBRA 与对比学习](CEBRA与对比学习.md)** — Mathis lab Nature 2023；behavior-aligned embedding
- **[神经基础模型 POYO](神经基础模型_POYO.md)** — POYO、POYO+、Neuroformer、BrainBERT 综述
