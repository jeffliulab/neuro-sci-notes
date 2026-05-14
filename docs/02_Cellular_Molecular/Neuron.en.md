# Neuron — The Basic Unit of the Nervous System

> *The neuron is the basic structural and functional unit of the nervous system. About 86 billion neurons (Azevedo 2009) form the human brain, interconnected by 100 trillion synapses. This article covers neuron anatomy, main types (pyramidal / interneuron / Purkinje), basic electrophysiology, comparison with modern deep learning.*
>
> **Difficulty**: Introduction-Intermediate
> **Prerequisites**: [Neuroscience History](../00_Foundations/Neuroscience_History.en.md)

---

## 1. Neuron Basic Anatomy

<div class="diagram">
<svg viewBox="0 0 720 240" xmlns="http://www.w3.org/2000/svg">
  <text x="360" y="30" text-anchor="middle" font-family="Fraunces, Georgia, serif" font-style="italic" font-weight="600" font-size="18" fill="var(--dia-stroke)">Pyramidal Neuron Structure</text>

  <circle cx="280" cy="130" r="35" fill="var(--dia-bg-card)" stroke="var(--dia-accent)" stroke-width="2"/>
  <text x="280" y="135" text-anchor="middle" font-family="JetBrains Mono, monospace" font-size="11" fill="var(--dia-accent-deep)">Soma</text>

  <path d="M 250 110 Q 200 70 160 50" stroke="var(--dia-stroke)" stroke-width="2" fill="none"/>
  <path d="M 240 130 Q 180 130 150 130" stroke="var(--dia-stroke)" stroke-width="2" fill="none"/>
  <path d="M 250 150 Q 200 180 170 200" stroke="var(--dia-stroke)" stroke-width="2" fill="none"/>
  <text x="120" y="100" text-anchor="middle" font-family="Fraunces, Georgia, serif" font-style="italic" font-size="11" fill="var(--dia-stroke-soft)">Dendrites (inputs)</text>

  <line x1="315" y1="130" x2="550" y2="130" stroke="var(--dia-stroke)" stroke-width="3"/>
  <text x="420" y="120" text-anchor="middle" font-family="JetBrains Mono, monospace" font-size="11" fill="var(--dia-stroke)">Axon (output)</text>

  <rect x="330" y="125" width="30" height="10" fill="var(--dia-bg-card)" stroke="var(--dia-blue)" stroke-width="1"/>
  <rect x="380" y="125" width="30" height="10" fill="var(--dia-bg-card)" stroke="var(--dia-blue)" stroke-width="1"/>
  <rect x="430" y="125" width="30" height="10" fill="var(--dia-bg-card)" stroke="var(--dia-blue)" stroke-width="1"/>
  <text x="420" y="155" text-anchor="middle" font-family="Fraunces, Georgia, serif" font-style="italic" font-size="10" fill="var(--dia-blue)">Myelin sheaths</text>

  <path d="M 550 130 L 580 110 M 550 130 L 600 130 M 550 130 L 580 150" stroke="var(--dia-stroke)" stroke-width="2" fill="none"/>
  <circle cx="595" cy="115" r="5" fill="var(--dia-green)"/>
  <circle cx="610" cy="130" r="5" fill="var(--dia-green)"/>
  <circle cx="595" cy="145" r="5" fill="var(--dia-green)"/>
  <text x="620" y="180" text-anchor="middle" font-family="Fraunces, Georgia, serif" font-style="italic" font-size="11" fill="var(--dia-stroke-soft)">Synapses</text>
</svg>
</div>
<p class="figure-caption">Figure 1 — Typical pyramidal neuron structure: dendrites collect many inputs, soma integrates, axon outputs spikes, synapses pass to next neuron.</p>

### 1.1 Soma (Cell Body)

- Nucleus + cytoplasm
- Diameter 4-100 μm
- Protein synthesis center
- Integrates dendrite inputs, decides whether to spike

### 1.2 Dendrites

- Receive inputs (10³-10⁴ synapses per pyramidal neuron)
- Active (contain voltage-gated channels) — nonlinear integration
- Branching pattern defines neuron type

### 1.3 Axon

- Output end, up to 1m+ (motor neuron)
- Myelin sheath: formed by Schwann cells (PNS) / oligodendrocytes (CNS)
- Myelin accelerates signal (saltatory conduction)
- Ranvier nodes are unmyelinated segments, concentrate Na+ channels

### 1.4 Axon Terminal (Pre-synaptic Terminal)

- Contains vesicles (loaded with neurotransmitter)
- Action potential arrives → Ca²⁺ inflow → vesicle fuses, releases NT

---

## 2. Resting Membrane Potential

Neuron at rest: -70 mV intracellular (relative to outside):
- High intracellular K⁺ (140 mM)
- High extracellular Na⁺ (145 mM) + Cl⁻
- Na⁺/K⁺ pump (ATPase) maintains gradient: 3 Na⁺ out + 2 K⁺ in per cycle
- At rest mainly K⁺ leak channels open → close to K⁺ equilibrium potential (-90 mV)

Nernst equation:

$$E_K = \frac{RT}{zF} \ln \frac{[K^+]_o}{[K^+]_i}$$

---

