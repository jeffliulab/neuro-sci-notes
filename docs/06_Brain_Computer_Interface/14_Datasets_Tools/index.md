# 数据集与工具

进入 BCI 研究的最直接路径是在公开数据集上跑通经典解码。本章汇集主要神经数据集、解码基准、开源工具链与学习资源，作为全章的实操后盾。

**怎么用本章。** 这一章是"上手清单"，不依赖前面 1–13 章学完，可以随时开。但配合前面读会更有效率：第 04、05 章学到一个解码方法 → 在「神经潜在基准 NLB / FALCON」找对应任务复现；第 03 章学到一种信号 → 在「开源工具 MNE / EEGLAB / CEBRA」找现成的预处理流水线；第 06、07 章想看真实管线 → 在「主要实验室与学习资源」里找原作者公开的代码与课程。整个 BCI 章节里所有能"动手做"的环节都汇集在这里。

**学习路径。** 完全的新手建议反过来读：先用「主要实验室与学习资源」选定一条路径（**Stanford NPL** 偏运动皮层与意图解码、**Caltech** 偏后顶叶与高维解码、**Pitt** 偏 ICMS 与具身、**BrainGate** 偏临床、**Shanechi** 偏 DPAD 与情绪 BCI），然后用「开源工具 MNE / EEGLAB / CEBRA」配齐工具链，最后用「神经潜在基准 NLB / FALCON」选一个具体任务作为入门项目。已经有方向的研究者直接从基准入手即可。

**本章内容：**

- **[神经潜在基准 NLB / FALCON](神经潜在基准_NLB_FALCON.md)** — Neural Latents Benchmark、FALCON NeurIPS 2024
- **[开源工具 MNE / EEGLAB / CEBRA](开源工具_MNE_EEGLAB_CEBRA.md)** — 预处理 + 解码代码库
- **[主要实验室与学习资源](主要实验室与学习资源.md)** — Stanford NPL、Caltech、Pitt、BrainGate、Shanechi 等
