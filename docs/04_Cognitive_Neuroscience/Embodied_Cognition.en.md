# Embodied Cognition

> *Embodied cognition argues cognition is not abstract symbol manipulation but **rooted in body, sensorimotor systems, environmental interaction**. Challenges the classic "computer metaphor." Evidence: action-language coupling, mirror system, grounded language, conceptual metaphor. Major AI impact: symbol grounding problem, embodied intelligence (robot learning). Theoretical basis of the LLM "no body" critique.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Social_Cognition](Social_Cognition.en.md), [Language](Language.en.md)

---

## 1. Core Claim

- Cognition ≠ disembodied symbol computation (anti classic cognitivism)
- Concepts **rooted** in sensorimotor experience (grounding)
- Body morphology + environment form part of cognition ("4E": embodied, embedded, enacted, extended)

---

## 2. Lines of Evidence

| Evidence | Description |
|---|---|
| Action-language coupling | Reading "kick" activates motor cortex leg area (Pulvermüller) |
| Mirror system | Observing action → own motor representation (see [Social_Cognition](Social_Cognition.en.md)) |
| Conceptual metaphor | "warm person", "heavy decision" (Lakoff & Johnson) |
| Sensorimotor simulation | Understanding = mental simulation of sensorimotor |
| Gesture-thought | Gestures aid thinking (not just communication) |

---

## 3. 4E Cognition

- **Embodied**: body structure shapes cognition
- **Embedded**: environment supports (offloading)
- **Enacted**: cognition = active sensorimotor loop (Varela)
- **Extended**: tools/notes are cognitive system extensions (Clark & Chalmers)

---

## 4. Grounded Cognition (Barsalou)

- Concept = partial reenactment of sensorimotor states (simulation)
- Opposes amodal symbols (traditional propositional representation)
- Perceptual symbol systems
- fMRI: concept activation in corresponding sensorimotor areas

---

## 5. PyTorch — Grounded vs Symbolic Representation

```python
import torch

# Symbolic (amodal): arbitrary one-hot, no sensorimotor content
symbolic_cup = torch.eye(100)[7]                     # token #7

# Grounded: concept = bundle of sensorimotor features
grounded_cup = torch.tensor([
    0.9,  # graspable (motor)
    0.8,  # contains liquid (function)
    0.6,  # cylindrical (visual)
    0.3,  # warm-if-coffee (tactile)
])
# Grounded supports inference/transfer; symbolic does not (grounding problem)
```

---

## 6. Significance for AI

- **Symbol grounding problem** (Harnad 1990): where does symbol meaning come from?
- LLM critique: text training → no sensorimotor grounding ("stochastic parrot" debate)
- **Embodied AI**: robots learn through bodily interaction (see ai-notes embodied intelligence)
- Multimodal + embodied is a grounding path

---

## 7. Strong vs Weak Embodiment

- **Strong**: cognition **requires** body (no body, no concepts)
- **Weak**: body **influences/shapes** cognition but not required
- Most evidence supports weak; strong has counterexamples (abstract concepts, congenitally limbless people's concepts)
- Current consensus: simulation is **part** of cognition's mechanism

---

## 8. Applications

- Education: embodied learning (gesture / manipulation → math/science, see [Numerical_Cognition](Numerical_Cognition.en.md))
- Rehabilitation: mirror therapy, action observation therapy
- HCI / VR: embodied interaction
- Clinical: abstract concept impairment (semantic dementia)

---

## 9. Criticisms

- Abstract concepts (democracy, prime number) hard to ground purely sensorimotor
- Neural activation ≠ causally necessary (could be epiphenomenal)
- "Grounding by association" vs truly constitutive
- Rising LLM capability → reignites "need body?" debate

---

## 10. Common Pitfalls

### 10.1 All Cognition Needs Body (Strong)

Abstract concept counterexamples; most support weak embodiment.

### 10.2 Neural Activation = Necessary

Correlation ≠ causation; need lesion / TMS to verify constitutive.

### 10.3 Embodied = Anti-Symbol Entirely

Complements/revises classic symbolism, not full rejection.

### 10.4 LLM No Body = No Understanding

Debate unresolved; multimodal + embodied open paths.

### 10.5 Metaphor = Pure Rhetoric

Lakoff: conceptual metaphor shapes reasoning (not just linguistic decoration).

---

## 11. Related Concepts

- **Same section**: [Social_Cognition](Social_Cognition.en.md), [Language](Language.en.md), [Numerical_Cognition](Numerical_Cognition.en.md), [Consciousness](Consciousness.en.md)
- **Systems**: [Motor System](../03_Systems_Neuroscience/Motor_System.en.md)
- **AI**: symbol grounding, embodied AI — https://jeffliulab.github.io/ai-notes/08_Embodied_Intelligence/

---

## References

1. **Barsalou, L. W.** "Grounded cognition." *Annu Rev Psychol*, 2008.
2. **Lakoff, G. & Johnson, M.** *Metaphors We Live By*. 1980.
3. **Pulvermüller, F.** "Brain mechanisms linking language and action." *Nat Rev Neurosci*, 2005.
4. **Clark, A. & Chalmers, D.** "The extended mind." *Analysis*, 1998.
