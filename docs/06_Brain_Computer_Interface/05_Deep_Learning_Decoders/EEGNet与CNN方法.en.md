# EEGNet and CNN Methods

**EEGNet (Lawhern et al. 2018, J Neural Eng)** is the most representative deep-learning architecture in the EEG BCI field. It brought CNNs to EEG and, with **few parameters + carefully designed structure**, reached SOTA on multiple BCI paradigms — becoming the **baseline for non-invasive BCI deep learning**.

## 1. Why EEG Needs a Specialized CNN

Directly porting computer-vision ResNet/VGG to EEG does not work, for several reasons:

1. EEG is a **(channels, time)** 2D signal; the channel dimension lacks image-like spatial structure
2. Few electrodes (8–64), so data dimensionality × sample count is ≤ 1/1000 of typical CV
3. Requirements: interpretability, low latency, small samples

EEGNet addresses these challenges with three design choices.

## 2. EEGNet Architecture

```
Input: (channels C, time T) — e.g. (64, 256)

① Temporal Conv 1D (depthwise on time axis):
   F1 filters, kernel = (1, 64)      # learns frequency selectors
   → (F1, C, T)

② Depthwise Conv (spatial):
   D=2 filters per F1, kernel = (C, 1)  # learns spatial filters (per channel)
   → (F1*D, 1, T)

③ Separable Conv:
   kernel = (1, 16)                  # temporal-scale features
   → (F2, 1, T/4)

④ Global average + Dense softmax
```

Core ideas:
- **Temporal convolutions learn frequency bands**
- **Depthwise spatial convolutions learn CSP-like spatial patterns**
- **Separable convolutions reduce parameters**

Total parameters **~2000** — 10000× fewer than ResNet.

## 3. Key Design Choices

### Depthwise Separable

Separate temporal and spatial processing — **parameters reduced 10×**, avoiding overfitting.

### Small Receptive Field

Typical receptive field 0.25–1 s, matching EEG event timescales.

### BatchNorm + ELU

Compared with ReLU, ELU is more stable on small-data EEG.

### Regularization

- Dropout 0.5
- L2 regularization
- Data augmentation: time shifting, noise injection

## 4. Performance on BCI Tasks

The original EEGNet paper covered four tasks:

| Task | Dataset | EEGNet | Baseline |
| --- | --- | --- | --- |
| P300 | BCI Competition III | **89.5%** | xDAWN+SWLDA 88% |
| ERP | MRCP | **91%** | FBCSP 89% |
| Motor imagery | BCI IV-2a | **69.9%** | FBCSP 67% |
| Error perception | Chavarriaga 2014 | **79%** | CCA 77% |

**Matches or beats classical methods on every task** — the first demonstration that a general-purpose EEG CNN is feasible.

## 5. Variants of EEGNet

### DeepConvNet / ShallowConvNet

**Schirrmeister et al. 2017 Hum Brain Mapp** — earlier EEG CNNs:
- DeepConvNet: deeper (4 conv blocks)
- ShallowConvNet: FBCSP-like simulation (log + mean pooling)

### EEG-TCNet

**Ingolfsson 2020** added TCN (temporal convolutional network) to handle long dependencies.

### EEGSym

**Pérez-Velasco 2022** exploited left-right brain-symmetry priors for further gains.

### BENDR (Transformer)

**Kostas 2021** used contrastive pretraining + Transformer — entering the foundation-model era.

## 6. Invasive CNNs: A Different Philosophy

Invasive BCI (spike + LFP) CNN design differs from EEGNet:

### QRNN / WaveNet-Style

**Willett 2021 handwriting BCI** used a RNN + CNN hybrid:
- 1D conv on the time axis
- GRU for sequence modeling
- CTC output for characters

### LFP-CNN

**Eden-lab** and others use a 1D CNN to learn features directly from raw LFP, replacing hand-crafted band power.

### Spike Tokenization

**NDT3 and POYO** represent spikes as discrete tokens and use a Transformer (see [NDT Series and Transformer](NDT系列与Transformer.md)) — going beyond the pure CNN paradigm.

## 7. Engineering Impact of EEGNet

### Open Source and Community

- **Braindecode (Python)**: PyTorch EEGNet reference implementation
- **TorchEEG**: a more modern EEG deep-learning framework
- EEGNet is the default baseline in Kaggle EEG competitions

### Teaching Baseline

Almost every EEG deep-learning paper uses EEGNet as the baseline, making EEGNet the "last classical CNN" — akin to ResNet in CV.

## 8. Comparison with Transformer / Foundation Models

After 2023, EEG foundation models emerged (BENDR, EEGPT, LaBraM); pretrained on multiple datasets, they outperform EEGNet. But:

- EEGNet is still the best choice for **single-session, small-data** scenarios
- Training an EEGNet takes minutes; Transformer foundation models need GPU-days
- EEGNet's small parameter count makes it easy to deploy on embedded devices (consumer BCI)

**Tiered use**: embedded / consumer / real-time latency-sensitive → EEGNet; research / big data / cross-subject transfer → Transformer.

## 9. CNN Interpretability

EEGNet's depthwise spatial filters, when visualized, approximate CSP filters — **the deep network spontaneously learned features that neuroscientists hand-designed**.

**Grad-CAM / Integrated Gradients** can locate which time windows and frequency bands are most important for classification. This preserves EEGNet's place in clinical settings (which demand interpretability).

## 10. Logical Chain

1. **EEG BCI's small-data, high-dimensional nature** requires specialized CNN design and cannot reuse CV architectures directly.
2. **EEGNet's separable temporal-spatial convolutions** achieve extreme parameter efficiency, reaching SOTA with ~2000 parameters.
3. **EEGNet is the baseline for EEG deep learning** — all subsequent EEG papers must compare against it.
4. **Invasive BCI CNN design differs**: CTC + 1D conv for handwriting, Transformer-based for speech.
5. **Transformer foundation models surpass EEGNet**, but EEGNet still dominates resource-constrained settings.

## References

- Lawhern et al. (2018). *EEGNet: a compact convolutional neural network for EEG-based brain-computer interfaces.* J Neural Eng. https://iopscience.iop.org/article/10.1088/1741-2552/aace8c
- Schirrmeister et al. (2017). *Deep learning with convolutional neural networks for EEG decoding and visualization.* Hum Brain Mapp.
- Willett et al. (2021). *High-performance brain-to-text communication via handwriting.* Nature. — RNN+CNN handwriting
- Kostas et al. (2021). *BENDR: using transformers and a contrastive self-supervised learning task to learn from massive amounts of EEG data.* Front Hum Neurosci.
- Braindecode: https://braindecode.org/
