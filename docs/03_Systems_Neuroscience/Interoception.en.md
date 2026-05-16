# Interoception

> *Interoception = perception of internal bodily states (heartbeat, breathing, hunger, viscera). The insula (esp. anterior) is core. Underlies emotion (James-Lange / Damasio somatic markers), self, homeostatic regulation, decision-making. The heartbeat detection task is the classic measure. Tightly linked to anxiety, depression, eating disorders. Core to Friston's active inference framework.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Autonomic_Nervous_System](Autonomic_Nervous_System.en.md), [Emotion](../04_Cognitive_Neuroscience/Emotion.en.md)

---

## 1. Definition + Dimensions

- **Interoception**: perceiving internal physiological states (vs exteroception external)
- Dimensions (Garfinkel):
  - **Accuracy**: objective accuracy (heartbeat counting)
  - **Sensibility**: self-report tendency
  - **Awareness**: metacognition (confidence-accuracy correspondence)
- All three dissociable

---

## 2. Pathway

```
Visceral receptors (mechano/chemo/nociceptive)
   ↓ vagus / spinal lamina I
NTS (nucleus of solitary tract) / parabrachial nucleus
   ↓
Thalamus VMpo
   ↓
Posterior insula → mid insula → anterior insula (AIC)
   ↓
ACC / OFC / amygdala (integration → emotion/value)
```

Anterior insula (AIC) = interoception + subjective feeling integration hub (Craig).

---

## 3. Relation to Emotion Theories

- **James-Lange**: emotion = perception of bodily changes ("see bear→flee→fear")
- **Damasio somatic marker hypothesis**: bodily state signals guide decisions (Iowa gambling, vmPFC)
- **Schachter-Singer two-factor**: physiological arousal + cognitive label
- Anterior insula = "feeling awareness" hub

---

## 4. PyTorch — Somatic Marker Guiding Decision

```python
import torch

def somatic_marker_decision(options, somatic_signal):
    """Bodily (interoceptive) signal biases choice before explicit reasoning."""
    # somatic_signal: learned visceral 'gut feeling' per option (vmPFC/insula)
    explicit_value = options.mean(dim=1)
    gut_bias = torch.tanh(somatic_signal)          # anticipatory bodily state
    decision_value = explicit_value + 0.6 * gut_bias
    return decision_value.argmax()
```

---

## 5. Heartbeat Detection Task

- **Heartbeat counting / discrimination**: classic accuracy measure
- High interoceptive accuracy → stronger emotional experience, better intuitive decisions (some studies)
- But task validity debated (may rely on heart rate knowledge, not true perception)
- Trend: respiration / gastric / multi-channel + neural measures

---

## 6. Active Inference View

- Friston: brain predicts internal states → minimizes error via **autonomic action** (allostasis)
- Interoception = generative model of the body (see [Free Energy Principle](../05_Computational_Neuroscience/Free_Energy_Principle.en.md))
- Emotion = result of interoceptive inference
- "Allostasis": predictive regulation (not passive homeostasis)

---

## 7. Clinical

- **Anxiety / panic**: interoceptive hypersensitivity + misinterpretation (heartbeat → "heart attack")
- **Depression**: interoceptive blunting
- **Eating disorders**: hunger/satiety signal dysregulation (insula abnormality)
- **Alexithymia**: difficulty identifying emotions ~ interoceptive deficit
- **Somatic symptom disorder**, PTSD
- Interoception training (intervention direction)

---

## 8. Relation to Self / Consciousness

- Bodily basis of minimal self (see [Self_and_Agency](../04_Cognitive_Neuroscience/Self_and_Agency.en.md))
- "Embodied" emotion + self (see [Embodied_Cognition](../04_Cognitive_Neuroscience/Embodied_Cognition.en.md))
- Bodily-root theory of consciousness (Damasio core consciousness)

---

## 9. Relation to AI

- LLM has no body → no interoception → one argument for "no true emotion"
- Affective computing: external physiology (HR/GSR) ≈ interoception proxy
- Homeostatic RL: internal state as reward (Keramati & Gutkin)
- Embodied agents need internal-state model (see [Embodied_Cognition](../04_Cognitive_Neuroscience/Embodied_Cognition.en.md))

---

## 10. Common Pitfalls

### 10.1 Interoception = Heartbeat Perception

It's multi-channel (respiration/hunger/pain/viscera); heartbeat is just a common measure.

### 10.2 Accuracy = Awareness

Three dimensions dissociable (Garfinkel); high report ≠ high accuracy.

### 10.3 Interoception Passive

Active inference / allostasis: predictive regulation.

### 10.4 Unrelated to Emotion

James-Lange/Damasio: interoception is emotion's core basis.

### 10.5 Heartbeat Counting Task Flawless

May rely on heart-rate knowledge; validity debated.

---

## 11. Related Concepts

- **Same section**: [Autonomic_Nervous_System](Autonomic_Nervous_System.en.md), [Hypothalamus_Homeostasis](Hypothalamus_Homeostasis.en.md), [Pain_System](Pain_System.en.md)
- **Cognition**: [Emotion](../04_Cognitive_Neuroscience/Emotion.en.md), [Self_and_Agency](../04_Cognitive_Neuroscience/Self_and_Agency.en.md), [Decision_Making](../04_Cognitive_Neuroscience/Decision_Making.en.md)
- **Computational**: [Free Energy Principle](../05_Computational_Neuroscience/Free_Energy_Principle.en.md)

---

## References

1. **Craig, A. D.** "How do you feel? Interoception: the sense of the physiological condition of the body." *Nat Rev Neurosci*, 2002.
2. **Damasio, A. R.** *Descartes' Error*. 1994.
3. **Garfinkel, S. N. et al.** "Knowing your own heart: distinguishing interoceptive accuracy from awareness." *Biol Psychol*, 2015.
4. **Seth, A. K.** "Interoceptive inference, emotion, and the embodied self." *Trends Cogn Sci*, 2013.
