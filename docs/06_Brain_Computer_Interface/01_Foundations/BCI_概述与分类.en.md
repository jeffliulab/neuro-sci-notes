# BCI Overview and Classification

## 1. What Is a Brain-Computer Interface

A **Brain-Computer Interface (BCI)** is a class of systems that establish an information pathway directly between the brain and external devices, bypassing peripheral nerves and muscles. Its core definition contains three elements:

1. **Signal source:** Records from or stimulates the central nervous system (brain, spinal cord) directly — not peripheral physiological signals like EMG or eye movement.
2. **Information pathway:** Transmits information bidirectionally between brain and machine — decoding (reading out) or encoding (writing in) neural activity.
3. **Purposefulness:** Serves specific functions such as communication, control, sensory restoration, or neuromodulation — not pure recording.

A system belongs to the BCI category only if all three conditions hold. This definition excludes purely observational brain imaging (e.g., research fMRI scans) and peripheral-only devices (e.g., watch heart-rate + AI emotion inference).

---

## 2. Three-Dimensional Taxonomy

BCI systems can be classified along three orthogonal dimensions; together these dimensions capture all the key properties of a concrete BCI system.

### Dimension 1: Invasiveness

This is the most direct classification dimension, determining spatial resolution, SNR, and surgical risk of the signal.

| Category | Electrode location | Typical systems | Spatial resolution | Surgical risk |
| --- | --- | --- | --- | --- |
| **Fully invasive (Intracortical)** | Within cortex | Utah array, Neuropixels, Neuralink N1 | Single-neuron (~0.1 mm) | High |
| **Cortical surface (ECoG / sEEG)** | Subdural / cortical surface | Clinical sEEG, Precision Layer 7 | ~1 mm | Medium |
| **Minimally invasive** | Intravascular / epidural | Synchron Stentrode | ~few mm | Low |
| **Non-invasive** | Scalp | EEG, MEG, fMRI, fNIRS | cm (EEG) | None |

Signal quality increases monotonically along this sequence, but so does surgical risk. **How to trade off "signal quality" against "surgical invasiveness"** is the foundational problem of BCI engineering.

### Dimension 2: Signal Direction

| Direction | Definition | Typical applications |
| --- | --- | --- |
| **Read-out** | Decode intent or perception from neural activity | Cursor control, speech BCI, handwriting BCI |
| **Write-in** | Stimulate signals into the nervous system | Intracortical microstimulation (ICMS), visual prosthesis, DBS |
| **Bidirectional** | Simultaneous read and write | Flesher 2021 arm with tactile feedback, Ganzer 2020 Cell |

Traditional BCI focused on read-out; after 2016, "read-write closed-loop" represented by bidirectional BCI has become the new frontier (see [Chapter 09](../09_Sensory_Writing_Bidirectional/index.md)).

### Dimension 3: Application / Usage Scenario

| Category | User population | Representative systems | Regulatory pathway |
| --- | --- | --- | --- |
| **Clinical BCI** | Paralysis, ALS, blindness, and other disabilities | BrainGate, Neuralink, Synchron, Neuracle | FDA IDE / NMPA Class III medical device |
| **Neuromodulation** | Parkinson's, epilepsy, depression | Medtronic DBS, NeuroPace RNS | Multiple products on market |
| **Research BCI** | Animal experiments, human experiments | Neuropixels, university labs | IRB / IACUC |
| **Consumer BCI** | Healthy population | Muse, Emotiv, OpenBCI | FCC / CE |
| **Augmentation / AR BCI** | Future vision | No real commercial products yet | No regulatory framework |

This dimension best reflects the 2024-2026 changes: **clinical BCI is moving from "single trial" into "multiple companies, multiple countries, parallel commercialization."**

---

## 3. BCI Signal Hierarchy

Different acquisition methods observe signals at vastly different scales, which determines the upper bound of what can be decoded.

| Signal | Temporal resolution | Spatial resolution | Sampled object | Typical channel count |
| --- | --- | --- | --- | --- |
| **Spike (single-neuron action potential)** | 1 ms | Single cell | 10² neurons | 100–10k |
| **LFP (local field potential)** | 1 ms | 100 μm–1 mm | 10³ neuron populations | 100–1k |
| **ECoG** | 1 ms | 1 mm | 10⁴ neurons | 64–256 |
| **EEG** | 1 ms | 1 cm | 10⁶ neurons | 8–256 |
| **MEG** | 1 ms | 5 mm | 10⁶ neurons | ~300 |
| **fMRI** | ~1 s (BOLD delay) | 1–3 mm | 10⁶ neurons | ~10⁵ voxels |
| **fNIRS** | ~1 s | 1–3 cm | 10⁶ neurons | ~50 |

