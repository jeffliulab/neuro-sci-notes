# Reasoning & Problem Solving

> *Reasoning = inferring conclusions from premises: deductive, inductive, abductive, analogical. Problem solving = searching a state space to reach a goal. PFC (esp. rostrolateral) + parietal + working memory are the substrate. Human reasoning is full of heuristic biases (Kahneman/Tversky). A hot human-LLM comparison dimension (chain-of-thought, System 2).*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Executive Function](Executive_Function.en.md), [Decision_Making](Decision_Making.en.md)

---

## 1. Reasoning Types

| Type | Direction | Example |
|---|---|---|
| **Deductive** | General→specific (necessary) | Syllogism |
| **Inductive** | Specific→general (probabilistic) | Scientific induction |
| **Abductive** | Observation→best explanation | Diagnosis |
| **Analogical** | Source→target mapping | Metaphor, transfer |

---

## 2. Neural Basis

- **Rostrolateral PFC (RLPFC/BA10)**: relational integration, analogy, multi-constraint
- **dlPFC**: working memory maintaining premises
- **Parietal (IPS)**: relational / magnitude representation
- **Fronto-parietal network**: fluid reasoning (correlated with g factor)
- Lesion → abstraction / analogy deficit

---

## 3. Classic Tasks

- **Raven's Progressive Matrices**: fluid intelligence gold standard
- **Wason selection task**: deduction (most people fail! → content effect)
- **Tower of Hanoi**: planning / subgoals
- **Analogy A:B::C:?**: relational mapping (RLPFC)
- **Syllogism**: deduction + belief bias

---

## 4. Dual Systems (Kahneman)

- **System 1**: fast, automatic, heuristic, intuitive
- **System 2**: slow, effortful, rule-based, analytic
- Reasoning biases from S1 overstepping + insufficient S2 supervision
- Related to [Metacognition](Metacognition.en.md)

---

## 5. Heuristics and Biases

| Bias | Description |
|---|---|
| Confirmation bias | Prefer confirming hypothesis |
| Belief bias | Conclusion plausibility affects logic judgment |
| Availability | Easily recalled = overestimated probability |
| Anchoring | Anchored on initial value |
| Base-rate neglect | Ignore base rate |
| Conjunction fallacy | "Linda problem" |

---

## 6. PyTorch — Analogical Relational Mapping (toy)

```python
import torch

def analogy_solve(a, b, c, candidates):
    """A:B :: C:? — vector relational mapping (word2vec-style)."""
    relation = b - a                       # extract relation
    target = c + relation                  # apply to C
    sims = torch.nn.functional.cosine_similarity(
        target.unsqueeze(0), candidates)
    return candidates[sims.argmax()]       # best D

# king - man + woman ≈ queen — same relational structure as RLPFC analogy
```

---

## 7. Problem Solving

- **State-space search**: initial → operators → goal (Newell & Simon)
- **Means-ends analysis**
- **Insight**: representational restructuring (aha! — right ATL/cingulate)
- **Expert vs novice**: chunking + pattern recognition (see [Working Memory](Working_Memory.en.md))
- **Functional fixedness**, mental set (obstacles)

---

## 8. Relation to AI / LLM

- LLM "chain-of-thought" ≈ explicit System 2 steps
- LLMs still have human-like biases (inherited from training data)
- Relational reasoning / systematic generalization / compositionality: LLM weaknesses (debated)
- RLPFC relational integration ↔ Transformer relational inductive bias research

---

## 9. Individual Differences

- Fluid reasoning ≈ core of g factor
- Highly correlated with working memory capacity (not identical)
- "Rationality" and intelligence separable (Stanovich)
- Limited training transfer (same brain-training debate)

---

## 10. Common Pitfalls

### 10.1 Humans Are Logical Reasoners

Full of heuristic biases; normative logic not default.

### 10.2 Deduction = Induction

Deduction necessary, induction probabilistic; different neural + error patterns.

### 10.3 Bias = Defect

Heuristics ecologically valid (fast + cheap); bias is a trade-off.

### 10.4 LLM CoT = Real Reasoning

May be pattern; faithfulness debate unresolved.

### 10.5 Reasoning = IQ

Correlated; but rationality ≠ intelligence (separable).

---

## 11. Related Concepts

- **Same section**: [Executive Function](Executive_Function.en.md), [Decision_Making](Decision_Making.en.md), [Metacognition](Metacognition.en.md), [Working Memory](Working_Memory.en.md)
- **Anatomy**: [Cortex](../01_Neuroanatomy/Cortex.en.md) (RLPFC)
- **AI**: chain-of-thought, relational reasoning

---

## References

1. **Kahneman, D.** *Thinking, Fast and Slow*. 2011.
2. **Knowlton, B. J. et al.** "A neurocomputational system for relational reasoning." *Trends Cogn Sci*, 2012.
3. **Newell, A. & Simon, H. A.** *Human Problem Solving*. 1972.
4. **Tversky, A. & Kahneman, D.** "Judgment under uncertainty: heuristics and biases." *Science*, 1974.
