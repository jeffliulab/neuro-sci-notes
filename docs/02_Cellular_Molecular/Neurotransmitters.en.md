# Neurotransmitters — Glutamate, GABA, DA, 5-HT, ACh, etc.

> *Neurotransmitters (NTs) are chemical molecules transmitting signals at synapses. The brain has 100+ NTs; 7-10 main types dominate most functions. This article covers **major NT types**, receptor mechanisms, drug / disease connections.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Synapse](Synapse.en.md), [Neuron](Neuron.en.md)
> **Further reading**: [LTP/LTD](LTP_LTD.en.md)

---

## 1. Main NT Categories

### 1.1 Amino Acids

- **Glutamate (Glu)**: main **excitatory** NT, ~80% cortex synapses
- **GABA**: main **inhibitory** NT
- **Glycine**: inhibitory, mainly spinal cord / brainstem

### 1.2 Monoamines

- **Dopamine (DA)**: reward, motor, attention
- **Norepinephrine (NE)**: arousal, attention, stress
- **Serotonin (5-HT)**: mood, sleep, appetite
- **Histamine**: arousal, allergy

### 1.3 Acetylcholine (ACh)

- Neuromuscular junction
- Attention, learning

### 1.4 Neuropeptides

- Endorphin, Enkephalin (endogenous opioids)
- Substance P (pain)
- Oxytocin (social)
- ~50+ types

### 1.5 Gases

- NO (Nitric Oxide), CO — retrograde messengers

---

## 2. Receptor Types

### 2.1 Ionotropic

NT binding → channel opens directly → ion flow. **Fast** (ms level).

- **AMPA receptor** (Glu): fast excitatory
- **NMDA receptor** (Glu): slow, voltage + ligand dual-gate
- **GABA-A** (GABA): Cl- inflow, inhibitory
- **Nicotinic ACh** (nAChR): cation

### 2.2 Metabotropic

NT binding → GPCR → secondary messenger → slow modulation (sec-min level).

- **GABA-B**: K+ outflow
- **mGluR** (Glu)
- **DA receptors D1-D5**
- **5-HT receptors 5-HT1-5-HT7**
- **Muscarinic ACh** (mAChR)

---

## 3. Glutamate Detail

Most important NT:
- 80% of cortex EPSPs mediated by Glu
- Receptors: AMPA + NMDA + mGluR
- **NMDA**: opens when V is depolarized AND Glu present (Mg²⁺ block removed) → coincidence detector for LTP
- **Excess Glu = excitotoxicity** (stroke / ALS)

---

## 4. GABA Detail

Main inhibitory:
- 30% of hippocampal + cortex synapse inhibition
- GABA-A receptor: Cl- channel, target of anxiolytics, anesthetics
- Drugs:
  - **Benzodiazepines** (Valium): GABA-A allosteric enhancers
  - **Barbiturates**, **Alcohol**: similar
  - **Bicuculline**: GABA-A blocker (experimental)

---

## 5. Dopamine System

```
VTA → NAcc / PFC (mesolimbic / mesocortical, reward)
SNc → striatum (nigrostriatal, motor)
Hypothalamus → pituitary (tuberoinfundibular)
```

### 5.1 Function

- Reward prediction error (Schultz 1997)
- Movement (Parkinson DA degeneration)
- Attention (ADHD: DA hypothesis)

### 5.2 Drugs

- **Cocaine, amphetamines**: block DA reuptake
- **Antipsychotics** (Haloperidol): D2 antagonist
- **L-DOPA**: DA precursor (Parkinson treatment)

---

## 6. 5-HT System

```
Brainstem raphe nuclei → whole brain
```

### 6.1 Function

- Mood (depression hypothesis)
- Sleep
- Appetite, sexuality

### 6.2 Drugs

- **SSRI** (Prozac, Zoloft): inhibit 5-HT reuptake → antidepressant
- **LSD, psilocybin**: 5-HT2A agonist → hallucinations

---

## 7. ACh System

### 7.1 Neuromuscular Junction

Motor neuron ACh → nAChR → muscle contraction.
Curare blocks nAChR → paralysis.

### 7.2 Central

- Basal forebrain → cortex (attention, learning)
- Brainstem → thalamus (arousal)
- Alzheimer: ACh neuron degeneration → cholinesterase inhibitor treatment

---

## 8. NT Lifecycle

```
Synthesis (neuron cytoplasm)
  ↓
Vesicle loading
  ↓
AP → Ca²⁺ → fusion → release
  ↓
Diffusion across cleft
  ↓
Bind post-receptor → signal
  ↓
Dissociation
  ↓
Reuptake (transporter) / enzymatic degradation
```

Example ACh: synthesized by ChAT → vesicle → release → AChE degrades to choline + acetate → choline reuptake → re-synthesis.

---

## 9. Experimental Measurement

- **HPLC**: NT concentration
- **Microdialysis**: in vivo NT measurement
- **Optogenetics**: control NT release
- **Fluorescent sensors** (GRAB-DA, iGluSnFR): real-time imaging

---

## 10. Drug Mechanism Summary

| Class | NT | Mechanism |
|---|---|---|
| Antidepressant (SSRI) | 5-HT | Reuptake block |
| Antipsychotic | DA | D2 antagonist |
| Anxiolytic (Benzo) | GABA | GABA-A enhancement |
| Stimulant (Adderall) | DA, NE | Reuptake block / release boost |
| Opioid | Endorphin | μ receptor agonist |
| Anesthetic | GABA, NMDA | Multi-target |
| Hallucinogen | 5-HT | 5-HT2A agonist |
| Cholinesterase inhibitor | ACh | AChE inhibition (Alzheimer) |

---

## 11. Common Pitfalls

### 11.1 Single-NT Misconception

Many neurons release > 1 NT (co-transmission).

### 11.2 Receptor Specificity

Subtypes have very different properties. "GABA receptor" includes many.

### 11.3 Acute vs Chronic

Short-term vs long-term NT changes have completely different mechanisms.

### 11.4 BBB

Many drugs don't cross the blood-brain barrier.

### 11.5 Net Effect ≠ NT Level

NT total ≠ function. Receptors / circuits / states all matter.

---

## 12. Related Concepts

- **Same section**: [Synapse](Synapse.en.md), [LTP/LTD](LTP_LTD.en.md), [Ion Channels](Ion_Channels.en.md)

---

## References

1. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
2. **Iversen, L. L. et al.** *Introduction to Neuropsychopharmacology*. Oxford, 2009.
3. **Schultz, W.** "A neural substrate of prediction and reward." *Science*, 1997.
4. **Squire, L. R. et al.** *Fundamental Neuroscience*. 4th ed., 2012.
