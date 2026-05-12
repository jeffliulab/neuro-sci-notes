# Open-source Tools: MNE, EEGLAB, CEBRA

**The open-source ecosystem for BCI research** is maturing rapidly. From **signal processing** (MNE-Python, EEGLAB), to **deep-learning decoders** (Braindecode, NDT, POYO), to **contrastive-learning frameworks** (CEBRA), open-source tools let any researcher reproduce a cutting-edge paper **within 24 hours**.

## 1. Signal Processing

### 1. MNE-Python

The **most mainstream** open-source EEG/MEG processing package.

#### Core

- Built on Python + NumPy
- Full MEG/EEG workflow
- **15+ years of active development**

#### Features

- Filtering, denoising
- ICA / artifact removal
- Source localization
- Statistical analysis
- Rich **KaTeX** documentation

#### Installation

```bash
pip install mne
```

#### Learning Resources

- Official site mne.tools
- 50+ tutorials
- Used in Harvard's neuroimaging courses

### 2. EEGLAB

The EEG standard in the **MATLAB** ecosystem.

#### Core

- MATLAB GUI-driven
- **Widely** used in academia
- Dozens of plugins

#### Advantages

- **Strong** visualization
- **Complete** standard algorithms
- First choice of old-school neuroscientists

#### Disadvantages

- MATLAB license
- Weak Python integration
- **Migration to MNE** trend

### 3. Brainstorm

- MATLAB/Python hybrid
- Strong on MEG
- Developed at McGill + USC

### 4. FieldTrip

- MATLAB
- Donders Institute, Netherlands
- Mainly MEG

### 5. BCILAB

- BCI extension of EEGLAB
- Real-time BCI support

## 2. Spike Processing

### 1. Kilosort

Standard of the **Neuropixels era**.

#### Development

- **Pachitariu, Stringer, Carandini** (UCL)
- Kilosort 4 is the latest version

#### Features

- GPU-accelerated spike sorting
- Template matching
- Automation

#### Usage

- 90%+ of academic research
- Open-source + documented

### 2. Wave_Clus

- Classic spike sorting
- Scala lab

### 3. MountainSort

- Automation
- Big data

### 4. SpikeInterface

- **Unified interface** for multiple sorters
- Python; integrates BrainBox and others

## 3. Deep-Learning Decoders

### 1. Braindecode

The **standard library for EEG deep learning**.

#### Core

- PyTorch-based
- EEGNet, DeepConvNet, Shallow
- Preprocessing + training + evaluation

#### Usage

```python
from braindecode.models import EEGNetv4
model = EEGNetv4(n_chans=22, n_outputs=4, n_times=1125)
```

### 2. NDT (Neural Data Transformer)

**NDT1/2/3** open-sourced on GitHub.

#### Usage

```python
from ndt3 import NDT3
model = NDT3.from_pretrained("snel-repo/ndt3-base")
```

### 3. POYO

**POYO (Azabou 2023)** foundation model.

- Multi-subject pretraining
- Fine-tune on new subject
- GitHub: https://github.com/poyo-brain

### 4. CEBRA

See [CEBRA and Contrastive Learning](../05_Deep_Learning_Decoders/CEBRA与对比学习.md).

#### Usage

```python
import cebra
cebra_model = cebra.CEBRA(model_architecture='offset10-model', 
                          batch_size=512,
                          learning_rate=3e-4,
                          output_dimension=8,
                          max_iterations=20000)
cebra_model.fit(neural_data, behavior)
embeddings = cebra_model.transform(neural_data)
```

- Developed at MPI-IS Tübingen
- **Open-sourced in Nature 2023**
- Complete documentation + tutorials

## 4. Brain-to-Image Reconstruction

### 1. MindEye / MindEye2

- Ryan Scotti et al.
- Open-sourced by the **MedARC** team
- Pretrained weights on HuggingFace

### 2. Brain-Diffuser

- Ozcelik 2023 open-sourced
- fMRI → SD pipeline

### 3. NeuroImagen

- Academic component
- Multi-model comparisons

## 5. Brain-to-Language

### 1. Willett 2023 Code

- Stanford NPL open-sourced
- PyTorch RNN + CTC
- GitHub

### 2. DeWave

- EEG → text
- Non-invasive baseline

### 3. EEGPT / LaBraM

- EEG foundation models
- GPT-like pretraining
- Open-sourced by Chinese labs

## 6. Data Storage + Visualization

### 1. NWB (Neurodata Without Borders)

- HDF5-based
- Cross-lab data standard
- PyNWB interface

### 2. DANDI

- NWB data repository
- 100+ datasets

### 3. Neo

- Python electrophysiology I/O
- Multi-format support

