# NDT 系列与 Transformer

**NDT（Neural Data Transformer）** 系列由 **Joel Ye** 等人在 2021–2024 年间推出，把 Transformer 架构引入神经信号解码，并在 NDT3（2024 NeurIPS）完成了**神经基础模型**的关键跨越——在 200+ 数据集、500+ 小时数据上预训练的通用 BCI 基础模型。

## 一、NDT1：BERT for Neurons

**Ye & Pandarinath (2021, NeurIPS)** 提出 NDT1：

### 设置

- 输入：spike rate 时间序列 (T, N) — T 时间步、N 神经元
- 架构：标准 Transformer encoder
- 任务：**masked modeling**——遮住随机时间步，重构

### 与 LFADS 比较

| | LFADS | NDT1 |
| --- | --- | --- |
| 结构 | VAE + GRU | Transformer |
| 训练 | 重构 ELBO | Masked reconstruction |
| 推断 | 有限潜维度 | 整个序列 |
| 性能（NLB） | 好 | **更好** |

NDT1 在 **Neural Latents Benchmark** 上超过 LFADS——**Transformer 比 RNN-VAE 更适合神经数据**。

## 二、NDT2：跨 session 对齐

**Ye et al. (2023, BioRxiv → ICLR 2024)** 的 NDT2 解决**跨 session 通道不对齐**问题：

### 问题

同一 Utah 阵列在不同 session 记录的"神经元 #42"可能不是同一个（电极漂移、神经元丢失）。直接模型会**把 session 认成不同 dataset**。

### 解决方案

- **每个通道一个 embedding**：类似 BERT 的 token embedding
- **Session embedding**：给每个 session 加一个可学习向量
- **Context learning**：少量数据就能快速对齐

### 意义

NDT2 证明：**一个模型可以跨 session、跨被试使用**——这是神经基础模型的关键前提。

## 三、NDT3：神经基础模型

**Azabou & Ye et al. (2024, NeurIPS)** 的 **NDT3** 是里程碑：

### 规模

- **200+ 数据集**（BrainGate、Pitt、Shenoy 实验室等）
- **500+ 小时** 电生理数据
- **100+ 被试**（猴子、人类）
- **1B+ 参数** Transformer

### 架构创新

- **PerceiverIO** 类 cross-attention，固定大小 latent
- **单位分词（unit tokenization）**：每个 unit 一个 query，自然支持变长输入
- **Rotary position embedding** 适应时间轴

### 预训练 + 微调

- **预训练**：masked autoencoding
- **微调**：少量标注数据适配特定任务（手势、语音、光标）
- **零样本**：甚至新被试无需微调即可合理解码

### 性能

- 新被试 10 分钟数据达到传统方法数小时的性能
- 跨任务零样本：手势模型直接迁移光标任务
- 在 Neural Latents 和 FALCON 基准上 SOTA

NDT3 是 **BCI 的 GPT-3 moment**。

## 四、Transformer 在 BCI 上的优势

为什么 Transformer 适合神经数据？

1. **变长序列**：spike 时间 + 神经元数量都可变
2. **长距离依赖**：决策、准备期可跨 500+ ms
3. **多模态融合**：spike + LFP + 行为变量都可 tokenize
4. **跨任务复用**：预训练特征在多任务通用
5. **规模效应**：更多数据 → 更好性能，遵循 scaling law

## 五、其他神经 Transformer

### BrainBERT (Wang 2023)

ECoG 版 BERT：masked spectrogram prediction，学到跨任务有用的 ECoG 表征。

### Neuroformer (Antoniades 2023)

多模态（vision + neural）自回归 Transformer，预测神经活动 + 动物行为。

### POYO (Azabou 2023 NeurIPS)

"跨数据集 + 跨被试" 统一架构——见 [神经基础模型_POYO](神经基础模型_POYO.md)。

### MAE for EEG

**EEGPT**（Pu 2024）：Masked Autoencoder 风格的 EEG 预训练。

## 六、神经 Transformer vs 语言 Transformer

| 方面 | 语言 | 神经 |
| --- | --- | --- |
| Token | word/subword | spike / bin / channel |
| 词表 | 固定（50K） | 无限（连续值） |
| 上下文 | 1K–1M | 0.5–10 s |
| 数据规模 | TB | GB（增长中） |
| 跨分布 | 自然成立 | 需要通道对齐 |

差别最大的是：**语言有标签（下一个 token 就是标签），神经数据需要行为/任务标签或自监督设计**。

## 七、架构设计的关键选择

NDT3 及后续模型的设计教训：

### Tokenization

- **Per-unit**：每个神经元一个 token（可变长度）
- **Per-bin**：每个时间 bin 一个 token（固定窗口）
- **Hybrid**：两维混合

Per-unit 更灵活但增加序列长度；多数现代模型用 hybrid。

### Positional Encoding

- **Absolute**：适合固定任务
- **RoPE (Rotary)**：适合变长、跨任务
- **Learnable per-session**：补偿 session 差异

### Attention

- **Standard**：二次复杂度，适合短序列
- **Linear attention / Flash**：长序列
- **Cross-attention (Perceiver)**：固定 latent 大小，扩展性好

## 八、在线部署

Transformer 延迟挑战：

- 自注意力 $O(T^2)$
- 500 ms 窗口 + 10 ms bin = T=50 尚可
- 长窗口（2 s+）需要 flash-attention / KV cache

**NDT3 在线推理**：流式处理 + 滚动窗口，10 ms 延迟可达。

## 九、NDT 系列的商业含义

**神经基础模型**让 BCI 像 NLP 一样走"预训练 + 微调"路线：

- **Neuralink、Synchron**：自己预训练或用开源基础模型
- **中小 BCI 公司**：无需从头训练，可在 HuggingFace 风格的"BrainHub"下载
- **研究者**：几百条数据就能做出有用 BCI

这将 BCI 从"每实验室独立研发"转向"社区协作生态"。

## 十、逻辑链

1. **NDT1 证明 Transformer > RNN 在神经数据上**，取代 LFADS 的 VAE-GRU。
2. **NDT2 解决跨 session 通道对齐**，打破单 session 模型的局限。
3. **NDT3 用 500+ 小时数据预训练**，开启 BCI 基础模型时代。
4. **Transformer 的变长 + 长依赖 + 可扩展性**特别适合神经数据。
5. **NDT3 = BCI 的 GPT-3 moment**——社区协作、跨被试迁移、零样本适配。

## 参考文献

- Ye & Pandarinath (2021). *Representation learning for neural population activity with Neural Data Transformers.* NeurIPS.
- Ye et al. (2024). *A unified framework for neural decoding with pretrained transformers (NDT2).* ICLR.
- Azabou, Ye et al. (2024). *NDT3: A foundation model for neural data.* NeurIPS. https://arxiv.org/abs/2407.14668
- Wang et al. (2023). *BrainBERT: self-supervised representation learning for intracranial recordings.* ICLR. https://openreview.net/forum?id=xmcYx_reUn6
- Antoniades et al. (2024). *Neuroformer: multimodal and multitask generative pretraining for brain data.* ICLR.
