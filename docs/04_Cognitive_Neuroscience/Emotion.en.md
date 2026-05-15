# Emotion — Amygdala and Emotion Networks

> *Emotions are brain's rapid evaluation + reaction to stimuli. Amygdala is core for fear / threat, but emotion involves wide cortical / subcortical network. Basic emotions vs constructed emotion is current debate.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Cortex](../01_Neuroanatomy/Cortex.en.md), [Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.en.md)

---

## 1. Classical vs Modern

### 1.1 Classical "Basic Emotions" (Ekman 1972)

- 6 universal: happy, sad, fear, anger, disgust, surprise
- Cross-cultural facial expression consistency
- Each emotion has distinct neural signature

### 1.2 Constructed Emotion (Barrett 2017)

- Emotion is brain actively constructed (based on prior + context)
- Not discrete categories
- Same brain signal + different context → different emotion experience
- Consistent with predictive coding

→ Two camps active debate.

---

## 2. Main Emotion-Related Regions

```
Amygdala — fear / threat detection
Insula — disgust, interoception
Anterior Cingulate (ACC) — distress, empathy
vmPFC — emotion regulation, value
Hypothalamus — autonomic + endocrine response
PAG (brainstem) — defensive behavior
NAcc / VTA — reward, pleasure
```

---

## 3. Amygdala Detail

- ~13 nuclei combined
- Lateral nucleus: input (sensory + cortex)
- Central nucleus: output (autonomic + behavior via hypothalamus, brainstem)
- BLA (basolateral): fear learning + memory
- Damage → reduced fear (Urbach-Wiethe disease)

---

## 4. Fear Conditioning

Classic Pavlovian:
1. CS (neutral, e.g., tone) + US (aversive, e.g., shock) paired
2. Learns CS → predicts US
3. Amygdala LTP at CS pathway
4. CS alone → freeze (CR, conditioned response)

Model for PTSD, anxiety.

---

## 5. Emotion Regulation

PFC top-down regulates amygdala:
- **Reappraisal**: re-evaluate situation
- **Suppression**: hide behavioral expression
- **Mindfulness**: observe without judgment

Reappraisal > suppression for long-term wellbeing (Gross 2002).

---

## 6. Autonomic Response

Emotion → physiological:
- HR speeds up (sympathetic)
- Skin conductance ↑ (sweat)
- Pupils dilate
- Breathing deep / fast
- GI ↓

→ Basis for polygraph (lie detection) (but only ~65% accuracy).

---

## 7. PyTorch — Emotion Classification from Face

```python
import torch
import torch.nn as nn

class EmotionClassifier(nn.Module):
    def __init__(self):
        super().__init__()
        self.backbone = nn.Sequential(
            nn.Conv2d(3, 64, 3, padding=1), nn.ReLU(), nn.MaxPool2d(2),
            nn.Conv2d(64, 128, 3, padding=1), nn.ReLU(), nn.MaxPool2d(2),
            nn.AdaptiveAvgPool2d(1),
        )
        self.fc = nn.Linear(128, 6)
    
    def forward(self, face):
        return self.fc(self.backbone(face).flatten(1))
```

---

## 8. Pathology

- **PTSD**: amygdala hyperactivity + PFC insufficient inhibition
- **Anxiety**: similar to PTSD but unclear trigger
- **Depression**: overall emotion processing dysregulation
- **Psychopathy**: low amygdala reactivity (low empathy / fear)
- **Capgras delusion**: recognize but no emotion → feels like impersonation
- **Klüver-Bucy syndrome**: bilateral amygdala damage → fearless + hypersexual / hyperphagia

---

## 9. AI / Affective Computing

- Facial expression recognition (FER)
- Voice emotion classification (audio)
- Multi-modal emotion AI (face + voice + text + physiology)
- Empathy in chatbots (Replika)
- Applications: mental health screening, marketing, gaming

---

## 10. Common Pitfalls

### 10.1 6 Basic Emotions Not Absolute

Cultural differences (collectivism vs individualism).

### 10.2 Amygdala Beyond Fear

Also positive emotions (anticipation reward).

### 10.3 Polygraph Inaccurate

Physical response can be triggered by anxiety / unfamiliarity.

### 10.4 Emotion ≠ Feeling

Emotion: neurobiological; feeling: conscious experience.

### 10.5 Suppressing Emotion Harmful

Long-term suppression increases stress, depression risk.

---

## 11. Related Concepts

- **Same section**: [Attention](Attention.en.md), [Decision Making](Decision_Making.en.md), [Consciousness](Consciousness.en.md)
- **Cellular**: [Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.en.md)
- **Diseases**: [Depression](../08_Neuro_Disorders/Depression.en.md)

---

## References

1. **LeDoux, J. E.** *The Emotional Brain*. 1996.
2. **Barrett, L. F.** *How Emotions Are Made*. 2017.
3. **Ekman, P.** "Universal facial expressions." *Science*, 1969.
4. **Gross, J. J.** "Emotion regulation: Affective, cognitive, and social consequences." *Psychophysiology*, 2002.