### 4. Plotly / Bokeh

- Interactive visualization
- Brain-map projection

### 5. Nilearn

- Neuroimaging ML
- fMRI analysis

## 7. Real-time BCI Systems

### 1. BCI2000

- **Classic** real-time BCI platform
- Wadsworth Center
- C++ + modular

### 2. OpenBCI GUI

- Open-source hardware + software
- Consumer EEG

### 3. LSL (Lab Streaming Layer)

- Real-time data-stream protocol
- Cross-device synchronization

### 4. OpenViBE

- Graphical BCI programming
- EU project

## 8. Machine-Learning Frameworks

### General

- **PyTorch:** standard for BCI deep learning
- **TensorFlow:** EEGNet etc. natively
- **JAX:** high-performance research

### Specialized

- **scikit-learn:** classical ML
- **Riemannian:** pyRiemann
- **CSP:** MNE + scikit

## 9. Cloud Collaboration

### 1. HuggingFace

- Pretrained model sharing
- CEBRA, MindEye, NDT, and others are all there

### 2. Weights & Biases

- Experiment tracking
- Widely used in BCI research

### 3. Neptune

- Another experiment tracker
- Similar to W&B

### 4. Colab / Kaggle

- Free GPU
- BCI entry point

## 10. Tutorials & Resources

### Books

- **"Brain Signal Analysis" (Sanei & Chambers)**
- **"Neural Engineering" (He)**
- **"Neurotechnology and Brain Machine Interface" (Kao & others)**

### Courses

- **NMA (Neuromatch Academy):** free online, multi-country
- **Stanford CME 290:** BCI course
- Multiple courses on **Coursera / edX**

### Documentation

- MNE tutorials
- CEBRA paper + GitHub
- ML for BCI review

## 11. Complete Pipeline Example

### EEG Classification Task

```python
import mne
from braindecode.models import EEGNetv4
import torch

# 1. Load + preprocess
raw = mne.io.read_raw_edf("data.edf")
raw.filter(1, 40)
epochs = mne.Epochs(raw, events, tmin=0, tmax=2)

# 2. ICA artifact removal
ica = mne.preprocessing.ICA(n_components=20)
ica.fit(raw)
ica.apply(raw)

# 3. Extract data
X = epochs.get_data()
y = epochs.events[:, -1]

# 4. Train EEGNet
model = EEGNetv4(n_chans=22, n_outputs=4, n_times=501)
# ... training code
```

**15 lines of code** for end-to-end.

## 12. Evaluation & Comparison

### Standard Datasets

- **PhysioNet BCI IV 2a** (EEG motor imagery)
- **CHB-MIT** (EEG epilepsy)
- **NSD** (fMRI images)
- **NLB / FALCON**

### Standard Metrics

- Classification accuracy
- R² for continuous
- WER for speech
- Kappa

## 13. Open-source Culture

### Why BCI Is Unique

- High hardware barrier
- Data privacy
- But **algorithms can be open-sourced**

### Community

- **BCI Society** annual meeting
- **NeurIPS / NAT NEURO** papers
- Twitter / Weibo BCI accounts

### Contribution

- Student → intern → job
- Open source is the **entry ticket**

## 14. Future

### 1. BCI × LLM Integration

- Open-source LLM + open-source BCI
- **Neural augmentation input** research

### 2. Real-time AI

- Edge LLM
- ONNX acceleration

### 3. Federated Learning

- Cross-hospital
- HIPAA-compliant

### 4. No-code

- Click-and-run
- Democratizing research

## 15. Logical Chain

1. **MNE + EEGLAB** are the foundation tools for signal processing.
2. **Kilosort** is the standard for spike sorting.
3. **Braindecode + NDT + POYO + CEBRA** are the deep-learning decoder ecosystem.
4. **MindEye + DeWave** are concrete brain-to-image/language applications.
5. **NWB + DANDI** are the data standard + repository.
6. **BCI2000 + LSL + OpenBCI** are real-time systems.
7. **Open-source culture + tutorials + community** make the BCI entry barrier lower than ever.

## References

- Gramfort et al. (2013). *MEG and EEG data analysis with MNE-Python.* Front Neurosci.
- Delorme & Makeig (2004). *EEGLAB: an open source toolbox for analysis of single-trial EEG dynamics.* J Neurosci Methods.
- Pachitariu et al. (2024). *Kilosort 4.* Nat Methods.
- Schirrmeister et al. (2017). *Deep learning with convolutional neural networks for EEG decoding and visualization.* Hum Brain Mapp.
- Schneider et al. (2023). *Learnable latent embeddings for joint behavioral and neural analysis.* Nature.
