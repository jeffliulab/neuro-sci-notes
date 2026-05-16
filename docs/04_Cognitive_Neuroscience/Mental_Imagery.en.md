# Mental Imagery

> *Mental imagery is "seeing/hearing in the mind" without external stimulus. The Kosslyn-Pylyshyn debate (depictive analog vs propositional) is a cognitive science classic. Evidence: imagery activates sensory cortex (V1!), shares mechanisms with perception. Aphantasia (no imagery) is an extreme individual difference. Connects perception, memory, imagination, creativity.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Visual System](../03_Systems_Neuroscience/Visual_System.en.md), [Working Memory](Working_Memory.en.md)

---

## 1. Core Question

- When imagining a cat, is the brain **depictive** (image-like) or **propositional**?
- **Kosslyn**: analog representation (spatial, has a "picture")
- **Pylyshyn**: propositional + tacit knowledge (no real "picture")
- Neural evidence leans Kosslyn (but propositional component exists)

---

## 2. Classic Behavioral Evidence

- **Mental rotation** (Shepard & Metzler 1971): reaction time ∝ rotation angle (linear!) → analog representation
- **Image scanning**: scan distance ∝ time
- **Image size effect**: small image detail judgment slower
- → Behavioral patterns like operating a real spatial representation

---

## 3. Neural Evidence

- **V1 activation**: vivid visual imagery activates early visual cortex (Kosslyn fMRI)
- **Topographic mapping**: imagery preserves retinotopy in V1 (large image occupies more V1)
- **Perception-imagery sharing**: decoders generalize across perception/imagery
- **Reverse hierarchy**: imagery more dependent on top-down (frontoparietal → sensory)

---

## 4. PyTorch — Perception vs Imagery (shared decoder)

```python
import torch

def perception_vs_imagery(stimulus, top_down, mode='perceive'):
    """Shared sensory representation; imagery is top-down driven."""
    if mode == 'perceive':
        rep = stimulus + 0.3 * top_down            # bottom-up dominant
    else:  # imagine: no stimulus, top-down generates pattern
        rep = top_down                              # generative, weaker, noisier
    return torch.tanh(rep)
# A decoder trained on perception partially generalizes to imagery
```

---

## 5. Aphantasia / Hyperphantasia

- **Aphantasia**: no voluntary visual imagery (~ 1-4%, Zeman 2015 named)
- **Hyperphantasia**: imagery vivid as perception
- Extreme individual difference (continuum)
- Aphantasics still do spatial reasoning (propositional/other strategies) → imagery not required for reasoning

---

## 6. Multimodal Imagery

- Visual (most studied), auditory (mental melody), motor (motor imagery), olfactory/gustatory
- **Motor imagery**: activates motor system (without actual movement) → motor rehab + BCI use
- Shares with corresponding sensory/motor cortex

---

## 7. Functional Roles

- **Memory**: vivid imagery → better encoding (method of loci)
- **Planning/simulation**: rehearse future (prospection, shares network with episodic)
- **Imagination/creativity**: recombine representations (see [Creativity](Creativity.en.md))
- **Emotion**: imagery stronger emotion than words (imagery exposure therapy)

---

## 8. Clinical + Applications

- **PTSD**: intrusive imagery (see [PTSD](../08_Neuro_Disorders/PTSD.en.md))
- **Imagery exposure/rescripting**: therapy
- **Motor imagery BCI**: imagine action → control (see [EEG](../07_Neurotech_Frontiers/EEG.en.md))
- **Motor training**: imagery practice improves performance

---

## 9. Relation to AI

- Generative models (diffusion) ≈ top-down "imagining" images
- "Mental simulation" ↔ world model rollout (see ai-notes world models)
- fMRI imagery decoding/reconstruction (Takagi 2023 + diffusion)
- Perception-imagery sharing ↔ encoder-decoder reuse

---

## 10. Common Pitfalls

### 10.1 Imagery = Real Picture in Brain

depictive vs propositional long debate; functional analog, not literal pixels.

### 10.2 V1 Activation Proves Pure Image

V1 involved but top-down + propositional components coexist.

### 10.3 Everyone Has Vivid Imagery

Aphantasia↔hyperphantasia huge individual differences.

### 10.4 No Imagery = Can't Think Visual Problems

Aphantasics use alternative strategies, still spatial reasoning.

### 10.5 Imagery = Weak Perception

Mechanisms overlap but opposite direction (top-down dominant) + generative.

---

## 11. Related Concepts

- **Same section**: [Working Memory](Working_Memory.en.md), [Creativity](Creativity.en.md), [Consciousness](Consciousness.en.md), [Embodied_Cognition](Embodied_Cognition.en.md)
- **Systems**: [Visual System](../03_Systems_Neuroscience/Visual_System.en.md)
- **Disease**: [PTSD](../08_Neuro_Disorders/PTSD.en.md)
- **AI**: generative models, world model

---

## References

1. **Shepard, R. N. & Metzler, J.** "Mental rotation of three-dimensional objects." *Science*, 1971.
2. **Kosslyn, S. M. et al.** "Neural foundations of imagery." *Nat Rev Neurosci*, 2001.
3. **Pearson, J.** "The human imagination: the cognitive neuroscience of visual mental imagery." *Nat Rev Neurosci*, 2019.
4. **Zeman, A. et al.** "Lives without imagery — congenital aphantasia." *Cortex*, 2015.
