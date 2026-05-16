# Cognitive Development

> *How does cognition build from infant to adult? Piaget's stage theory → modern view: infant core knowledge far exceeds Piaget's estimate + gradual + domain-specific. Theory of mind ~ 4 years, EF to 25 years (PFC myelination). Nativism vs empiricism → modern constructivism + Bayesian "child as scientist." An inspiration for AI developmental learning.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Neurodevelopment](../00_Foundations/Neurodevelopment.en.md), [Social_Cognition](Social_Cognition.en.md)

---

## 1. Piaget's Four Stages (classic, partly revised)

| Stage | Age | Hallmark |
|---|---|---|
| Sensorimotor | 0-2 | Object permanence |
| Preoperational | 2-7 | Symbols / language; egocentric, no conservation |
| Concrete operational | 7-11 | Conservation, logic (concrete) |
| Formal operational | 11+ | Abstract, hypothetico-deductive |

→ Pioneering framework, but underestimates infants + stages too rigid.

---

## 2. Core Knowledge (modern revision)

Infants innately (or very early) have domain-specific systems (Spelke):
- **Objects** (permanence, continuity) — far earlier than Piaget (violation-of-expectation)
- **Number** (ANS, see [Numerical_Cognition](Numerical_Cognition.en.md))
- **Agents/intention** (goal-directed)
- **Space** (geometry)
- Later learning builds on these core systems

---

## 3. Key Milestones

- Object permanence: ~ 3-4 months (looking time) not Piaget's 8 months
- Joint attention: ~ 9-12 months
- Theory of Mind (false belief): ~ 4 years (implicit earlier) — see [Social_Cognition](Social_Cognition.en.md)
- Language explosion: ~ 18-24 months
- EF / abstract reasoning: adolescence-25 years (PFC, see [Executive Function](Executive_Function.en.md))

---

## 4. PyTorch — Violation-of-Expectation (VOE) Paradigm Logic

```python
import torch

def violation_of_expectation(predicted, observed, infant_model):
    """Longer looking = surprise = expectation violated (Spelke method)."""
    surprise = (predicted - observed).abs().mean()
    looking_time = 1.0 + 3.0 * torch.sigmoid(surprise - 0.5)
    # If infant has 'object permanence' prior, impossible event -> high surprise
    return looking_time
```

---

## 5. Learning Mechanisms

- **Statistical learning**: infants extract speech / visual transitional probabilities (Saffran 1996)
- **Bayesian "child as scientist"**: active hypothesis testing (Gopnik)
- **Social learning**: imitation, natural pedagogy
- **Constructivism**: core knowledge + experience → gradual refinement
- Not pure blank slate, not pure innate → interaction

---

## 6. Nature vs Nurture (dichotomy transcended)

- Extreme nativism / empiricism both refuted
- Modern: **probabilistic epigenesis**, gene×environment interaction
- Experience-dependent plasticity + critical periods (see [Neurodevelopment](../00_Foundations/Neurodevelopment.en.md))
- Individual differences = genetics + environment + random developmental noise

---

## 7. Neural Development Correlates

- Sensory cortex matures early → association/PFC latest (~25 years)
- Synaptic overproduction → pruning (experience shapes)
- Myelination back-to-front (PFC last) → explains adolescent risky decisions
- Critical periods (language, vision)

---

## 8. Relation to AI / Developmental ML

- "Child as scientist" ↔ active learning, curiosity-driven RL
- Core knowledge ↔ inductive bias, object-centric models
- Curriculum learning ↔ developmental order
- Few-shot / rapid generalization: children far exceed current AI (sample efficiency gap)

---

## 9. Clinical

- Developmental delay / ID
- Autism: social cognition + ToM trajectory abnormal (see [Autism](../08_Neuro_Disorders/Autism.en.md))
- Deprivation (Romanian orphans) → sensitive period + plasticity evidence
- Early intervention window

---

## 10. Common Pitfalls

### 10.1 Piaget Stages Strict + Complete

Underestimates infants; stages not rigid, domain-specific + gradual.

### 10.2 Infant Is a Blank Slate

Core knowledge: innate object/number/agent systems.

### 10.3 Nature vs Nurture Either-Or

Replaced by interactionism / probabilistic epigenesis.

### 10.4 ToM Suddenly Appears at 4

Explicit ~4 years, implicit earlier (gradual).

### 10.5 Development = Monotone Progress

Has U-shaped / regression / reorganization (nonlinear).

---

## 11. Related Concepts

- **Same section**: [Social_Cognition](Social_Cognition.en.md), [Executive Function](Executive_Function.en.md), [Numerical_Cognition](Numerical_Cognition.en.md), [Language](Language.en.md)
- **Foundation**: [Neurodevelopment](../00_Foundations/Neurodevelopment.en.md)
- **Disease**: [Autism](../08_Neuro_Disorders/Autism.en.md)
- **AI**: developmental learning, inductive bias

---

## References

1. **Piaget, J.** *The Origins of Intelligence in Children*. 1952.
2. **Spelke, E. S. & Kinzler, K. D.** "Core knowledge." *Dev Sci*, 2007.
3. **Gopnik, A. et al.** *The Scientist in the Crib*. 1999.
4. **Saffran, J. R. et al.** "Statistical learning by 8-month-old infants." *Science*, 1996.
