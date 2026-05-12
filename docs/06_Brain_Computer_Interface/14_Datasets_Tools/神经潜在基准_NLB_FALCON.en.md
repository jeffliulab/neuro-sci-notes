# Neural Latents Benchmark: NLB and FALCON

**Neural benchmarks** are the "**ImageNet**" of the BCI × deep learning world. **NLB (Neural Latents Benchmark) 2021** + **FALCON (NeurIPS 2024)** define **standardized evaluation protocols** that enable cross-paper comparison of **neural foundation models** (NDT3, POYO, CEBRA).

## 1. Why Benchmarks Are Needed

### Fragmentation in the BCI Field

Before 2020:
- Each lab had **its own data**
- Different tasks, different metrics
- **Papers hard to compare**
- Progress **hard to measure**

### Value of Benchmarks

- **Unified evaluation:** defining tasks + metrics
- **Public leaderboards:** driving competition
- **Reproducibility:** verifiable
- **New methods surfaced**

ImageNet 2012 → the deep-learning explosion in CV. BCI needs the same.

## 2. NLB (Neural Latents Benchmark) 2021

### Background

- Pei, Ye et al. (2021, NeurIPS Datasets)
- Driven by Chethan Pandarinath, Mackenzie Mathis, Eva Dyer, and others
- The first comprehensive BCI benchmark

### Datasets

- **MC_Maze:** monkey delayed-reach task
- **MC_RTT:** monkey random-target reach
- **Area2_Bump:** monkey sensory + motor
- **DMFC_RSG:** time estimation
- **4 datasets, 36 hours of neural data** in total

### Tasks

**Given a time window of neural spikes, predict:**
- Future spikes (self-supervised)
- Behavior (kinematics)
- Latent variables

### Main Metrics

- **co-bps (co-smoothing bits per spike):** future-spike prediction
- **vel R²:** velocity prediction
- **FP R²:** forward prediction

### Submission

- Predictions submitted to EvalAI
- Public leaderboard
- Active 2022-2024

## 3. Evolution of the NLB Leaderboard

### Early 2022

- Baselines: GLM / LSTM / LFADS
- Best **co-bps ~0.3**

### 2023

- **NDT2** introduced
- co-bps rose to **~0.4**
- Transformers showed their advantage

### 2024

- **NDT3, POYO** foundation models
- co-bps **~0.5**
- Cross-subject pretraining plays a significant role

### Expected 2025+

- Continued increase
- Saturation point **hard to predict**

## 4. FALCON (2024 NeurIPS)

### Background

- **Foundation Animal LLM Cross-ObservatioN**
- 2024 NeurIPS Datasets Track
- Driven by Dyer, Mathis, Kording, and others

### Datasets

Larger and more diverse:
- **H1:** human handwriting (Willett 2021 data)
- **M1, M2:** monkey motor
- **B1:** bird / rodent

Total **~800 hours of neural data** — **20×** NLB.

### Tasks

- **"Few-shot calibration":** pretrain → rapidly calibrate on small amounts of data from a new subject/task
- Simulates the **clinical setting** (BCI cannot afford 10 hours of data every session)

### Metrics

- R² on held-out behavior
- Calibration efficiency
- Cross-subject transfer

### Significance

- **Tests true capability** of neural foundation models
- Cross-subject + cross-task **no longer assumed**
- **Clinically relevant** design

## 5. NLB vs FALCON

| | NLB | FALCON |
| --- | --- | --- |
| Year | 2021 | 2024 |
| Data | 36 hours | 800 hours |
| Species | Monkeys | Monkey + human + bird |
| Tasks | Fixed | Multi + few-shot |
| Metric | co-bps | Calibration efficiency |
| Focus | Decoding | **Transfer** |

FALCON is **NLB's foundation-model-era evolution**.

## 6. Other Benchmarks

### BCI Competitions

- **BCI IV, V:** EEG classics
- **EEG decoding** standards
- Web platform: bbci.de/competition

### IBL (International Brain Laboratory)

- Standardized rodent electrophysiology
- Multi-center collaboration
- **22 PIs, 10 countries**
- Shared data + behavior

### DANDI Archive

