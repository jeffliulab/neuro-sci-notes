# Open Neuroscience

> *The open science movement is reshaping neuroscience: open data (OpenNeuro, DANDI, Allen Brain), open tools (MNE, Suite2p, DeepLabCut), open standards (BIDS, NWB), preregistration, preprints (bioRxiv). A response to the replication crisis + big-data era. Especially crucial in the AI era: large models need large-scale standardized neural data.*
>
> **Difficulty**: Beginner
> **Prerequisites**: [Research Methods](Research_Methods.en.md), [Statistics in Neuroscience](Statistics_in_Neuroscience.en.md)

---

## 1. Why Open

- Response to **replication crisis** (transparency → verifiable)
- **Big data**: single lab cannot collect everything
- **AI**: training models needs large-scale standard data
- **Efficiency**: don't reinvent the wheel
- **Equity**: under-resourced groups can also research

---

## 2. Open Databases

| Database | Content |
|---|---|
| **OpenNeuro** | fMRI/EEG/MEG (BIDS format) |
| **DANDI** | Electrophysiology / optical (NWB) |
| **Allen Brain Atlas** | Gene expression, connectivity, Ca imaging |
| **Human Connectome Project** | Healthy connectome |
| **UK Biobank** | 500k people imaging + genes + health |
| **IBL** (Int'l Brain Lab) | Standardized decision task + Neuropixels |
| **DABI** | Invasive human data |
| **NeuroMorpho** | Neuron morphology |

---

## 3. Data Standards

- **BIDS** (Brain Imaging Data Structure): file organization standard
- **NWB** (Neurodata Without Borders): unified electrophysiology / optical format
- Standardization → tool interoperability + data reuse

---

## 4. Open Tools

| Tool | Use |
|---|---|
| **MNE-Python** | EEG/MEG analysis |
| **EEGLAB** | EEG (MATLAB) |
| **Suite2p / CaImAn** | Calcium imaging pipeline |
| **DeepLabCut / SLEAP** | Behavior tracking |
| **SpikeInterface** | Unified spike sorting |
| **fMRIPrep** | fMRI preprocessing standard |
| **Nilearn** | fMRI machine learning |
| **Brian2 / NEST** | SNN simulation |

---

## 5. Preregistration + Registered Reports

- **Preregistration**: lock hypothesis + methods before analysis
- **Registered Reports**: methods peer-reviewed first → in-principle acceptance (results don't affect publication)
- Solves p-hacking + publication bias

---

## 6. Preprints

- **bioRxiv** (since 2013), **arXiv** q-bio
- Fast + free dissemination
- COVID accelerated acceptance
- But note: not peer-reviewed

---

## 7. Python — Loading NWB Data Example

```python
# pip install pynwb
from pynwb import NWBHDF5IO

with NWBHDF5IO('session.nwb', 'r') as io:
    nwb = io.read()
    spikes = nwb.units['spike_times'][:]      # standardized spike data
    behavior = nwb.acquisition['position'].data[:]
# Same code reads any NWB dataset → interoperability
```

---

## 8. Big Science Projects

- **BRAIN Initiative** (US, 2013): tools + cell census
- **Human Brain Project** (EU, 2013-2023): simulation + infrastructure
- **China Brain Project**, **Japan Brain/MINDS**
- **MICrONS**: cortex connectome + function
- **Allen Institute**: open data exemplar

---

## 9. AI × Open Neuro

- Large-scale standard data → train foundation models (neural data)
- POYO, Neuroformer, etc. "neural data Transformers"
- Need NWB/BIDS to scale
- Analogous to NLP needing Common Crawl

---

## 10. Challenges

- **Privacy**: human brain data = sensitive (see [Neuroethics](Neuroscience_Ethics.en.md))
- **Incentives**: sharing data lacks career reward
- **Quality**: open ≠ high-quality (needs curation)
- **Storage**: EM/connectome PB-EB scale

---

## 11. Common Pitfalls

### 11.1 Open Data = High Quality

Needs curation + metadata; garbage in garbage out.

### 11.2 Preprint = Peer-Reviewed

Not reviewed; read critically.

### 11.3 Preregistration Kills Exploration

Exploratory analysis still allowed, just labeled (non-confirmatory).

### 11.4 Standard Format Optional

Without BIDS/NWB → data reuse + tool interoperability collapses.

### 11.5 Big Data Automatically = Good Science

Without good questions + behavior, data volume useless (see [Behavioral Neuroscience](Behavioral_Neuroscience.en.md)).

---

## 12. Related Concepts

- **Same section**: [Research Methods](Research_Methods.en.md), [Statistics in Neuroscience](Statistics_in_Neuroscience.en.md), [Neuroscience Ethics](Neuroscience_Ethics.en.md)
- **Frontiers**: [Calcium Imaging](../07_Neurotech_Frontiers/Calcium_Imaging.en.md), [EEG](../07_Neurotech_Frontiers/EEG.en.md)
- **AI**: foundation models, data standardization

---

## References

1. **Gorgolewski, K. J. et al.** "The brain imaging data structure (BIDS)." *Sci Data*, 2016.
2. **Rübel, O. et al.** "The Neurodata Without Borders ecosystem for neurophysiological data science." *eLife*, 2022.
3. **International Brain Laboratory** "Standardized and reproducible measurement of decision-making in mice." *eLife*, 2021.
4. **Poldrack, R. A. & Gorgolewski, K. J.** "Making big data open: data sharing in neuroimaging." *Nat Neurosci*, 2014.
