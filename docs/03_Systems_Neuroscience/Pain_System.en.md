# Pain System

> *Pain = a multidimensional experience of sensation + emotion + cognition, not merely a "harm signal." Nociceptor → spinal dorsal horn (gate) → ascending (lateral: sensory-discriminative; medial: affective) → cortical "pain matrix." Descending modulation (PAG-RVM) + endogenous opioids. Chronic pain is central sensitization (a neuroplastic disease), not persistent tissue damage.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Somatosensory](Somatosensory.en.md), [Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.en.md)

---

## 1. Pain ≠ Nociception

- **Nociception**: neural detection of harmful stimuli (unconscious)
- **Pain**: subjective multidimensional experience (sensation + emotion + cognition)
- Dissociable: congenital insensitivity (SCN9A) has nociceptors but no pain; pain without injury (fibromyalgia)

---

## 2. Nociceptors + Fibers

| Fiber | Myelination | Speed | Pain quality |
|---|---|---|---|
| **Aδ** | Thin myelin | 5-30 m/s | First pain (fast, sharp, localized) |
| **C** | Unmyelinated | 0.5-2 m/s | Second pain (slow, dull, diffuse, burning) |

Nociceptor: free nerve endings, TRPV1 (heat/capsaicin), TRPM8 (cold), ASIC (acid), mechanically gated.

---

## 3. Pathway

```
Nociceptor → DRG → spinal dorsal horn (relay + gate)
   ↓ decussation
Spinothalamic tract (ascending)
   ├→ Lateral system: VPL → S1/S2 (sensory-discriminative: where, how strong)
   └→ Medial system: medial thalamus → ACC/insula (affective: how unpleasant)
"Pain matrix" (no single pain center)
```

---

## 4. Gate Control (Melzack & Wall 1965)

- Spinal dorsal horn "gate": Aβ (touch) activates inhibitory interneurons → closes gate → suppresses pain
- Explains rubbing sore spot reducing pain, TENS, acupuncture partial mechanism
- Later expanded into **neuromatrix theory** (pain is a brain-generated output)

---

## 5. Descending Modulation

- **PAG → RVM → spinal cord**: endogenous analgesia (opioid, 5-HT, NE)
- Placebo analgesia = endogenous opioids (naloxone-reversible)
- Stress analgesia, attentional modulation
- Can also descend to **facilitate** (imbalanced in chronic pain)

---

## 6. PyTorch — Gate Control

```python
import torch

def gate_control(c_fiber, a_beta, descending_inhib=0.0):
    """A-beta (touch) + descending close the gate on C-fiber pain."""
    inhibitory_interneuron = torch.sigmoid(a_beta + descending_inhib)
    pain_output = torch.relu(c_fiber - 1.5 * inhibitory_interneuron)
    return pain_output   # rubbing (↑a_beta) or descending → less pain
```

---

## 7. Central Sensitization + Chronic Pain

- **Wind-up**: repeated C-fiber → dorsal horn NMDA → progressive increase (short-term)
- **Central sensitization**: long-term → pain threshold↓, receptive field expand, allodynia (light touch hurts), hyperalgesia
- Chronic pain = a **plasticity disease** of the nervous system (often no persistent tissue damage)
- LTP-like mechanism (see [LTP_LTD](../02_Cellular_Molecular/LTP_LTD.en.md))

---

## 8. Chronic Pain Types

| Type | Mechanism |
|---|---|
| Nociceptive | Persistent tissue damage (inflammation) |
| Neuropathic | Nerve injury (burning, electric-shock-like) |
| Nociplastic | Central sensitization (fibromyalgia; no clear injury) |
| Mixed | Multiple mechanisms |

---

## 9. Treatment

- **WHO ladder**: NSAID → weak opioid → strong opioid (+ adjuvants)
- **Neuropathic**: gabapentinoids, TCA/SNRI (non-opioid first)
- **Opioid crisis**: long-term opioids for chronic non-cancer pain harmful (tolerance/hyperalgesia/addiction, see [Addiction](../08_Neuro_Disorders/Addiction.en.md))
- **Non-pharmacological**: CBT, exercise, acupuncture, neuromodulation (SCS, DBS)
- **New targets**: Nav1.7/1.8, CGRP (migraine), NGF antibody

---

## 10. Common Pitfalls

### 10.1 Pain ∝ Tissue Damage

Weakly correlated; pain is a brain output (neuromatrix), chronic pain often no damage.

### 10.2 A Single "Pain Center"

It's a distributed pain matrix (sensory + emotion + cognition).

### 10.3 Chronic Pain = Prolonged Acute Pain

It's central sensitization (plasticity disease), different mechanism.

### 10.4 Opioids First-Line for Chronic Pain

Long-term opioids for chronic non-cancer pain harmful; non-opioid + multimodal first.

### 10.5 Placebo = "Fake"

Real endogenous opioid mechanism (naloxone-reversible), neurally measurable.

---

## 11. Related Concepts

- **Same section**: [Somatosensory](Somatosensory.en.md), [Reward_System](Reward_System.en.md)
- **Cellular**: [Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.en.md), [Ion Channels](../02_Cellular_Molecular/Ion_Channels.en.md) (TRPV1/Nav), [LTP_LTD](../02_Cellular_Molecular/LTP_LTD.en.md)
- **Disease**: [Addiction](../08_Neuro_Disorders/Addiction.en.md) (opioid), [Migraine](../08_Neuro_Disorders/Migraine.en.md)

---

## References

1. **Melzack, R. & Wall, P. D.** "Pain mechanisms: a new theory." *Science*, 1965.
2. **Basbaum, A. I. et al.** "Cellular and molecular mechanisms of pain." *Cell*, 2009.
3. **Woolf, C. J.** "Central sensitization: implications for the diagnosis and treatment of pain." *Pain*, 2011.
4. **Tracey, I. & Mantyh, P. W.** "The cerebral signature for pain perception and its modulation." *Neuron*, 2007.
