# 侵入式语音 BCI

**侵入式语音 BCI**（invasive speech BCI）是 2020–2025 最活跃的 BCI 子领域，目标是让失语患者（ALS、脑干中风、闭锁综合征）恢复自然对话。从 Moses 2021（15 WPM）到 Willett 2023（62 WPM）到 Card 2024（UC Davis）——三年内性能 3 倍提升，正在逼近自然对话速度。

## 一、语音神经基础

### 语音相关脑区

- **vSMC（ventral sensorimotor cortex）**：发音器官（嘴、舌、喉）的运动编码
- **dPCG（dorsal precentral gyrus）**：手区附近，惊人地编码**言语相关肌肉活动**
- **Broca 区（Brodmann 44/45）**：语言产生
- **颞上回（STG）**：语音感知

### 发音 vs 想象语音

- **发音 BCI**：解码**实际说话**的神经活动
- **想象 BCI**：解码"想说但没说"（无声语音）
- 目前主流：**发音尝试**（attempted speech）——患者尝试说，但无声，神经信号仍清晰

## 二、UCSF Moses 2021（15 WPM）

**Moses et al. (2021, NEJM)**：

- **被试**：Pancho（anarthria 脑干中风 18 年）
- **电极**：128 通道 **高密度 ECoG**（硬膜下，vSMC）
- **解码**：
  - RNN 解码**词级**
  - 词表 50
  - HMM + 统计语言模型

### 性能

- **15 WPM**
- 词表 50 的 **>90% 准确率**
- **每天实时使用**—— Pancho 首次与家人"对话"

### 意义

- **首次证明 ECoG 可做实时语音 BCI**
- 词级解码 + LM 的可行性
- **18 年失语后仍能恢复言语意图**——神经活动未消失

## 三、Willett 2023（62 WPM）

**Willett, Kunz et al. (2023, Nature)**：

- **被试**：Pat Bennett（ALS）
- **电极**：4 × Utah Array = **256 通道 spike**（vSMC + dPCG）
- **解码**：
  - 双 RNN（local + global）
  - CTC 输出**音素**
  - 3-gram LM + GPT-2 rescoring

### 架构细节

```
Spike rates (256 ch, 20 ms bins)
  ↓
Input RNN (per-area)
  ↓
Main RNN
  ↓
Phoneme logits (41 phonemes + blank)
  ↓
CTC decoding
  ↓
Beam search + 3-gram LM
  ↓
GPT-2 rescoring
  ↓
Text
```

### 性能

- **62 WPM**（自然说话 ~150 WPM）
- 词表 125,000（全英语）
- **WER 9.1%**
- 训练数据：~10,000 句

### 意义

- 跨越"有用水平"：超过自然对话 1/3 速度
- **LM rescoring 提升 50%** WER（从 23% → 9.1%）
- Utah spike > ECoG 证明了通道密度的价值

### 关键工程教训

1. **dPCG + vSMC 双区域比单 vSMC 好**——语言是**分布式**的
2. **Spike > LFP > ECoG**——越精细越好
3. **Phoneme > Word** 解码更灵活（OOV 词可拼）
4. **LM 不可或缺**

## 四、UCSF Metzger 2023（Avatar，78 WPM）

**Metzger et al. (2023, Nature)**：

- **被试**：Ann（脑干中风 18 年）
- **电极**：253 通道 **高密度 ECoG**（Paradromics-style）
- **输出**：
  - 文字 + 语音合成（Ann 婚礼前录音）
  - 虚拟 avatar 面部表情

### 性能

- **78 WPM**
- 词表 1024
- Avatar 表情同步

### 创新

- **三路并行解码**：文字、语音、面部肌肉
- 语音合成用**患者本人年轻时声音**
- Avatar 让 BCI 输出具有**情感维度**

## 五、UC Davis Card 2024（256 WPM 里程碑）

**Card et al. (2024, NEJM)**：

- **被试**：Casey Harrell（ALS）
- **电极**：4 × Utah = 256 通道
- **特点**：
  - 更大词表（125K 全英语）
  - **实时 WPM 峰值 256**
  - 平均 **62 WPM WER 3%**

### 意义

- **WER 3% 接近人类水平**
- 证明"Willett 方法"可复现且**持续改进**
- UC Davis 成为 UCSF 之外的第二个 speech BCI 中心

