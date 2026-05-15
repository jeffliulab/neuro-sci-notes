# Metacognition

> *Metacognition is "cognition about cognition" — monitoring of one's own thoughts, knowledge, confidence. Flavell coined the term in 1976. Includes metamemory (knowing whether you know), confidence assessment, error detection. Anterior PFC + parietal are core. Animals and infants show partial metacognition. Neighbor to consciousness; LLM "calibration" is a functional parallel.*
>
> **Difficulty**: Advanced
> **Prerequisites**: [Consciousness](Consciousness.en.md), [Working Memory](Working_Memory.en.md)

---

## 1. Definition + Categories

- **Metacognitive knowledge**: "I know I'm good at X" (self-knowledge)
- **Metacognitive monitoring**: how confident am I now?
- **Metacognitive control**: studying is hard → review / ask for help

---

## 2. Measurement

### 2.1 Confidence Judgment

Report confidence (1-5) after each decision.
Correct + high confidence + incorrect + low confidence = high metacognition.

### 2.2 Metacognitive Sensitivity

Type 2 ROC (Maniscalco & Lau 2012):
- $d'$ measures perception
- meta-$d'$ measures metacognition
- $M_{ratio} = \text{meta-}d' / d'$

### 2.3 Feeling of Knowing (FOK)

"This is on the tip of my tongue" — knows it knows, but retrieval fails.

---

## 3. Neural Basis

- **Anterior PFC** (BA10): main metacognition
- **Dorsolateral PFC**: monitoring + control
- **PCC + precuneus**: self-referential
- **Insula**: confidence → feeling
- Damage → metacognition drops but task performance intact (dissociation)

---

## 4. Dual Process

- **System 1**: fast, intuitive, low metacog
- **System 2**: slow, analytic, high metacog
- Kahneman 2011 *Thinking, Fast and Slow*

---

## 5. Animal Metacognition

- Macaque (Hampton 2001): rejects hard trials → hints at "knowing I don't know"
- Dolphin, rat similar
- But alternative: learned avoidance, not true metacog

---

## 6. Infants + Children

- 3 yo: very weak metacog
- 4-5 yo: passes false belief task, metacog emerges
- Adolescence: large metacog growth (PFC maturation)
- ADHD, autism: metacog deficits

---

## 7. Dunning-Kruger Effect (1999)

- Unskilled overestimate
- Skilled underestimate (know what they don't know)
- Reflects metacog scaling with skill
- Recent replication concerns

---

## 8. PyTorch — Confidence Calibration

```python
import torch
import torch.nn as nn

class CalibratedClassifier(nn.Module):
    """LLM-like temperature calibration."""
    def __init__(self, base_model):
        super().__init__()
        self.base = base_model
        self.temperature = nn.Parameter(torch.tensor(1.0))
    
    def forward(self, x):
        logits = self.base(x)
        return logits / self.temperature
    
    def calibrate(self, val_loader):
        """Fit temperature on validation to minimize NLL."""
        # Standard Platt / temperature scaling
        pass
```

---

## 9. AI / LLM Connection

- **Calibration**: does model confidence match accuracy?
- LLMs often overconfident (esp. reasoning)
- **Verbalized confidence**: ask LLM "how confident?" (partially works)
- **Constitutional AI / RLHF**: improves calibration
- But is LLM truly metacog? Debated

---

## 10. Improving Metacog

- **Reflection**: journal, self-explain
- **Calibration training**: confidence + outcome feedback
- **Education**: teach metacog strategies
- **Mindfulness**: strengthens monitoring

---

## 11. Common Pitfalls

### 11.1 Metacog = Consciousness

No; metacog is a specific function about cognition; consciousness is broader.

### 11.2 High Confidence = Accurate

Wrong; Dunning-Kruger.

### 11.3 LLM Confidence = Real Metacog

May be statistical pattern, not introspection.

### 11.4 Universal Metacog

Large individual differences; depends on training, PFC integrity.

### 11.5 Animal Metacog Confirmed

Still debated; learned response alternative.

---

## 12. Related Concepts

- **Same section**: [Consciousness](Consciousness.en.md), [Decision_Making](Decision_Making.en.md), [Working_Memory](Working_Memory.en.md)
- **Anatomy**: [Cortex](../01_Neuroanatomy/Cortex.en.md) (esp. BA10)
- **AI**: Calibration, constitutional AI

---

## References

1. **Flavell, J. H.** "Metacognitive aspects of problem solving." 1976.
2. **Fleming, S. M. & Dolan, R. J.** "The neural basis of metacognitive ability." *Phil Trans R Soc B*, 2012.
3. **Maniscalco, B. & Lau, H.** "A signal detection theoretic approach for estimating metacognitive sensitivity." *Conscious Cogn*, 2012.
4. **Kahneman, D.** *Thinking, Fast and Slow*. 2011.
