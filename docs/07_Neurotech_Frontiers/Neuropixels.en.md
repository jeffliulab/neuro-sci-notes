# Neuropixels — High-Density Silicon Probes

> *Neuropixels (2017 IMEC + Allen + HHMI + UCL) is an electrophysiology revolution: a single silicon probe with ~ 1000 recording sites, hundreds of neurons simultaneously across multiple brain regions. 2.0 (2021) ~ 5000 sites + multi-shank. Fundamentally changed systems neuroscience data scale — from dozens to thousands of neurons. Paired with Kilosort spike sorting.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Action Potential](../02_Cellular_Molecular/Action_Potential.en.md), [Research Methods](../00_Foundations/Research_Methods.en.md)

---

## 1. Why Revolutionary

| | Traditional (tetrode/Utah) | Neuropixels |
|---|---|---|
| Sites | ~ 4-100 | 960 (1.0) / 5120 (2.0) |
| Simultaneous neurons | 10s | 100s-1000s |
| Brain regions | Single | Spans multiple (one probe) |
| Form | Bulky | Thin & long (< 100 μm wide) |

---

## 2. Specs (1.0 / 2.0)

| Parameter | 1.0 | 2.0 |
|---|---|---|
| Electrode sites | 960 | 5120 |
| Simultaneous read | 384 ch | 384 ch (switchable) |
| Shank | 1 | 1 or 4 |
| Length | 10 mm | 10 mm |
| Sampling | 30 kHz | 30 kHz |
| CMOS integration | On-chip amp + ADC + MUX | Smaller, more stable |

---

## 3. CMOS Integration

- Probe is a chip: on-shank amplifier + ADC + multiplexer
- Solves "how to wire thousands of sites" (on-chip digitization + time-division multiplexing)
- Only ~ digital lines output → freely moving animals

---

## 4. Spike Sorting Required

- One site records multiple neurons + one neuron recorded by multiple sites
- **Kilosort** (template matching + GPU), MountainSort, SpyKING CIRCUS
- High density → drift tracking (brain movement) more critical
- Still needs manual curation (false positives/merges)

---

## 5. PyTorch — High-Density Template Matching (simplified)

```python
import torch

def template_match(traces, templates):
    """traces: (n_ch, T); templates: (n_units, n_ch, L). Detect spikes."""
    n_units = templates.shape[0]
    scores = []
    for u in range(n_units):
        tmpl = templates[u]                        # (n_ch, L)
        # Cross-correlate across channels (matched filter)
        sc = torch.nn.functional.conv1d(
            traces.unsqueeze(0), tmpl.unsqueeze(0)).squeeze()
        scores.append(sc)
    return torch.stack(scores)   # peaks = spike times per unit
```

---

## 6. Scientific Impact

- **Large-scale population dynamics** (see [Neural Population Dynamics](../05_Computational_Neuroscience/Neural_Population_Dynamics.en.md))
- **Multi-region simultaneous**: cross-region interaction (e.g., IBL brain-wide map)
- **Behavior correlation**: thousands of neurons + naturalistic behavior
- **Brain-wide single-cell**: approaching "whole-brain single-cell activity map" goal

---

## 7. International Brain Lab (IBL)

- Multi-lab standardized Neuropixels + unified decision task
- "Brain-wide map" of decision-making
- Open data (see [Open Neuroscience](../00_Foundations/Open_Neuroscience.en.md))
- Exemplar of reproducibility + scale

---

## 8. Limitations

- Still **invasive** (insertion causes tissue damage + glial reaction)
- Long-term stability (chronic recording drift, signal decay)
- Huge data volume (TB/experiment → storage + pipeline challenge)
- Spike sorting still imperfect (ground truth hard)
- Mainly rodent / primate (humans only limited intraoperatively)

---

## 9. vs Other High-Density

- **Utah array**: cortical surface array (used in human BCI, e.g., BrainGate)
- **Neuropixels**: deep penetrating (mostly animals)
- **Neuralink threads**: flexible + wireless (see [Neuralink](Neuralink.en.md))
- **NeuroGrid / flexible ECoG**: surface high-density

---

## 10. Common Pitfalls

### 10.1 Non-Invasive

It's invasive probe insertion; tissue damage occurs.

### 10.2 Automatically Gives Neurons

Needs spike sorting + curation; not plug-and-play.

### 10.3 One Site = One Neuron

One site records multiple neurons; one neuron spans multiple sites.

### 10.4 Long-Term Stable

Chronic drift + signal decay; long-term recording still challenging.

### 10.5 Routinely Used in Humans

Humans only intraoperative / limited research; mainly animals.

---

## 11. Related Concepts

- **Same section**: [Calcium Imaging](Calcium_Imaging.en.md), [Neuralink](Neuralink.en.md)
- **Computational**: [Neural Population Dynamics](../05_Computational_Neuroscience/Neural_Population_Dynamics.en.md)
- **Foundation**: [Research Methods](../00_Foundations/Research_Methods.en.md), [Open Neuroscience](../00_Foundations/Open_Neuroscience.en.md)

---

## References

1. **Jun, J. J. et al.** "Fully integrated silicon probes for high-density recording of neural activity (Neuropixels)." *Nature*, 2017.
2. **Steinmetz, N. A. et al.** "Neuropixels 2.0: A miniaturized high-density probe for stable, long-term brain recordings." *Science*, 2021.
3. **Pachitariu, M. et al.** "Kilosort: realtime spike-sorting for extracellular electrophysiology." *bioRxiv*, 2016.
4. **International Brain Laboratory** "A brain-wide map of neural activity during complex behaviour." *bioRxiv*, 2023.