## 六、关键技术组件

### Spike Sorting（或跳过）

现代 Speech BCI 多用 **threshold crossings**（TCR）而非 spike sorting——深度学习直接从 bin 的 spike 计数学习。

### 特征

- **Spike rate**（20 ms bins）
- **High-γ power**（80–200 Hz LFP 包络）
- 两者并用性能最好

### 模型

- **Willett**: 双 RNN + CTC
- **Moses**: 基于 GRU 的 word classifier
- **Metzger**: 基于 Transformer + multi-task

### 词表策略

- **Closed vocabulary**（Moses）：快速但受限
- **Open vocabulary with phonemes**（Willett）：慢一点但无限
- **Hybrid**：常用词直接解码 + 稀有词 fallback 到音素

## 七、LM 在 Speech BCI 中的作用

Language Model 是语音 BCI 的**核心放大器**：

### 阶段 1：解码

神经 → 音素概率（每 20 ms）

### 阶段 2：Beam search + n-gram LM

把音素概率 × LM 似然

$$P(\text{word sequence}) = P_{\text{acoustic}}(\text{phonemes}) \cdot P_{\text{LM}}(\text{words})$$

### 阶段 3：神经 LM rescoring

Top-K 候选送入 GPT-2/GPT-4 重新打分：

- Willett: GPT-2 rescoring 把 WER 从 23% 降到 9.1%
- 未来: 直接用 GPT-4 / Claude

### 理论

BCI 信号信噪比低——**LM 的先验极大改进**。这与 ASR（语音识别）的历史完全一致：声学模型 + LM 的组合是 ASR 成功的关键。

## 八、延迟 & 实时性

Speech BCI 延迟构成：

- 神经信号采集：20 ms
- 预处理 + 特征：10 ms
- 神经网络推理：30–50 ms
- LM beam search：50 ms
- **总**：~100–150 ms

与自然对话（100–300 ms 反应）**可接受**。但 GPT-4 rescoring 会加 500 ms+——需要 latency-aware rescoring。

## 九、多语言与跨语系

大部分 Speech BCI 是**英语**。挑战：

- **中文**：声调、字符 vs 拼音
- **韩文**：音节
- **日语**：假名 + 汉字
- **手语**：完全不同模态

**Ma et al. 2024**（复旦 / 清华）首个中文侵入式 Speech BCI，证明方法可迁移但**词库、LM 必须本地化**。

## 十、与非侵入式的对比

| | 非侵入（MEG/EEG） | 侵入（ECoG/spike） |
| --- | --- | --- |
| 顶尖性能 | ~5 WPM（MEG） | 62+ WPM |
| 词表 | 小（100–1000） | 大（125K） |
| 手术 | 无 | 开颅 |
| 适用患者 | 一般 | ALS、脑干中风 |

侵入式仍是失语 BCI **目前唯一实用** 的方案。非侵入式最终能否赶上？详见 [非侵入式脑-文本](非侵入式脑-文本.md)。

## 十一、逻辑链

1. **Moses 2021** 证明 ECoG 能做实时语音 BCI（15 WPM）。
2. **Willett 2023** 用 Utah spike + LM rescoring 达到 62 WPM——临界点。
3. **Metzger 2023 Avatar** 把输出扩展到**语音 + 表情**。
4. **Card 2024** 证明方法可复现，WER 降到 3%。
5. **LM 是 Speech BCI 性能倍增器**——未来会用到 GPT-4/Claude 级别。
6. **多语言 + 跨语系** 是 2025 后的主要扩展方向。

## 参考文献

- Moses et al. (2021). *Neuroprosthesis for decoding speech in a paralyzed person with anarthria.* NEJM. https://www.nejm.org/doi/full/10.1056/NEJMoa2027540
- Willett et al. (2023). *A high-performance speech neuroprosthesis.* Nature.
- Metzger et al. (2023). *A high-performance neuroprosthesis for speech decoding and avatar control.* Nature.
- Card et al. (2024). *An accurate and rapidly calibrating speech neuroprosthesis.* NEJM.
- Anumanchipalli et al. (2019). *Speech synthesis from neural decoding of spoken sentences.* Nature.
