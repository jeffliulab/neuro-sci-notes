# LTP / LTD — Long-Term Synaptic Plasticity, Cellular Basis of Learning & Memory

> *In 1973 Bliss & Lømo first recorded **Long-Term Potentiation (LTP)** in the hippocampal dentate gyrus — high-frequency stimulation causes sustained synaptic strength increase lasting hours-days. This is a leading candidate for "cellular mechanism of learning & memory". 50 years of research identified the NMDA receptor + CaMKII + AMPA receptor trafficking chain.*
>
> **Difficulty**: Intermediate-Advanced
> **Prerequisites**: [Synapse](Synapse.en.md), [Neurotransmitters](Neurotransmitters.en.md)

---

## 1. Experimental Protocol

### 1.1 LTP Induction

- **High-frequency stimulation (HFS, tetanus)**: 100 Hz × 1s
- Recording: field EPSP slope or single-cell EPSC
- Strength enhanced 200-300%, lasts hours-days

### 1.2 LTD Induction

- **Low-frequency stimulation (LFS)**: 1 Hz × 15 min
- Or paired-pulse low frequency
- Synaptic strength reduced 50%

---

## 2. Molecular Mechanism (NMDA-dependent LTP)

```
1. Pre AP → Glu release
2. Glu → AMPA receptor → local depolarize
3. Post depolarize + Glu → NMDA receptor opens (Mg²⁺ block removed)
4. Massive Ca²⁺ inflow
5. Ca²⁺ → CaMKII activation
6. CaMKII → AMPA receptor phosphorylation + PSD insertion
7. ↑ AMPA receptor → larger EPSP
8. Long-term: gene transcription, protein synthesis
```

### 2.1 NMDA as Coincidence Detector

- Requires Glu (pre) **and** depolarize (post) simultaneously
- → "Hebbian": cells that fire together, wire together

### 2.2 LTD: Low Ca²⁺ → Calcineurin → AMPA Endocytosis

Low-freq low Ca²⁺ → different enzymes → AMPA receptor removal.

---

## 3. STDP (Spike-Timing-Dependent Plasticity)

Precise timing determines LTP / LTD:

| Pre vs Post Order | Result |
|---|---|
| Pre before post (~ 10 ms) | LTP |
| Post before pre (~ 10 ms) | LTD |
| Distance > 50 ms | No change |

---

## 4. Early vs Late LTP

### 4.1 E-LTP (1-3 hours)

- No protein synthesis needed
- AMPA receptor already exists, only trafficking
- Anisomycin (protein synth blocker) doesn't prevent

### 4.2 L-LTP (> 3 hours, "synaptic consolidation")

- **Requires** protein synthesis
- CREB and other transcription factors
- New mRNA → new proteins → new synapses

---

## 5. LTP Types

### 5.1 NMDA-dependent (Schaffer collateral → CA1)

Classic LTP, described above.

### 5.2 Non-NMDA (mossy fiber → CA3)

Pre-synaptic LTP — increases release probability.

### 5.3 Cerebellar LTD

Core of cerebellar learning — Marr-Albus theory (Climbing fiber teaches Parallel fiber).

---

## 6. Behavioral Correlates

### 6.1 Hippocampal LTP ↔ Spatial Memory

- LTP block (NMDA antagonist) → impairs Morris water maze learning
- But: LTP ≠ memory itself, only neural correlate

### 6.2 Amygdala LTP ↔ Fear Conditioning

Classical Pavlovian conditioning strongly correlates with amygdala LTP.

### 6.3 Cortex LTP ↔ Perceptual Learning

V1 LTP correlates with post-training visual improvement.

---

## 7. PyTorch — STDP Learning Rule

```python
import torch

class STDPSynapse:
    def __init__(self, w=0.5, lr=0.01, tau=20):
        self.w = w
        self.lr, self.tau = lr, tau
        self.pre_trace = 0
        self.post_trace = 0
    
    def step(self, pre_spike, post_spike, dt=1):
        self.pre_trace *= (1 - dt/self.tau)
        self.post_trace *= (1 - dt/self.tau)
        if pre_spike:
            self.pre_trace += 1
            self.w -= self.lr * self.post_trace
        if post_spike:
            self.post_trace += 1
            self.w += self.lr * self.pre_trace
        self.w = max(0, min(1, self.w))
        return self.w * pre_spike
```

---

## 8. Relation to Backprop

### 8.1 Backprop

- Global gradient: $\frac{\partial L}{\partial w}$
- Needs backward pass + full label
- Biologically infeasible

### 8.2 STDP

- Local spike-timing rule
- No global signal
- Lacks credit assignment

### 8.3 Modern Attempts

- Equilibrium Propagation (Bengio 2017): mathematically equivalent to backprop, biologically feasible
- Predictive coding: gradients via error neurons
- Survey: Lillicrap "Backpropagation and the brain" (2020)

---

## 9. Pathology

- **Alzheimer**: synapse loss + LTP impairment precede neuron loss
- **Depression**: hippocampal LTP reduced
- **Addiction**: VTA LTP hijacked by abused drugs
- **Autism**: NMDA / AMPA imbalance

---

## 10. History

- **1949** — Hebb hypothesis ("cells that fire together")
- **1973** — Bliss & Lømo first hippocampal LTP report
- **1986** — Morris NMDA antagonist blocks spatial learning
- **1992** — Tsien NMDA-KO mouse — LTP + memory impair
- **1998** — STDP classic protocol (Bi & Poo, Markram)
- **2000s** — CaMKII / AMPA trafficking details
- **2010s** — Labeling memory engrams (Tonegawa)
- **2020s** — optogenetic LTP induction proves causally

---

## 11. Common Pitfalls

### 11.1 LTP ≠ Memory

LTP is a cellular phenomenon; memory is behavioral. Strong correlation, causation incomplete.

### 11.2 NMDA Not Unique

Mossy fiber + cerebellum LTP don't require NMDA.

### 11.3 Saturation

LTP has a ceiling; too much stimulation flips to LTD.

### 11.4 In vitro ≠ In vivo

Slice LTP protocols don't fully transfer to live animals.

### 11.5 Network Plasticity

LTP is synapse-level; memory involves network reorganization.

---

## 12. Related Concepts

- **Same section**: [Synapse](Synapse.en.md), [Neurotransmitters](Neurotransmitters.en.md), [Hodgkin-Huxley](Hodgkin_Huxley.en.md)
- **AI comparison**: https://jeffliulab.github.io/ai-notes/02_Deep_Learning/01_Intro/MLP/

---

## References

1. **Bliss, T. V. P. & Lømo, T.** *J Physiol*, 1973.
2. **Hebb, D. O.** *The Organization of Behavior*. 1949.
3. **Bi, G. & Poo, M.-M.** "Synaptic Modifications in Cultured Hippocampal Neurons (STDP)." *J Neurosci*, 1998.
4. **Tonegawa, S. et al.** "Memory engram cells have come of age." *Neuron*, 2015.
5. **Lillicrap, T. et al.** "Backpropagation and the brain." *Nat Rev Neurosci*, 2020.
