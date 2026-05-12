# LLM 后处理融合

**LLM 后处理（LLM post-processing / rescoring）** 是现代语音/手写 BCI 的核心组件。BCI 直接输出音素或字符概率，**LLM 把"可能的候选"重新排序为"最合理的句子"**。这一技术是 Willett 2023 从 WER 23% 降到 9.1% 的关键，也是 BCI × LLM 合流的最具体表现。

## 一、为什么 BCI 需要 LLM

### BCI 信号的先天限制

- Spike 噪声：每个 20 ms bin 有真实发放率波动
- ECoG 空间模糊：相邻电极串扰
- EEG：颅骨滤波

**直接解码的准确率天花板** ~70–80%。要达到"自然对话"水平（>95%），必须有**外部先验**。

### LLM 作为强先验

LLM 知道：
- **词表合法性**："xqzr" 不是词
- **语法**：主谓宾
- **常识**：咖啡**热** vs 冰
- **上下文**：前一句在讨论啥

这些先验让 LLM 能从**多个候选**中选出**最可能正确**的。

## 二、rescoring 的三个级别

### 级别 1：n-gram LM

最简单：3-gram 计数 + Kneser-Ney 平滑。

$$P(\text{word}_t | \text{word}_{t-2}, \text{word}_{t-1})$$

- **Willett 2023** 用 3-gram 做 first-pass
- **速度**：极快，<10 ms
- **效果**：词级错误减少 30%

### 级别 2：神经 LM（GPT-2 级）

GPT-2 为每个候选打对数似然：

$$\log P(\text{sentence}) = \sum_t \log P(\text{word}_t | \text{context})$$

- **Willett 2023** 用 GPT-2 做 second-pass rescoring
- **延迟**：~100 ms（小模型）
- **效果**：WER 23% → 9.1%

### 级别 3：大 LLM (GPT-4 / Claude)

把 Top-K 候选发给 LLM 让它选：

```
Prompt: "Based on the context '...previous sentence...', 
which of these is most likely?
A. 'the weather is nice today'
B. 'the weatherice night today'
C. 'he whether is night today'
Select A, B, or C."
```

- **效果**：接近 WER 0% for common phrases
- **延迟**：500 ms+（API 调用）
- **实时性差**——适合 offline 或延迟容忍场景

## 三、Beam search + LM 的经典公式

现代 BCI 用**beam search**结合神经声学模型 + LM：

$$\text{score}(w) = \lambda_1 \log P_{\text{neural}}(w) + \lambda_2 \log P_{\text{LM}}(w) + \lambda_3 \cdot |w|$$

- $P_{\text{neural}}$: BCI decoder 的输出概率
- $P_{\text{LM}}$: LM 概率
- $|w|$: 长度惩罚（防止过短）
- $\lambda_{1,2,3}$: 权重（通常在验证集上调）

Beam size 典型 **50–500**——越大越准，但越慢。

## 四、两阶段解码架构

Willett 2023 和 Card 2024 的共同设计：

```
阶段 1: RNN + CTC → 音素概率
阶段 2a: CTC beam search + 词 LM → Top-K 句子候选
阶段 2b: GPT-2 rescoring → 选最好
```

这样设计的关键优点：
- 阶段 1 快（实时）
- 阶段 2 精（但用户需 wait）
- 流式 + 精调的组合

## 五、LLM 对话层

在 **BCI + LLM + 机器人**（见 [分层规划_BCI_LLM_机器人](../06_Intention_to_Action/分层规划_BCI_LLM_机器人.md)）中，LLM 不只是 rescoring——还扮演**对话管理**角色：

### 澄清

用户说 "拿东西"。LLM：
> "您想拿什么？A. 水 B. 手机 C. 遥控器"

用户 BCI 选 A。

### 补全

用户说 "给我..." → LLM 补全 "...一杯水？是/否"

用户点头（BCI detect）→ 执行。

### 记忆

LLM 记住用户偏好、日常模式：
- "您早上通常想要咖啡。要来一杯吗？"
- 减少重复意图输入

## 六、实时性优化

大 LLM rescoring 的延迟问题解决方案：

