# 脑-图像/视频解码

继脑-语言之后的第二大前沿是：从神经活动中**重建用户所看、所想象的视觉场景**。2023–2024 年出现了三类关键进展：MindEye / MindEye2 用扩散模型把 fMRI 信号解码为高保真图像；MinD-Video 首次从 fMRI 重建视频；EEG2Video 在非侵入端复现了这一流程。与此同时，Fernández 2021 Science Advances 让盲人通过 V1 微刺激首次看到稳定的 phosphene——这是视觉写入的起点。

**和第 07 章的关系。** 脑-语言把神经信号映射到离散符号；脑-视觉把神经信号映射到高维连续空间（pixel / latent）。Diffusion 模型的爆发正好提供了 prior：MindEye 用 CLIP-aligned latent 把 fMRI 嵌入映射到 Stable Diffusion 的潜空间，让"任何 fMRI 体素都能借扩散先验产生合理图像"。这一章和**第 13 章伦理**有最直接的张力——视觉重建会重建用户的私密视觉记忆，是神经权利立法的最强动机之一。

**学习路径。** 先读「fMRI 图像重建」掌握 **MindEye / MindEye2 / Takagi-Nishimoto Stable Diffusion** 三条主干；再用「语义重建」补足 Tang 2023 Nat Neuro 的语义层级解码与 CLIP-latent 路线；接下来「脑-视频解码」沿时间轴扩展到视频（**MinD-Video / EEG2Video NeurIPS 2024**）；最后「视觉皮层假体」转向写入侧，理解 Fernández 2021 之后 phosphene 高分辨率化的工程路径——这一节是第 09 章感觉写入的视觉版本。

**本章内容：**

- **[fMRI 图像重建](fMRI图像重建.md)** — MindEye (ICLR 2023)、MindEye2 (ICML 2024)、Takagi-Nishimoto Stable Diffusion
- **[语义重建](语义重建.md)** — Tang 2023 Nat Neuro；MindDiffuser；CLIP 潜空间解码
- **[脑-视频解码](脑-视频解码.md)** — MinD-Video、Kupershmidt 自监督、EEG2Video NeurIPS 2024
- **[视觉皮层假体](视觉皮层假体.md)** — Fernández 2021 Sci Adv；phosphene 高分辨率化路径
