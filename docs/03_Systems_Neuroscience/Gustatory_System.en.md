# Gustatory System

> *Taste detects soluble molecules; five basic tastes: sweet, salty, sour, bitter, umami. Taste buds → CN VII/IX/X → nucleus of solitary tract → thalamus VPM → insular gustatory cortex. Labeled-line vs population coding debate. "Flavor" = taste + smell (retronasal) + trigeminal (spicy/temperature) multisensory. Evolutionary core of approach-food/avoid-poison.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Olfactory_System](Olfactory_System.en.md), [Somatosensory](Somatosensory.en.md)

---

## 1. Five Basic Tastes

| Taste | Ligand | Receptor | Adaptive meaning |
|---|---|---|---|
| Sweet | Sugars | T1R2+T1R3 (GPCR) | Energy |
| Umami | Glutamate | T1R1+T1R3 | Protein/amino acids |
| Bitter | Diverse toxin-like | T2R family (~25) | Avoid poison |
| Salty | Na⁺ | ENaC | Electrolytes |
| Sour | H⁺ | OTOP1 | Spoilage/unripe |

(Fat, calcium, water as "sixth taste" candidates, under research)

---

## 2. Transduction

- **Sweet/umami/bitter**: GPCR → PLCβ2 → IP3 → Ca²⁺ → TRPM5 → ATP release (purinergic transmission to nerve)
- **Salty**: Na⁺ via ENaC directly depolarizes
- **Sour**: H⁺ via OTOP1 proton channel
- Each taste bud contains multiple receptor cell types (I/II/III)

---

## 3. Pathway

```
Taste buds (tongue/palate/pharynx)
   ↓ CN VII (facial) / IX (glossopharyngeal) / X (vagus)
Nucleus of solitary tract (NTS, medulla)
   ↓
Thalamus VPM (ventral posteromedial)
   ↓
Insula / frontal operculum gustatory cortex (primary)
   ↓
OFC (secondary, taste-value) + amygdala
```

(Note: taste **goes through** thalamus; smell does not — see [Olfactory_System](Olfactory_System.en.md))

---

## 4. Coding Debate

- **Labeled line**: dedicated channel per taste ("sweet line") — receptor + NTS partly supports
- **Across-fiber/population**: population pattern encodes taste quality
- Modern: periphery more labeled-line (receptor-specific), central more distributed + state-dependent

---

## 5. PyTorch — Five-Taste Population Code

```python
import torch

def taste_population_code(stimulus, receptor_affinity):
    """5 receptor types -> population response; OFC reads value."""
    # receptor_affinity: (5, n_tastants); stimulus: (n_tastants,)
    response = torch.sigmoid(receptor_affinity @ stimulus)   # (5,)
    # Hedonic value: sweet/umami positive, bitter negative (innate)
    valence = torch.tensor([1.0, 1.0, -0.3, 0.2, -1.0])
    pleasantness = (response * valence).sum()
    return response, pleasantness
```

---

## 6. Flavor = Multisensory

- "Flavor" is mainly **olfaction** (retronasal, mouth-to-nose) + taste + trigeminal (spicy TRPV1, mint TRPM8, carbonation)
- Pinch nose while eating → flavor greatly reduced (actually olfaction)
- Multisensory integration in OFC
- Tightly linked to [Olfactory_System](Olfactory_System.en.md)

---

## 7. Innate + Plastic

- Sweet/umami → innate approach; bitter → innate avoidance (infant facial expressions universal)
- Taste aversion learning (Garcia effect): single illness pairing → long-term avoidance (amygdala + strong innate preparedness)
- Experience + culture greatly shape preferences

---

## 8. Pathology / Modulation

- **Taste disorders**: ageusia (none) / dysgeusia (distorted) — COVID, chemo, zinc deficiency, nerve damage
- **State-dependent**: hunger increases sweet pleasantness, satiety decreases (alliesthesia)
- **Bitter genetics**: PTC/PROP taste blindness (TAS2R38 polymorphism)
- Linked to obesity / sugar preference / appetite regulation

---

## 9. Relation to AI / Engineering

- Electronic tongue (electrochemical sensor array + ML) ≈ taste receptor array + classification
- Population coding ↔ distributed representation
- Flavor = multimodal fusion (taste+smell+touch) ↔ multimodal models
- Approach-avoid value ↔ intrinsic reward (see [Reward_System](Reward_System.en.md))

---

## 10. Common Pitfalls

### 10.1 Tongue Has a "Taste Map"

Classic zone map is a myth; all tastes sensed across tongue (density slightly varies).

### 10.2 Taste = Flavor

Flavor mainly via olfaction (retronasal); verify by pinching nose.

### 10.3 Only 4 Tastes

Umami is the fifth; fat etc. candidates under research.

### 10.4 Taste Doesn't Go Through Thalamus

Taste **goes through** VPM thalamus; smell does not.

### 10.5 Bitter = Bad

Bitter is innate poison-avoidance bias; not objectively "bad" (coffee/bitter melon culturally acquired).

---

## 11. Related Concepts

- **Same section**: [Olfactory_System](Olfactory_System.en.md), [Somatosensory](Somatosensory.en.md), [Reward_System](Reward_System.en.md)
- **Anatomy**: [Brainstem](../01_Neuroanatomy/Brainstem.en.md) (NTS), [Thalamus](../01_Neuroanatomy/Thalamus.en.md)
- **Cognition**: [Decision_Making](../04_Cognitive_Neuroscience/Decision_Making.en.md) (value)

---

## References

1. **Chandrashekar, J. et al.** "The receptors and cells for mammalian taste." *Nature*, 2006.
2. **Yarmolinsky, D. A. et al.** "Common sense about taste: from mammals to insects." *Cell*, 2009.
3. **Small, D. M.** "Flavor is in the brain." *Physiol Behav*, 2012.
4. **Garcia, J. et al.** "Conditioned aversion to saccharin resulting from exposure to gamma radiation." *Science*, 1955.
