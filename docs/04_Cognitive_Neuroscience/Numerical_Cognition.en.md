# Numerical Cognition

> *Numerical cognition studies how the brain represents + manipulates quantity. Approximate Number System (ANS): innate, logarithmically compressed, Weber's law, shared by human infants + animals. Exact symbolic numbers: culturally acquired, IPS (intraparietal sulcus) is core. Dehaene's "number sense." Dyscalculia is its disorder. Connects perception, space (mental number line), and education.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Somatosensory](../03_Systems_Neuroscience/Somatosensory.en.md), [Reasoning_Problem_Solving](Reasoning_Problem_Solving.en.md)

---

## 1. Two Systems

| System | Feature |
|---|---|
| **ANS** (approximate) | Innate, non-symbolic, logarithmic, approximate, animal-shared |
| **Exact symbolic** | Culturally acquired, exact, needs language/symbols |
| **Subitizing** | ≤ 4 items instant exact counting (parallel) |

---

## 2. ANS Properties

- **Weber's law**: discrimination depends on ratio ($\Delta n / n$ constant)
- **Distance effect**: 8 vs 9 harder than 2 vs 9
- **Size effect**: large numbers harder to discriminate
- Infants (Xu & Spelke), monkeys, fish, bees all have it → evolutionarily ancient

---

## 3. Neural Basis

- **IPS (intraparietal sulcus)**: core of quantity representation (across symbolic/non-symbolic/modality)
- HIPS (horizontal IPS): abstract quantity
- Angular gyrus: symbolic arithmetic retrieval (verbal)
- PFC: working memory + complex calculation
- "Number neurons" (monkey PFC/IPS, Nieder) — tuned to specific numerosity

---

## 4. Mental Number Line

- Number ↔ space mapping (small left, large right, Western culture)
- **SNARC effect**: small numbers faster left hand, large faster right
- Overlaps with spatial attention / parietal
- Culture-dependent (reading direction affects orientation)

---

## 5. PyTorch — Numerosity Tuning + Log

```python
import torch

def number_neuron(n, preferred, sigma=0.3):
    """Tuning curve on LOG scale (Weber-Fechner) — like IPS number neurons."""
    return torch.exp(-((torch.log(torch.tensor(float(n)))
                        - torch.log(torch.tensor(float(preferred)))) ** 2)
                     / (2 * sigma ** 2))

# Distance + size effects emerge naturally from log-Gaussian tuning
```

---

## 6. Development

- Infants have ANS (large-number comparison, addition expectation violation)
- ANS acuity correlated with later math achievement (debated strength)
- Symbol acquisition = building ANS↔symbol mapping ("symbol grounding")
- Finger counting → embodied (see [Embodied_Cognition](Embodied_Cognition.en.md))

---

## 7. Dyscalculia

- ~ 3-7%, math-domain-specific learning disorder
- Hypotheses: ANS deficit / symbol-quantity mapping deficit / working memory
- IPS structural/activation abnormality
- Comorbid with dyslexia but separable

---

## 8. Animal Number Sense

- Monkeys: ordinal + cardinal + simple arithmetic
- Bees: zero concept, addition/subtraction
- Fish / birds: quantity discrimination
- → ANS is cross-species conserved (not language-dependent)

---

## 9. Relation to AI

- NN trained to count → spontaneous numerosity tuning (IPS-like, Nasr 2019)
- LLM arithmetic weak (tokenization + no symbol grounding)
- "Number sense" is a microcosm of the grounding problem
- Tool augmentation (calculator) ≈ humans using external symbols

---

## 10. Common Pitfalls

### 10.1 Number Sense = Arithmetic Ability

ANS (approximate) ≠ symbolic exact arithmetic; doubly dissociable.

### 10.2 Number Representation Linear

ANS is log-compressed (Weber); children earlier more logarithmic.

### 10.3 Only Humans Have Number Sense

ANS cross-species (monkey/bee/fish); not language-dependent.

### 10.4 Subitizing = Fast Counting

≤ 4 parallel direct perception, mechanism differs from sequential counting.

### 10.5 Mental Number Line Universal

Orientation culture-dependent (reading direction).

---

## 11. Related Concepts

- **Same section**: [Reasoning_Problem_Solving](Reasoning_Problem_Solving.en.md), [Working Memory](Working_Memory.en.md), [Embodied_Cognition](Embodied_Cognition.en.md)
- **Anatomy**: [Cortex](../01_Neuroanatomy/Cortex.en.md) (IPS)
- **Disease**: Dyscalculia
- **AI**: numerosity in NN, symbol grounding

---

## References

1. **Dehaene, S.** *The Number Sense*. Oxford, 1997.
2. **Nieder, A.** "The neuronal code for number." *Nat Rev Neurosci*, 2016.
3. **Xu, F. & Spelke, E. S.** "Large number discrimination in 6-month-old infants." *Cognition*, 2000.
4. **Nasr, K. et al.** "Number detectors spontaneously emerge in a deep neural network." *Sci Adv*, 2019.
