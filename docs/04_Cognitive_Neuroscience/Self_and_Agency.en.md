# Self & Sense of Agency

> *"Self" is not unitary: includes minimal self (body ownership + agency) and narrative self (autobiographical narrative). Sense of agency = "I did that." The comparator model (prediction vs feedback) explains agency + why you can't tickle yourself. DMN + insula + TPJ are the substrate. Schizophrenia (passivity experiences), rubber hand illusion are key evidence.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Social_Cognition](Social_Cognition.en.md), [Consciousness](Consciousness.en.md)

---

## 1. Levels of Self

| Level | Content | Region |
|---|---|---|
| **Minimal self** | Body ownership + agency (now) | insula, TPJ, SMA |
| **Narrative self** | Autobiography, identity, temporal continuity | mPFC, PCC/precuneus (DMN) |

---

## 2. Two Components (minimal self)

- **Ownership**: "this is my body"
- **Agency**: "this action was initiated by me"
- Dissociable: rubber hand illusion (ownership mislocated) vs passive movement (no agency, has ownership)

---

## 3. Comparator Model (Agency)

```
Intention → motor command
   ├→ Efference copy → Forward model → predicted sensory result
   │                                      ↓ compare
   └→ Actual action → real sensory feedback ──────→ match?
Match → strong agency + sensory attenuation
Mismatch → weak agency / external attribution
```

→ Explains why you **can't tickle yourself** (prediction → attenuates self-generated stimulus).

---

## 4. PyTorch — Comparator + Sensory Attenuation

```python
import torch

def sense_of_agency(motor_cmd, actual_feedback, forward_model_gain=0.9):
    """Predicted == actual -> high agency + attenuated sensation."""
    predicted = forward_model_gain * motor_cmd          # efference copy
    mismatch = (actual_feedback - predicted).abs()
    agency = torch.exp(-3.0 * mismatch)                 # high if match
    perceived = actual_feedback - 0.7 * predicted        # self-gen attenuated
    return agency, perceived
```

---

## 5. Rubber Hand Illusion (RHI)

- Synchronous brushing of real hand (hidden) + fake hand (visible) → fake hand "belongs to me"
- Multisensory integration (visuo-tactile) overrides proprioception
- Proves body ownership is **inferred** (manipulable)
- Full-body illusion (out-of-body) similar (TPJ)

---

## 6. Neural Basis

- **Insula (anterior)**: interoception → bodily self + agency
- **TPJ**: self-other distinction, perspective, agency mismatch detection
- **SMA / pre-SMA**: intention + efference
- **mPFC / PCC (DMN)**: narrative self, self-referential processing
- **Angular gyrus**: agency mismatch

---

## 7. Pathology

- **Schizophrenia**: passivity experiences, thought insertion = agency deficit (comparator dysregulation, weak attenuation → self-generated attributed external)
- **Anarchic/alien hand**: action without agency (SMA/corpus callosum)
- **Depersonalization**: self-detachment feeling
- **Anosognosia**: denial of deficit (right parietal)
- **Somatoparaphrenia**: denial of limb ownership

---

## 8. Default Mode Network

- DMN (mPFC, PCC, angular) = self-referential + autobiographical memory + mind-wandering
- "Resting state" activation; suppressed during task
- Core of narrative self (see [Consciousness](Consciousness.en.md))
- Overlaps with [Memory_Systems](Memory_Systems.en.md) episodic

---

## 9. Relation to AI

- Forward model / efference copy ↔ robot self-model, predictive control
- "Sense of agency" ↔ agent credit assignment / intention modeling
- LLM has no unified self / body (persona is pattern, not minimal self)
- Self-modeling = open embodied intelligence problem (see [Embodied_Cognition](Embodied_Cognition.en.md))

---

## 10. Common Pitfalls

### 10.1 Self Is a Single Entity

At least minimal vs narrative multi-component (dissociable lesions).

### 10.2 Body Ownership Directly Given

It's multisensory **inference** (rubber hand manipulable).

### 10.3 Agency = Intention

Needs comparator match; intention can exist yet lose agency (alien hand).

### 10.4 Sensory Attenuation Irrelevant

It's an agency marker; dysregulation → schizophrenia passivity experiences.

### 10.5 DMN = "Doing Nothing"

It's an active self/memory/simulation network, not idle.

---

## 11. Related Concepts

- **Same section**: [Consciousness](Consciousness.en.md), [Social_Cognition](Social_Cognition.en.md), [Embodied_Cognition](Embodied_Cognition.en.md), [Memory_Systems](Memory_Systems.en.md)
- **Systems**: [Motor System](../03_Systems_Neuroscience/Motor_System.en.md)
- **Disease**: [Schizophrenia](../08_Neuro_Disorders/Schizophrenia.en.md)
- **AI**: self-model, predictive control

---

## References

1. **Gallagher, S.** "Philosophical conceptions of the self: implications for cognitive science." *Trends Cogn Sci*, 2000.
2. **Blakemore, S.-J. et al.** "Why can't you tickle yourself?" *NeuroReport*, 2000.
3. **Botvinick, M. & Cohen, J.** "Rubber hands 'feel' touch that eyes see." *Nature*, 1998.
4. **Frith, C. D. et al.** "Abnormalities in the awareness and control of action." *Phil Trans R Soc B*, 2000.