- Neural data repository
- NWB standard
- All NLB data live here

### HCP, NSD

- Human fMRI
- **NSD** is the ImageNet of MindEye
- Open-source

### Allen Institute

- Comprehensive mouse brain data
- Brain Observatory
- Ecosystem learning

## 7. Data Standards

### NWB (Neurodata Without Borders)

- **NWB 2.0** standard
- HDF5-based
- Cross-lab compatible

### BIDS

- Neuroimaging standard
- fMRI, EEG, MEG

### Why It Matters

- Data reuse
- Metadata + experimental setup
- **Reduces "garbage data data"** problems

## 8. Human BCI Data

### Limited Openness

- Medical data restricted by **HIPAA**
- Most labs **cautious about sharing**

### Open-Source Examples

- **Willett 2021 handwriting:** partially open-sourced
- **DIDI:** BrainGate data sharing
- **Physionet:** open-source EEG

### Challenges

- Patient informed consent
- De-identification (but brain data is identifiable — see [Brain Data Privacy & Cognitive Biometrics](../13_Ethics_Neurorights/脑数据隐私与认知生物计量.md))
- **Ethics review**

## 9. Tooling Support

### Evaluation Tools

- **EvalAI:** main NLB platform
- **HuggingFace Datasets:** FALCON
- **Papers With Code:** integrated rankings

### How to Participate

- Register an account
- Download data
- Train models
- Submit predictions
- Leaderboard auto-updates

### Code

- Baselines are **open-source**
- New methods can be forked

## 10. Impact

### On Research

- **Method evolution** is standardized and recorded
- **Value of foundation models** confirmed
- Cross-lab **dialogue**

### On Industry

- **Precision, Paradromics** use NLB-pretrained models
- **Neuralink** undisclosed but likely internal use
- **Synchron** may collaborate with academia

### On Education

- Students can **participate immediately**
- Lowers the BCI entry barrier
- **Accelerates talent development**

## 11. Limitations

### 1. Data Scale

- Even FALCON's 800 hours
- vs ImageNet's 140M images
- **Data bottleneck** persists

### 2. Experimental Setup

- Simple reaching tasks dominate
- **Few** free, naturalistic behaviors
- Real-world BCI use is **more complex**

### 3. Scarce Human Data

- Mainly monkey, rodent
- Clinical transfer is **indirect**

### 4. Metric Limitations

- R² / co-bps do not equal **clinical utility**
- New metrics needed (e.g., usability)

## 12. Future Directions

### 1. Larger Benchmarks

- **FALCON v2** expected
- Target 10,000+ hours
- Multi-species + multi-task

### 2. Closed-Loop Evaluation

- **Online BCI performance**
- Subjective user experience
- Beyond **offline** R²

### 3. Clinical Benchmarks

- **HIPAA-compliant** clinical data
- Real-patient validation
- Federated learning framework

### 4. Multimodal

- Neural + behavior + environment
- **"World model" benchmarks**
- Toward embodied intelligence

## 13. Logical Chain

1. **Benchmarks are the ImageNet moment for BCI deep learning.**
2. **NLB 2021** is the first comprehensive benchmark, with unified metrics like co-bps.
3. **FALCON 2024** scales 20×, focusing on **few-shot transfer**.
4. **NWB, BIDS, DANDI** form the data-standards ecosystem.
5. **Human BCI data** is limited by HIPAA / ethics, with little open-sourcing.
6. **Industry + academia** both use the benchmarks to drive progress.
7. **Future:** larger, more realistic, closed-loop, multimodal benchmarks.

## References

- Pei et al. (2021). *Neural Latents Benchmark '21: evaluating latent variable models of neural population activity.* NeurIPS Datasets. https://neurallatents.github.io/
- Karpowicz et al. (2024). *FALCON benchmark.* NeurIPS 2024. https://snel-repo.github.io/falcon/
- Teeters et al. (2015). *Neurodata Without Borders: Creating a Common Data Format for Neurophysiology.* Neuron.
- International Brain Laboratory (2021). *Standardized and reproducible measurement of decision-making in mice.* eLife.
- DANDI Archive. https://dandiarchive.org