## 3. Action Potential

Classic 1ms event:
1. **Depolarization**: threshold ~ -55 mV, Na⁺ channels open
2. **Peak**: ~+30 mV (Na⁺ inflow)
3. **Repolarization**: Na⁺ channels inactivate, K⁺ channels open, K⁺ outflow
4. **Hyperpolarization**: K⁺ channels slowly close
5. **Return to rest**: Na⁺/K⁺ pump

All-or-none: consistent spike above threshold; no fire below.

---

## 4. Main Neuron Types

### 4.1 Pyramidal Neurons

- Main excitatory neurons of cortex
- Triangular soma + apical dendrite (branches upward)
- ~70% of cortex
- Release glutamate

### 4.2 Interneurons

- Mainly inhibitory (GABA)
- Types:
  - **PV (Parvalbumin)**: fast-spiking, control network timing
  - **SOM (Somatostatin)**: modulate dendrite
  - **VIP**: disinhibition
- ~20-30% of cortex

### 4.3 Purkinje Neurons

- Cerebellum-specific
- **Massive dendrite tree** (~200k inputs)
- Output inhibitory (GABA)

### 4.4 Dopamine Neurons (VTA/SNc midbrain)

- ~600k in humans
- Control reward / motor
- Parkinson's = SNc degeneration

### 4.5 Granule Cells

- Cerebellum + hippocampal dentate gyrus
- Most abundant type

---

## 5. Biological vs AI Neural Network

| Aspect | Biological neuron | AI neuron |
|---|---|---|
| Output | spike (binary, 0-1ms) | continuous (float) |
| Integration | dendrite nonlinear + soma | weighted sum |
| Time | precise spike timing | synchronous forward pass |
| Learning | STDP, neuromodulation | Backprop, SGD |
| Count | 8×10¹⁰ | GPT-4 ~ 10¹² params |
| Connectivity | sparse, local + long | dense matrix |
| Energy | 20W brain | 10kW GPU |

→ AI vastly **simpler** than brain; brain at least 1-2 orders more complex.

---

## 6. PyTorch — Simplified Neuron Model

```python
import torch

class LIFNeuron:
    """Leaky Integrate-and-Fire — simplest spiking neuron."""
    def __init__(self, V_rest=-70, V_th=-55, V_reset=-75, tau=20):
        self.V = V_rest
        self.V_rest, self.V_th, self.V_reset, self.tau = V_rest, V_th, V_reset, tau
    
    def step(self, I_input, dt=0.1):
        dV = (self.V_rest - self.V + I_input) / self.tau * dt
        self.V += dV
        if self.V >= self.V_th:
            self.V = self.V_reset
            return 1
        return 0

neuron = LIFNeuron()
spikes = []
for t in range(1000):
    I = 12 if 200 < t < 800 else 0
    spikes.append(neuron.step(I))
```


---

## 7. Count and Connectivity

Human brain:
- 86 billion (8.6 × 10¹⁰) neurons (Azevedo 2009)
- 100-500 trillion synapses
- Density varies regionally: cerebellum 80% of neurons but 10% of volume
- Total axon length ~165,000 km (4× around Earth)

---

## 8. Relation to Glia

Non-neuronal cells (glia):
- Astrocytes: nutritional support, BBB
- Oligodendrocytes: CNS myelin
- Microglia: immunity / synaptic pruning
- Schwann cells: PNS myelin

Glia:neuron ≈ 1:1 (old 10:1 view corrected).

---

## 9. Neurogenesis

Adult brain can still produce new neurons:
- Hippocampus dentate gyrus (confirmed)
- Olfactory bulb (rodents)
- Cortex (disputed)

→ Stem cell-based therapies explored in Parkinson etc.

---

## 10. Common Pitfalls

### 10.1 "Neuron Count Determines Intelligence" Misconception

Humans (8.6 × 10¹⁰) vs elephants (2.5 × 10¹¹) — elephants have more. But humans have more cortex neurons + higher connectivity.

### 10.2 LIF ≠ Real Neuron

LIF is a toy model; real neurons have dendritic computation + neuromodulation etc.

### 10.3 Hodgkin-Huxley Axon Only

HH measured in axon; dendrite dynamics differ (NMDA etc. complex).

### 10.4 Spike Timing Matters

Many ML treats firing rate as output, ignores timing. Real brain heavily depends on precise timing.

### 10.5 Glia Aren't Just "Support"

Past 20 years revealed glia participate in synaptic plasticity / signaling.

---

## 11. Related Concepts

- **History**: [Neuroscience History](../00_Foundations/Neuroscience_History.en.md)
- **AI comparison**: https://jeffliulab.github.io/ai-notes/02_Deep_Learning/01_Intro/MLP/

---

## References

1. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
2. **Azevedo, F. et al.** "Equal numbers of neuronal and nonneuronal cells make the human brain an isometrically scaled-up primate brain." *J Comp Neurol*, 2009.
3. **Hodgkin, A. L. & Huxley, A. F.** *J Physiol*, 1952.
4. **Cajal, S. R.** *Histology of the Nervous System*. 1909-1911.
5. **Bear, M. F. et al.** *Neuroscience: Exploring the Brain*. 4th ed., 2015.