**Key pattern:** the higher the spatial resolution, the finer the decodable intent granularity. Spike-level signals can decode word-level speech, complex handwriting, and fine-grained finger movement; EEG-level signals typically only support discrete choices (e.g., P300 speller) or large-amplitude motor imagery.

---

## 4. Active vs Passive BCI

Following Zander & Kothe 2011's taxonomy, BCIs admit an orthogonal division by the degree of user-intent engagement:

- **Active BCI:** The user actively generates intent (imagined movement, attempted speech); the system decodes and controls external devices. This is the mode of most clinical BCIs.
- **Reactive BCI:** The system presents external stimuli; the user reacts passively; the system decodes the reaction signal. Typical examples are P300 spellers and SSVEP spellers.
- **Passive BCI:** The system monitors the user's cognitive or emotional state without any deliberate intent generation. Typical applications are fatigue detection and attention monitoring.

This distinction is crucial for product design — active BCI requires user training; passive BCI can run invisibly. Apple's AirPods EEG patents take the passive route.

---

## 5. Today's "Read / Write" Boundaries

As of early 2026, SOTA boundaries of each capability:

| Capability | SOTA | System / time |
| --- | --- | --- |
| Motor imagery decoding (non-invasive) | ~80% on 6-class | EEGNet baseline |
| 2D cursor control (invasive) | 90 bit/min | BrainGate + ReFIT |
| Handwriting BCI | 90 chars/min | Willett 2021 Nature |
| Invasive speech BCI | 62 WPM, 9.1% WER | Willett 2023 Nature |
| Non-invasive speech decoding | 41% sentence recognition | Meta Défossez 2023 |
| fMRI → image reconstruction | Near-photographic | MindEye2 2024 |
| fMRI → video | Low fidelity, semantically recognizable | MinD-Video 2024 |
| Cortical visual prosthesis | Perceivable stable phosphenes | Fernández 2021 |
| Intracortical microstimulation (ICMS) | 90% tactile detection | Flesher 2021 |

These numbers update every **6–12 months**. Subsequent chapters give the detailed technical paths behind each.

---

## 6. Shared Concerns with "Human-Like Intelligence" Research

Viewed from the BCI angle, "how to turn the brain into a readable/writable computational object" is itself a human-like-intelligence problem:

- **Representation:** Neural population activity lies on a low-dimensional manifold — isomorphic to concepts like "object-centric learning" and "latent-space prediction" in human-like intelligence.
- **Dynamics:** Motor cortex as a dynamical system (Churchland-Shenoy) and JEPA prediction in latent space are two sides of the same problem.
- **Learning:** Co-adaptation between BCI user and decoder is essentially a meta-learning problem.

These concepts are discussed in depth in [Chapter 10 Link to Embodied Intelligence](../10_Embodied_Intelligence_Link/index.md).

---

## 7. Logical Chain

1. **BCI = central nervous system × direct pathway × bidirectional information** — all three must hold.
2. **The three-dimensional taxonomy (invasiveness / direction / scenario) jointly characterizes a concrete system**; any BCI product should be located in this three-axis coordinate system.
3. **Signal scale determines intent granularity** — spike-level for fine-grained speech/handwriting; EEG-level for discrete choices.
4. **The 2024-2026 change occurs in the "clinical BCI scenario" dimension:** from single trials to multi-company, multi-country commercialization.
5. **Bidirectional BCI is becoming the new frontier** — read-write closed-loop is the key from "controlling external objects" to "embodied prosthesis."

## References

- Wolpaw et al. (2002). *Brain-computer interfaces for communication and control.* Clinical Neurophysiology. — Classic definitional review
- Zander & Kothe (2011). *Towards passive brain-computer interfaces.* J. Neural Engineering. — Active/passive taxonomy
- Hochberg et al. (2006). *Neuronal ensemble control of prosthetic devices.* Nature. https://www.nature.com/articles/nature04970
- Willett et al. (2023). *A high-performance speech neuroprosthesis.* Nature. https://www.nature.com/articles/s41586-023-06377-x
- Brain-Computer Interface Review (2024). Wiley Brain-X. https://onlinelibrary.wiley.com/doi/full/10.1002/brx2.70024
