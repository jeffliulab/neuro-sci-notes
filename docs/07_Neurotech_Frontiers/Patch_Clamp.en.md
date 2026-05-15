# Patch Clamp

> *Patch clamp (Neher & Sakmann, 1991 Nobel) is the gold standard of single-cell electrophysiology: a glass micropipette forms a gigaohm seal on a membrane patch, recording single ion channels or whole-cell currents/voltage. Unmatched spatial precision (single channel pA). Modern: automated patch clamp, in vivo patch, Patch-seq (electrophysiology + transcriptome). Foundation of [Hodgkin Huxley](../02_Cellular_Molecular/Hodgkin_Huxley.en.md) and channel pharmacology.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Ion Channels](../02_Cellular_Molecular/Ion_Channels.en.md), [Action Potential](../02_Cellular_Molecular/Action_Potential.en.md)

---

## 1. Principle

- Polished glass micropipette (tip ~ 1 μm) gently presses cell membrane + negative pressure → **gigaohm seal** (GΩ)
- High-resistance seal → very low noise → can measure pA-level single-channel currents
- Clamp: fix voltage measure current, or fix current measure voltage

---

## 2. Four Configurations

| Configuration | Contact | Use |
|---|---|---|
| **Cell-attached** | Intact membrane | Single channel (in situ) |
| **Whole-cell** | Ruptured membrane | Whole-cell current / AP / synapse |
| **Inside-out** | Inner face out | Intracellular regulation of single channel |
| **Outside-out** | Outer face out | Ligand-gated single channel |
| **Perforated** | Antibiotic-perforated | Whole-cell but preserve intracellular |

---

## 3. Voltage Clamp vs Current Clamp

- **Voltage clamp**: fix V, measure I → resolve ion currents (basis of HH experiments!)
- **Current clamp**: inject I, measure V → observe AP, firing patterns
- Dynamic clamp: real-time compute injected "virtual conductance" (test hypotheses)

---

## 4. PyTorch — Single-Channel Current (stochastic gating)

```python
import torch

def single_channel_current(T=1000, p_open=0.3, i_single=2.0, tau=10.0):
    """Stochastic 2-state channel: pA-level openings (cell-attached-like)."""
    state = 0
    trace = []
    for t in range(T):
        if state == 0 and torch.rand(1) < p_open / tau:
            state = 1
        elif state == 1 and torch.rand(1) < (1 - p_open) / tau:
            state = 0
        trace.append(state * i_single)   # discrete pA steps
    return torch.tensor(trace)   # ensemble average -> macroscopic I
```

---

## 5. History — HH and Single Channels

- Hodgkin-Huxley used voltage clamp (squid giant axon) to infer ion conductances (1952)
- But couldn't see single channels — Neher & Sakmann 1976 patch clamp **directly saw** single-channel openings
- Confirmed HH's m³h, n⁴ are statistical averages
- See [Hodgkin Huxley](../02_Cellular_Molecular/Hodgkin_Huxley.en.md)

---

## 6. Modern Advances

- **Automated patch clamp**: robotic + multi-channel (high-throughput drug screening)
- **In vivo patch**: single-cell recording in anesthetized / awake animals (blind / 2-photon targeted)
- **Patch-seq**: same neuron electrophysiology + morphology + single-cell transcriptome → multimodal cell typing
- **Dynamic clamp**: inject computed conductance to test circuit hypotheses

---

## 7. vs Other Electrophysiology

| | Patch clamp | Extracellular | Ca imaging |
|---|---|---|---|
| Signal | Membrane current/voltage (direct) | Field potential spike | Ca indirect |
| Precision | Single channel pA | Single neuron spike | Slow |
| Count | 1 (few) | 100s (Neuropixels) | 1000s |
| Synaptic current | ✓ (unique) | ✗ | ✗ |
| Throughput | Low | Medium | High |

→ Patch clamp irreplaceable: **subcellular + synaptic current** precision.

---

## 8. Applications

- Ion channel pharmacology (drug screening — automated patch clamp)
- Synaptic transmission mechanisms (EPSC/IPSC, quantal analysis)
- Cellular intrinsic excitability (firing type classification)
- Channelopathy research
- Patch-seq cell type atlas (Allen / BICCN)

---

## 9. Limitations

- Very low throughput (1 cell / long time, needs high skill)
- Invasive (rupture dialysis alters intracellular)
- Hard long-term / hard free behavior
- Hard to reach deep / specific cell types (needs targeting)

---

## 10. Common Pitfalls

### 10.1 High Throughput

Very low throughput (skill + time intensive); automation partially helps.

### 10.2 Whole-cell Non-Perturbing

Rupture → intracellular dialysis alters contents; perforated patch mitigates.

### 10.3 Single Channel = Deterministic

Stochastic gating; macroscopic current is statistical average (HH m/h/n).

### 10.4 = Neuropixels-Like

Complementary: patch single-cell subcellular precision, Neuropixels large-scale population.

### 10.5 In Vivo Impossible

In vivo / awake patch + Patch-seq achieved (very difficult).

---

## 11. Related Concepts

- **Same section**: [Calcium Imaging](Calcium_Imaging.en.md), [Neuropixels](Neuropixels.en.md), [Optogenetics_Advanced](Optogenetics_Advanced.en.md)
- **Cellular**: [Ion Channels](../02_Cellular_Molecular/Ion_Channels.en.md), [Hodgkin Huxley](../02_Cellular_Molecular/Hodgkin_Huxley.en.md), [Synapse](../02_Cellular_Molecular/Synapse.en.md)
- **Foundation**: [Research Methods](../00_Foundations/Research_Methods.en.md)

---

## References

1. **Neher, E. & Sakmann, B.** "Single-channel currents recorded from membrane of denervated frog muscle fibres." *Nature*, 1976.
2. **Hamill, O. P. et al.** "Improved patch-clamp techniques for high-resolution current recording." *Pflügers Arch*, 1981.
3. **Cadwell, C. R. et al.** "Electrophysiological, transcriptomic and morphologic profiling of single neurons (Patch-seq)." *Nat Biotechnol*, 2016.
4. **Hodgkin, A. L. & Huxley, A. F.** "A quantitative description of membrane current." *J Physiol*, 1952.