### 1. 预测性解码

LLM 持续处理**部分输出**，不等用户说完：
- 用户说 "the w..."
- LLM 已在预测 "weather", "water", "wolf"
- 收到下一个音素 "e"，排除 "wolf"

### 2. 推测解码（Speculative Decoding）

小模型快速生成，大模型验证/修正：
- 小模型 2-token/ms
- 大模型每 4 token 一次验证
- **3–5× 加速**

### 3. 边缘 LLM

- Llama-3 8B / Phi-3 3.8B 本地运行
- 延迟 50–100 ms
- 云端 GPT-4 作为 fallback

### 4. Caching

高频短语的 BCI → 文本映射缓存——日常对话大部分是重复模板。

## 七、LLM 辅助的学习

LLM 不只在推理时帮助 BCI，在训练时也：

### 自动数据扩增

LLM 生成类似训练句子——扩展 BCI 训练数据。

### 主动学习

LLM 识别 BCI 解码不确定的样本 → 请用户明确 → 强化学习。

### 用户建模

LLM 学习用户的**说话风格**（正式/随意、词汇偏好）——个性化 rescoring。

## 八、LLM 融合的风险

### 1. 过度修正

LLM 可能**替用户发声**——用户想说"冰水"，LLM 因为常见偏向"热咖啡"。

### 2. 意图偏移

LLM 可能"理解错"用户 → 改变意图。

### 3. 隐私

LLM API 调用意味着**用户思想被云端处理**——见 [神经权利](../13_Ethics_Neurorights/index.md)。

### 4. 对齐

LLM 可能拒绝某些输出（敏感话题）——瘫痪患者是否仍有**完整言论自由**？

## 九、开源 + 闭源方案

| 方案 | 模型 | 优缺点 |
| --- | --- | --- |
| **GPT-4 API** | 闭源 | 最强但隐私、延迟 |
| **Claude API** | 闭源 | 强，对齐好 |
| **Llama-3 local** | 开源 | 隐私好，性能略低 |
| **Phi-3 3.8B** | 开源 | 边缘设备可行 |
| **SLM 定制** | 自训练 | BCI 专用，需数据 |

**BCI 公司的选择**：
- **Neuralink**：目前用开源边缘 LLM + 可选云端
- **Synchron**：与 OpenAI 合作
- **Precision**：未公开

## 十、从 rescoring 到 generation

下一步：**让 LLM 不只是 rescoring，而是直接生成输出**。

### Neural embedding as soft prompt

- BCI → neural embedding (CEBRA, NDT3)
- Embedding 作为 LLM 输入
- LLM 直接生成文本

### 端到端训练

- Neural + text 成对数据
- 联合训练神经编码器 + LLM
- **NeuroLM（2024）** 是这方向早期尝试

这让 BCI 从"神经 → 文本"的**离散**解码变成**连续**生成——类似从 ASR 转向 diffusion-based 语音。

## 十一、逻辑链

1. **BCI 直接解码准确率天花板 ~80%**，需要 LLM 补偿。
2. **n-gram → GPT-2 → GPT-4** 三级 rescoring 逐级提升。
3. **Beam search + LM 加权** 是现代 BCI 解码标准。
4. **LLM 在对话层**扩展 BCI 功能——澄清、补全、记忆。
5. **实时性优化**（推测解码、边缘 LLM）让大 LLM 可用。
6. **LLM 融合带来新风险**——过度修正、隐私、言论自由。
7. **未来方向**：神经 embedding 作为 LLM 输入的端到端融合。

## 参考文献

- Willett et al. (2023). *A high-performance speech neuroprosthesis.* Nature. — GPT-2 rescoring
- Bourlard & Morgan (1994). *Connectionist speech recognition: a hybrid approach.* Springer. — ASR + LM 经典
- Leviathan et al. (2022). *Fast inference from transformers via speculative decoding.* arXiv. — speculative decoding
- Willett et al. (2024). Follow-up on BCI + LM integration. NEJM.
- Metzger et al. (2023). *A high-performance neuroprosthesis for speech decoding and avatar control.* Nature. — multi-stream LLM fusion
