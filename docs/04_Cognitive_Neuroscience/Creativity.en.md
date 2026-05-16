# Creativity

> *Creativity = producing ideas that are both **novel** and **useful**. Not a single brain region: relies on dynamic switching between default mode network (generation) ↔ executive control network (evaluation). Divergent vs convergent thinking. Comparison with LLM generation is a current focus (can AI "truly create").*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Executive Function](Executive_Function.en.md), [Mental_Imagery](Mental_Imagery.en.md)

---

## 1. Definition

- Creative product = **novel** + **useful/appropriate**
- Both criteria essential (pure random novelty ≠ creativity)
- Levels: little-c (everyday) ↔ Big-C (transformative)

---

## 2. Divergent vs Convergent

| | Divergent thinking | Convergent thinking |
|---|---|---|
| Direction | Many solutions, spread | Single optimal solution |
| Measure | AUT (alternative uses), fluency/flexibility/originality | RAT (remote associates) |
| Network | DMN dominant | ECN dominant |

Creativity = **dynamic collaboration** of both.

---

## 3. Network Model

```
Default Mode Network (DMN)
  ↳ spontaneous generation, association, mind-wandering
        ⇅ dynamic switching + coupling
Executive Control Network (ECN)
  ↳ evaluation, filtering, constraint, goal-directed
Salience Network
  ↳ switches/scheduling between the two
```

→ Creativity is not the "right brain" myth; it's dynamic inter-network interaction (Beaty 2016).

---

## 4. Neural Evidence

- High creatives: DMN-ECN **enhanced coupling** (usually anticorrelated)
- Aha! insight: right ATL + anterior cingulate (restructuring)
- Dopamine: exploration / novelty (moderate; inverted-U)
- Frontal lesion: sometimes disinhibition → abnormal "creativity" (paradoxical)

---

## 5. PyTorch — Divergent Generation + Convergent Selection

```python
import torch

def creative_process(seed, n_candidates=20, temperature=1.5):
    """DMN: high-temperature divergent generation; ECN: convergent select."""
    # Divergent: many varied candidates (high entropy)
    candidates = seed + temperature * torch.randn(n_candidates, seed.numel())
    # Convergent: score novelty * usefulness, pick best
    novelty = (candidates - seed).norm(dim=1)
    usefulness = torch.sigmoid(-(candidates.abs().mean(dim=1) - 1.0))
    score = novelty * usefulness                    # both required
    return candidates[score.argmax()]
```

---

## 6. Stage Model (Wallas 1926)

1. **Preparation**: immerse in problem
2. **Incubation**: set aside → unconscious processing (DMN / sleep)
3. **Illumination**: insight (aha!)
4. **Verification**: evaluate + implement (ECN)

Incubation effect: leaving the problem promotes solution (attention + DMN).

---

## 7. Individual + State Factors

- **Openness** (Big Five): most robust personality correlate
- Moderate psychopathology (hypomania, schizotypy) weakly linked to creativity (debated)
- Mood: positive + relaxed → divergent; but constraint sometimes aids creativity
- Expertise paradox: needs domain knowledge but prone to fixation (see [Reasoning_Problem_Solving](Reasoning_Problem_Solving.en.md))

---

## 8. Relation to AI / LLM

- LLM generation = high-temperature sampling + training distribution recombination
- Debate: is it "combinatorial creativity" or "true novelty"? (no intent / evaluation / world grounding)
- Human-machine collaborative creativity ("centaur")
- DMN-ECN switching ↔ generate-then-select architecture

---

## 9. Cultivating Creativity

- Divergent training (brainstorm, SCAMPER) limited transfer
- Cross-domain knowledge + analogy (see [Reasoning_Problem_Solving](Reasoning_Problem_Solving.en.md))
- Incubation (rest / sleep — see [Sleep_Wake](../03_Systems_Neuroscience/Sleep_Wake.en.md))
- Psychologically safe environment (reduce evaluation anxiety)

---

## 10. Common Pitfalls

### 10.1 Creativity = Right Brain

Pseudoscience; it's DMN-ECN whole-brain network interaction.

### 10.2 Novelty = Creativity

Must be novel **and** useful; pure random doesn't count.

### 10.3 Creativity = Divergent Thinking

Needs divergent **+** convergent dynamic collaboration.

### 10.4 Creativity Unrelated to Intelligence

Moderately correlated ("threshold hypothesis" debated: weakens after IQ~120).

### 10.5 LLM = True Creativity

Combinatorial recombination vs intentional novelty, philosophical + empirical debate unresolved.

---

## 11. Related Concepts

- **Same section**: [Executive Function](Executive_Function.en.md), [Mental_Imagery](Mental_Imagery.en.md), [Reasoning_Problem_Solving](Reasoning_Problem_Solving.en.md), [Consciousness](Consciousness.en.md)
- **Systems**: [Sleep_Wake](../03_Systems_Neuroscience/Sleep_Wake.en.md), [Reward_System](../03_Systems_Neuroscience/Reward_System.en.md)
- **AI**: generative models, human-AI collaboration

---

## References

1. **Beaty, R. E. et al.** "Creative cognition and brain network dynamics." *Trends Cogn Sci*, 2016.
2. **Guilford, J. P.** "Creativity." *Am Psychol*, 1950.
3. **Wallas, G.** *The Art of Thought*. 1926.
4. **Dietrich, A. & Kanso, R.** "A review of EEG, ERP, and neuroimaging studies of creativity and insight." *Psychol Bull*, 2010.
