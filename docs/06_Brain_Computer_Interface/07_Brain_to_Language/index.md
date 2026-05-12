# 脑-语言解码

把神经活动直接翻译成文字或语音，是 BCI 领域最激动人心、也是 LLM 介入最深的方向。2023 年 Willett 等在 Nature 发表 62 WPM 语音 BCI、Metzger 同年让瘫痪 18 年的患者通过数字 avatar 发声——这些成就直接得益于 Transformer decoder + 大语言模型后处理的组合。

**这章的位置。** 它是第 06 章 I2A 范式在"语言"这一通道上的具体落地，也是 LLM 进入 BCI 解码栈最深的章节。和第 06 章不同：脑-语言解码的输出不是动作，而是离散符号序列；这恰好让 LLM 在 BCI 解码栈里承担起类似 ASR 后处理的角色——**RNN / Transformer 把神经活动映射到 phoneme posterior，n-gram LM 提供初步约束，GPT 类大模型做最终 rescoring**。这条路径让 2023 年之后非侵入端的 EEG / MEG 文本解码也开始有了工程意义。

**学习路径。** 建议从「侵入式语音 BCI」入门（**Moses 2021 NEJM → Willett 2023 Nature → Metzger 2023 avatar** 是必看的三步进展），先理解侵入端的 SOTA。然后用「手写解码」看 Willett 2021 的"想象手写 → 90 CPM"如何用极少电极完成高带宽输出。接下来用「非侵入式脑-文本」对照非侵入端的现状：Meta MEG / DeWave / EEGPT / MEGFormer 在 WER 上还远不如侵入端，但**离线条件下已能解码出语义层级**。最后通过「LLM 后处理融合」理解整条解码栈的工程组合。

**本章内容：**

- **[侵入式语音 BCI](侵入式语音BCI.md)** — Moses 2021 NEJM、Willett 2023 Nature、Metzger 2023 avatar
- **[手写解码](手写解码.md)** — Willett 2021 Nature；想象手写 → 90 CPM
- **[非侵入式脑-文本](非侵入式脑-文本.md)** — Meta Défossez MEG (2023)、DeWave、EEGPT、MEGFormer
- **[LLM 后处理融合](LLM后处理融合.md)** — RNN-transducer + n-gram LM + GPT rescoring
