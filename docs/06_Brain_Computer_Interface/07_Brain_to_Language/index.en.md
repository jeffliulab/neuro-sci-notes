# Brain-to-Language Decoding

Translating neural activity directly into text or speech is the most exciting direction in BCI — and the one where LLMs have had the deepest impact. In 2023, Willett et al. published a 62 WPM speech BCI in Nature, and Metzger et al. that same year gave voice, through a digital avatar, to a patient who had been paralyzed for 18 years. These achievements are the direct fruit of combining Transformer decoders with large language model post-processing.

**Where this chapter sits.** It is the concrete instantiation of the I2A paradigm from Chapter 06 on the "language" channel, and the chapter where LLMs reach deepest into the BCI decoding stack. Unlike Chapter 06, the output here is not motor action but a discrete symbol sequence — which lets LLMs play a role analogous to ASR post-processing in the BCI stack: **an RNN / Transformer maps neural activity to phoneme posteriors, an n-gram LM imposes initial constraints, and a GPT-class model does final rescoring**. This pipeline is why text decoding on the non-invasive EEG / MEG side has begun to have engineering relevance since 2023.

**Recommended reading order.** Start with *Invasive Speech BCI* (the three milestones **Moses 2021 NEJM → Willett 2023 Nature → Metzger 2023 avatar** are essential) to grasp the invasive-side SOTA. Then *Handwriting Decoding* shows how Willett 2021 used an "imagined handwriting → 90 CPM" trick to achieve high-bandwidth output from very few electrodes. Next, *Non-invasive Brain-to-Text* contrasts the non-invasive state of the art: Meta MEG / DeWave / EEGPT / MEGFormer remain far below the invasive WER, but **already decode at the semantic level offline**. Finally, *LLM Post-processing Fusion* ties together the engineering composition of the full decoding stack.

**Chapter contents:**

- **[Invasive Speech BCI](侵入式语音BCI.md)** — Moses 2021 NEJM, Willett 2023 Nature, Metzger 2023 avatar
- **[Handwriting Decoding](手写解码.md)** — Willett 2021 Nature; imagined handwriting → 90 CPM
- **[Non-invasive Brain-to-Text](非侵入式脑-文本.md)** — Meta Défossez MEG (2023), DeWave, EEGPT, MEGFormer
- **[LLM Post-processing Fusion](LLM后处理融合.md)** — RNN-transducer + n-gram LM + GPT rescoring
