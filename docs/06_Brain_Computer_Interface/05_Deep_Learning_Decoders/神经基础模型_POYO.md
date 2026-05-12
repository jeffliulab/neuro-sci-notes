# 神经基础模型 POYO

**POYO（Azabou et al., 2023 NeurIPS）** 是第一个**大规模跨数据集、跨被试**预训练的神经基础模型，和 **NDT3（2024）** 共同开创了神经 BCI 的基础模型时代。这与 NLP 从 BERT 到 GPT-3 的跨越在结构上对等。

## 一、神经数据的"预训练-微调"范式

NLP 的启示：
- BERT/GPT 在海量无监督文本上预训练
- 下游任务只需少量标注微调
- 性能 + 泛化 + 跨任务迁移

**神经数据的对等**：
- 预训练：大量无监督 spike/LFP 数据
- 微调：少量标注（手势、光标、语音）
- 目标：**跨被试/跨任务/跨记录方式**迁移

POYO 是首个把这一范式做通的模型。

## 二、POYO 架构

### 核心设计

```
Input: {(unit_i, time_j, spike_count)} — 稀疏 tokens

① Per-unit Embedding
② Cross-attention (PerceiverIO 风格)
   Query: 固定 latent bank (如 256 个)
   Key/Value: 输入 spike tokens
③ Latent self-attention 若干层
④ Task head (可交换)
```

### 关键创新

1. **Spike-as-token**：每个 spike 是一个 token (unit, time)，类似 words
2. **PerceiverIO**：固定 latent 大小，**与输入长度解耦**——支持变长数据
3. **Rotary position encoding**：时间轴
4. **Per-unit embedding**：每个神经元独立向量，跨 session 可对齐

## 三、训练数据与规模

**POYO-1**（2023）:
- ~160 小时电生理
- 40+ 任务类型
- 27 个被试（猴子为主）

**POYO+**（2024）:
- 500+ 小时
- 多个动物种群 + 人类
- Cross-modal（spike + LFP + 行为）

## 四、实验结果

### 零样本迁移

在**未见过的被试**上，POYO 零样本解码精度可达 65–80%（相比从头训练 40%）。

### 少样本微调

新被试 **5 分钟数据** 微调后超过从头训练 30 分钟的基线。

### 跨任务迁移

在猴子手势数据上预训练，迁移到手写 → 性能优于直接在手写数据上训练。

## 五、POYO 与 NDT3 的比较

| | POYO | NDT3 |
| --- | --- | --- |
| 时间 | 2023 NeurIPS | 2024 NeurIPS |
| 规模 | ~160 h | ~500 h |
| 架构 | PerceiverIO | Perceiver + 扩展 |
| Tokenization | per-spike | per-unit-bin |
| 多模态 | 有限 | 完整 |
| 开源 | 部分 | 2024 释出 |

两者是**同源工作**（Azabou 是共同核心作者）——POYO 奠基，NDT3 进一步扩展。

## 六、其他神经基础模型

### BrainBERT (Wang 2023)

ECoG 专用基础模型，使用 masked 预测。

### Neuroformer (Antoniades 2024)

视觉 + 神经多模态预训练。

### EEGPT (Pu 2024)

EEG 基础模型，百万级预训练数据。

### LaBraM (Jiang 2024, ICLR)

Large Brain Model，VQ 离散 tokenization + Transformer，EEG 跨数据集预训练。

### BFM (Brain Foundation Model, 2024 arXiv 综述)

综述回顾 2023–2024 的 10+ 神经基础模型工作。

## 七、为什么基础模型能在神经数据上工作

尽管神经记录的**通道数、被试、任务**千差万别，共享一些深层结构：

1. **生物学相似**：人脑与猴脑的运动皮层、视觉皮层功能相近
2. **流形几何保守**：跨被试神经流形形状相似（第 02 章提到的 Gallego 2020）
3. **任务结构可复用**：视觉-运动-注意过程有跨任务共性
4. **自监督无上限**：只要有 spike 数据就能预训练

这些让基础模型得以在"脏数据"环境中学到**真正共享的计算表征**。

## 八、基础模型的下游任务

POYO / NDT3 基础模型可服务多种下游：

1. **运动解码**：光标、机械臂
2. **语音解码**：ECoG 版
3. **脑-语言**：和 LLM 联动
4. **认知状态**：疲劳、注意、错误监测
5. **刺激设计**：反向——从目标感知生成刺激

一个预训练模型 + 多个 task head = **平台化 BCI 系统**。

## 九、神经基础模型的 scaling law

初步观察（NDT3, POYO+）表明：

- **数据翻倍 → 误差 ~-20%**（类似 NLP 的 Chinchilla 法则）
- **参数翻倍 → 误差 ~-15%**
- **下游任务数据 10×** → fine-tune 性能显著提升

**结论**：BCI 基础模型还在 scaling 早期，预计**未来 5 年**大规模预训练将持续提升 SOTA。

## 十、开放挑战

1. **伦理与数据共享**：神经数据高度敏感，跨机构聚合有隐私障碍
2. **电极异质性**：Utah、Neuropixels、Neuralink 输出格式不同，统一 tokenization 仍在探索
3. **闭环适应**：预训练基础模型如何在用户使用中**持续学习**
4. **可解释性**：基础模型通常黑盒，临床需要可解释性
5. **安全性**：大模型可能被攻击、误用 —— LLM 的对齐问题在 BCI 上同样存在

## 十一、与类人智能的连接

POYO / NDT3 和 **JEPA / LLM** 在哲学上一致：

- **JEPA**：预训练的视觉潜空间 → 世界模型
- **NDT3**：预训练的神经潜空间 → "神经基础模型"
- **LLM**：预训练的语言表征 → 通用语言能力

共同的思想：**大规模自监督 + 任务条件化**——用数据换通用性。详见 [10 章 与具身智能的连接](../10_Embodied_Intelligence_Link/index.md)。

## 十二、逻辑链

1. **NLP 的预训练-微调范式启发 BCI**——但需要克服通道异质性。
2. **POYO 用 PerceiverIO + per-unit embedding** 实现跨被试、跨数据集预训练。
3. **POYO+ / NDT3 扩展到 500 小时级**，达到"神经 GPT-3"规模。
4. **基础模型在零样本、小样本、跨任务上显著优于传统方法**。
5. **神经数据的 scaling law 尚在早期**——未来 5 年模型仍将持续提升。

## 参考文献

- Azabou et al. (2023). *A unified, scalable framework for neural population decoding.* NeurIPS. https://arxiv.org/abs/2310.16046
- Azabou, Ye et al. (2024). *Multi-session, multi-task neural decoding from distinct cell-types and brain regions.* NeurIPS.
- Jiang et al. (2024). *Large Brain Model for learning generic representations with tremendous EEG data in BCI.* ICLR. https://openreview.net/forum?id=QzTpTRVtrP
- Wang et al. (2023). *BrainBERT: self-supervised representation learning for intracranial recordings.* ICLR.
- Brain Foundation Models Survey (2025). arXiv:2503.00580.
