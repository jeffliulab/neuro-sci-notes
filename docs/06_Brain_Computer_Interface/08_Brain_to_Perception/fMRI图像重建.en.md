# fMRI Image Reconstruction

**fMRI image reconstruction** is the most visually striking direction at the intersection of BCI × generative AI in 2022–2025: **generating the images a user sees directly from brain activity**. Within a year, MindEye, MindEye2, and Takagi-Nishimoto pushed reconstruction quality from "blurry category" to "near photo-realistic."

## 1. Core Principles

### What fMRI sees about vision

- **V1/V2/V3**: retinotopy — early visual features
- **V4**: color, shape
- **IT (inferior temporal)**: object recognition, semantics
- **Fusiform, PPA**: faces, scenes

Multi-voxel pattern analysis (MVPA) on fMRI can decode image information from the **BOLD pattern across the entire visual cortex**.

### The role of generative AI

Generative models such as **Stable Diffusion and CLIP** provide **strong priors** — even with noisy fMRI signals, as long as they point to the right semantics, the generative model can fill in the details.

## 2. Takagi-Nishimoto 2023 CVPR

**Takagi & Nishimoto (2023)** is the foundational work of modern fMRI image reconstruction.

### Data

- NSD (Natural Scenes Dataset)
- 8 subjects, ~10,000 COCO images per subject × fMRI

### Method

```
fMRI voxels 
  ├→ linear map → CLIP text embedding
  └→ linear map → CLIP image embedding
         ↓
    Stable Diffusion
         ↓
    Reconstructed image
```

- Uses **Ridge regression** to map fMRI → CLIP embedding
- Stable Diffusion generates the image from the CLIP embedding

### Results

- Reconstructs **high-quality natural images**
- Some details (color, object category) match the ground truth
- Certain reconstructions are **visually indistinguishable** from the originals

## 3. MindEye 2023 NeurIPS

**Scotti et al. (MindEye)** significantly improved on Takagi-Nishimoto:

### Key innovations

1. **MLP + Diffusion prior**: a stronger fMRI → CLIP mapping
2. **Multimodal alignment**: maps to both image embedding and text embedding simultaneously
3. **Test-time optimization**: iteratively refine the image

### Performance

- **Cross-subject training** + subject-specific fine-tuning
- **2× the reconstruction quality of Takagi** (LPIPS, CLIP-sim, etc.)

## 4. MindEye2 2024

**Scotti et al. (MindEye2, 2024)** is the milestone:

### Scale

- Trainable with just 1 hour of fMRI
- Open source + Hugging Face models

### Architecture

```
fMRI 
  ↓
Pretrained fMRI encoder (cross-subject)
  ↓
Diffusion prior
  ↓
CLIP embedding
  ↓
Stable Diffusion XL
  ↓
High-quality image
```

### Performance

- **Human-level image identification**:
- From 300 candidate images, identifies reconstruction → ground truth with 93% accuracy
- Detail quality approaches the original

### Significance

**"We can now reconstruct near-photograph-quality images from 1 hour of fMRI"** — the biggest leap in brain decoding in a decade.

## 5. Technology Stack Details

### CLIP as bridge

CLIP is the key component of fMRI image reconstruction:

- The CLIP image encoder maps images to 512/768 dimensions
- The fMRI learns to map into **the same space**
- Stable Diffusion / SDXL accept CLIP embeddings as conditioning

This turns fMRI decoding into **standard CLIP-guided generation**.

### Diffusion prior

```
fMRI → Diffusion prior network → CLIP embedding
```

Compared to a plain MLP, the diffusion prior models the distribution of CLIP embeddings, yielding more realistic reconstructions.

### Training data

**NSD (Natural Scenes Dataset)** is the ImageNet of this field:
- 8 subjects
- ~10,000 COCO images per subject
- 3 presentations per image
- High-quality 7T fMRI

Without NSD, much of this work would not have been possible.

## 6. Limitations

### 1. Subject-specific

Most methods require **10+ hours of data per subject** for training. Zero-shot across subjects still lags.

