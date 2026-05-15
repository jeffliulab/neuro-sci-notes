# Attention — Selectivity + Limits

> *Attention is the brain's mechanism for selectively processing information. Humans can process ~1 thing at a time (parallel limited). Posner 1980 divides into 3 networks: alerting, orienting, executive. A cognitive bottleneck. AI Transformer attention is a mathematical version.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Cortex](../01_Neuroanatomy/Cortex.en.md)

---

## 1. Types

- **Selective attention**: focus on 1 / ignore others (cocktail party)
- **Divided attention**: multi-task (driving + listening to podcast — limited)
- **Sustained attention**: long-duration focus (vigilance task)
- **Top-down**: goal-driven (find friend in crowd)
- **Bottom-up**: stimulus-driven (loud sudden noise)

---

## 2. Posner 3 Networks

### 2.1 Alerting

- Raises vigilance (preparing to process)
- Involves locus coeruleus NE
- "Ready to receive" signal

### 2.2 Orienting

- Points attention to specific spatial / sensory location
- Involves superior parietal lobule + frontal eye fields
- Overt (eye move) vs covert (no eye move)

### 2.3 Executive

- Resolves conflict (Stroop)
- Involves ACC (Anterior Cingulate) + dlPFC
- Task switching

---

## 3. Visual Attention Neural Basis

- **V1**: spatial attention gates visual signal
- **FEF (Frontal Eye Fields)**: saccade + covert attention
- **PPC (Posterior Parietal Cortex)**: spatial map
- **Pulvinar (thalamus)**: coordinates

---

## 4. Cocktail Party Effect

In noisy environment selectively listen to one person:
- Spatial cue (location)
- Spectral (voice pitch)
- Linguistic
- Top-down knowledge (hear name → attention switch)

---

## 5. Stroop Test

Classic conflict task:
- Show "RED" in red → easy (congruent)
- Show "RED" in blue → slow (incongruent)
- ACC handles conflict

---

## 6. Attention vs AI Transformer

| Aspect | Brain | Transformer |
|---|---|---|
| Bottleneck | yes (limited capacity) | parallel all tokens |
| Top-down | strong | weak (positional encoding) |
| Multi-modal | seamless | requires special encoding |
| Energy | low | high |
| Dynamic | yes | static after train |

Transformer "attention" is mathematical weighted sum; brain attention is selective routing + gain modulation.

---

## 7. PyTorch — Spatial Attention Module

```python
import torch
import torch.nn as nn

class SpatialAttention(nn.Module):
    def __init__(self, channels):
        super().__init__()
        self.conv = nn.Conv2d(channels, 1, 1)
    
    def forward(self, feature_map, attention_loc=None):
        if attention_loc is not None:
            mask = self._gauss_mask(attention_loc, feature_map.shape[-2:])
            attn = torch.sigmoid(self.conv(feature_map)) * mask
        else:
            attn = torch.sigmoid(self.conv(feature_map))
        return feature_map * (1 + attn)
    
    def _gauss_mask(self, loc, shape, sigma=10):
        h, w = shape
        ys, xs = torch.meshgrid(torch.arange(h), torch.arange(w))
        cy, cx = loc
        return torch.exp(-((ys-cy)**2 + (xs-cx)**2) / (2*sigma**2))
```

---

## 8. Pathology / Abnormalities

- **ADHD**: difficulty sustaining attention; DA / NE dysregulation
- **Visual neglect**: parietal damage → ignore one side of visual field
- **Balint syndrome**: bilateral PPC damage → simultanagnosia
- **Hemispatial neglect**: post-stroke ignore opposite side

---

## 9. Training Attention

- Meditation: shown to modify ACC + insula
- Action video games: improves visual attention span
- ADHD: cognitive training + medication (Adderall)

---

## 10. Applications

- UI/UX design (guide user attention)
- Driver fatigue detection (eye tracking + EEG)
- AR/VR (gaze tracking + foveated rendering)
- Marketing (eye tracking)

---

## 11. Common Pitfalls

### 11.1 Multitasking Illusion

Actually fast switching; each switch has cost.

### 11.2 Attention ≠ Consciousness

Can attend without consciously aware (subliminal priming).

### 11.3 "ADHD = Attention Deficit"

Actually attention regulation problem; some ADHD hyperfocus on interesting tasks.

### 11.4 fMRI Assumes Attention = Activation

Could be inhibition / gain change.

### 11.5 Transformer Attention Doesn't Fully Model Brain

---

## 12. Related Concepts

- **Same section**: [Decision Making](Decision_Making.en.md), [Language](Language.en.md), [Consciousness](Consciousness.en.md)
- **AI**: [Self-Attention](https://jeffliulab.github.io/ai-notes/02_Deep_Learning/05_Transformers/Self_Attention/)

---

## References

1. **Posner, M. I. & Petersen, S. E.** "The attention system of the human brain." *Annu Rev Neurosci*, 1990.
2. **Corbetta, M. & Shulman, G. L.** "Control of goal-directed and stimulus-driven attention." *Nat Rev Neurosci*, 2002.
3. **Carrasco, M.** "Visual attention: the past 25 years." *Vis Res*, 2011.
