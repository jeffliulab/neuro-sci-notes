# Brain-to-Image/Video Decoding

After brain-to-language, the second major frontier is **reconstructing the visual scenes a user sees or imagines from neural activity**. In 2023–2024, three key advances appeared: MindEye / MindEye2 used diffusion models to decode fMRI signals into high-fidelity images; MinD-Video reconstructed video from fMRI for the first time; and EEG2Video replicated the pipeline on the non-invasive side. At the same time, Fernández 2021 Science Advances let a blind patient see stable phosphenes for the first time via V1 microstimulation — the starting point of visual write-in.

**Relationship to Chapter 07.** Brain-to-language maps neural signals to discrete symbols; brain-to-vision maps them to a high-dimensional continuous space (pixels / latents). The diffusion-model boom supplies exactly the prior this needs: MindEye projects fMRI embeddings into the CLIP-aligned latent space of Stable Diffusion, so that "any fMRI voxel pattern can be turned into a plausible image with a diffusion prior." This chapter has the most direct tension with **Chapter 13 (Ethics)** — visual reconstruction can recover a user's private visual memories, and is one of the strongest motivations for neurorights legislation.

**Recommended reading order.** Start with *fMRI Image Reconstruction* and master the three trunks: **MindEye / MindEye2 / Takagi-Nishimoto Stable Diffusion**; then *Semantic Reconstruction* fills in Tang 2023 Nat Neuro's semantic-level decoding and the CLIP-latent route; *Brain-to-Video Decoding* extends along the time axis to video (**MinD-Video / EEG2Video NeurIPS 2024**); and finally, *Visual Cortex Prosthesis* turns to the write-in side and traces the engineering path toward high-resolution phosphenes after Fernández 2021 — this section is the visual counterpart to the sensory writing in Chapter 09.

**Chapter contents:**

- **[fMRI Image Reconstruction](fMRI图像重建.md)** — MindEye (ICLR 2023), MindEye2 (ICML 2024), Takagi-Nishimoto Stable Diffusion
- **[Semantic Reconstruction](语义重建.md)** — Tang 2023 Nat Neuro; MindDiffuser; CLIP latent-space decoding
- **[Brain-to-Video Decoding](脑-视频解码.md)** — MinD-Video, Kupershmidt self-supervised, EEG2Video NeurIPS 2024
- **[Visual Cortex Prosthesis](视觉皮层假体.md)** — Fernández 2021 Sci Adv; high-resolution phosphene roadmap