MindEye2 brings this down to 1 hour — still far from "plug and play."

### 2. Can only reconstruct what was seen

fMRI decoding is **associative** — visual concepts outside the training data are hard to decode.

### 3. Depends on natural-image priors

Stable Diffusion's "natural-image prior" makes reconstructions look good — but it may **exceed the actual content of the fMRI**. Reconstructions may be SD's "hallucination" rather than true decoding.

### 4. High latency

fMRI is slow (~1 s/scan) and cannot be real-time.

### 5. Semantic vs visual

fMRI reconstruction is **stable for high-level semantics (this is a dog)** but **weak for low-level visuals (fur color, posture)**.

## 7. Contrast with Tang's Semantic Reconstruction

**Tang 2023** (fMRI → language) expresses semantics in language; **Takagi/MindEye** (fMRI → image) express vision in images.

| | Tang | MindEye |
| --- | --- | --- |
| Output | Text | Image |
| Emphasis | Semantics | Vision |
| Brain region | Distributed | Visual cortex |
| Use case | Listening-to-story reconstruction | Image-viewing reconstruction |

The two may **merge** in the future — reconstructing **captioned images** from fMRI.

## 8. Extension to Brain-to-Video

**MinD-Video (Chen 2023)** extends this approach to **video**:

- fMRI time series → CLIP embedding sequence
- Video diffusion generation
- Reconstructs "videos that were watched"

See [Brain-to-Video Decoding](脑-视频解码.md).

## 9. Application Outlook

### Clinical

- **Visual prosthesis**: V1 cortical stimulation lets the blind "see" — this is the **inverse problem** (see [Visual Cortex Prosthesis](视觉皮层假体.md))
- **Amnesia diagnosis**: compare visual-reconstruction quality in healthy subjects vs patients

### Research

- Neuroscience: understanding how the visual cortex encodes
- AI research: generative models × biological vision

### Consumer (future)

- Dream recording (requires portable fMRI)
- Visually assisted creation
- Emotion visualization

### Risks

- **Privacy**: fMRI can reveal thoughts
- **Consent**: is passive scanning legal?
- See [Chapter 13 Ethics](../13_Ethics_Neurorights/index.md)

## 10. Open-Source Tools

- **[MindEye2](https://medarc-ai.github.io/mindeye2/)**: full code + pretrained models
- **NSD dataset**: publicly downloadable
- **Stable Diffusion / SDXL**: Hugging Face

Researchers can reproduce paper results within 24 hours — **low barriers have driven rapid progress**.

## 11. Logical Chain

1. **fMRI has high spatial resolution** and is well-suited for visual-cortex decoding.
2. **CLIP + Stable Diffusion** provide a unified pipeline from fMRI → image.
3. **Takagi 2023** opened the field; **MindEye2 2024** reached human-level identification.
4. **1 hour of fMRI is enough to train** — scales to more subjects.
5. **Limitations**: subject-specific training, can only reconstruct what was seen, semantics beats detail.
6. **Brain-to-video extension** has begun; the future direction is fusing text + image.

## References

- Takagi & Nishimoto (2023). *High-resolution image reconstruction with latent diffusion models from human brain activity.* CVPR. https://openaccess.thecvf.com/content/CVPR2023/html/Takagi_High-Resolution_Image_Reconstruction_With_Latent_Diffusion_Models_From_Human_Brain_CVPR_2023_paper.html
- Scotti et al. (2023). *Reconstructing the mind's eye: fMRI-to-image with contrastive learning and diffusion priors.* NeurIPS.
- Scotti et al. (2024). *MindEye2: Shared-subject models enable fMRI-to-image with 1 hour of data.* ICML. https://medarc-ai.github.io/mindeye2/
- Allen et al. (2022). *A massive 7T fMRI dataset to bridge cognitive neuroscience and artificial intelligence.* Nat Neurosci. — NSD
- Ozcelik & VanRullen (2023). *Brain-Diffuser: natural scene reconstruction from fMRI signals using generative latent diffusion.* Scientific Reports.
