# Neuroethics

> *Neuroethics studies the ethical implications of neuroscience: cognitive enhancement, mind-reading, neural privacy, consciousness determination, BCI, neurolaw. Urgent with Neuralink, fMRI mind-reading, brain organoids. "Neurorights" (cognitive liberty, mental privacy) legislation advancing in Chile, UNESCO. Roskies 2002 split "ethics of neuroscience" + "neuroscience of ethics."*
>
> **Difficulty**: Beginner-Intermediate
> **Prerequisites**: none

---

## 1. Two Branches (Roskies 2002)

- **Ethics of neuroscience**: ethics of neuroscience research + application (enhancement, privacy, experiments)
- **Neuroscience of ethics**: neural basis of moral judgment (moral cognition)

---

## 2. Core Issues

| Issue | Question |
|---|---|
| Cognitive enhancement | Are smart drugs fair? |
| Mental privacy | fMRI mind-reading → privacy? |
| Neuro-prediction | Predict crime from brain? |
| Consciousness | Do vegetative patients have it? Does AI? |
| Free will | Neural determinism → responsibility? |
| BCI | Identity + agency |
| Brain organoids | Do organoids have consciousness? |
| Neuromarketing | Manipulating consumers? |

---

## 3. Cognitive Enhancement

- Modafinil, Ritalin, tDCS non-therapeutic use
- Analogy to sports doping (fairness)
- "Cosmetic neurology"
- Fairness vs autonomy vs coercion (workplace pressure)

---

## 4. Mental Privacy + Mind-Reading

- fMRI decoding reconstructs images / semantics (Tang 2023 semantic decoding)
- Lie detection (fMRI, EEG P300) — courtroom admissibility debate
- "Neurorights": mental privacy legislation
- Chile 2021 first to add neurorights to constitution

---

## 5. Consciousness Determination

- Vegetative state vs minimally conscious state
- Owen 2006: fMRI command-following detects hidden consciousness
- Affects life support withdrawal decisions
- AI consciousness? Brain organoid consciousness?

---

## 6. Free Will + Neurolaw

- Libet 1983: readiness potential precedes conscious decision ~ 300 ms (debated)
- Neural determinism → legal responsibility?
- "My brain made me do it" defense
- Most modern scholars take compatibilist stance

---

## 7. PyTorch — Mental Privacy Risk Demo

```python
import torch
import torch.nn as nn

class BrainDecoder(nn.Module):
    """fMRI → stimulus reconstruction (privacy concern demo)."""
    def __init__(self, n_voxels=10000, latent=512):
        super().__init__()
        self.enc = nn.Linear(n_voxels, latent)
        self.dec = nn.Linear(latent, 784)  # reconstruct seen image
    def forward(self, fmri):
        z = torch.relu(self.enc(fmri))
        return torch.sigmoid(self.dec(z))
# Ethical question: who consents to this being run on their brain data?
```

---

## 8. BCI Ethics

- Agency: is the action "me" or the decoder?
- Identity: does implant change personality? (DBS case reports)
- Data ownership: who owns brain data?
- Access equity: who can afford it?
- See [Neuralink](../07_Neurotech_Frontiers/Neuralink.en.md)

---

## 9. Brain Organoids

- Petri-dish mini-brains (organoids)
- Could they have sentience?
- "Assembloid", organoid intelligence (OI)
- Regulatory vacuum, discussion intensifying 2023+

---

## 10. Governance Frameworks

- **UNESCO** 2023 neurotechnology ethics recommendation
- **Chile** 2021 constitutional neurorights
- **EU AI Act**: emotion recognition restrictions
- **OECD** 2019 neurotechnology principles
- Ienca & Andorno 2017: 4 neurorights (cognitive liberty, mental privacy, mental integrity, psychological continuity)

---

## 11. Common Pitfalls

### 11.1 fMRI Can Read Any Thought

Only decodes trained range; not omniscient mind-reading.

### 11.2 Libet Proves No Free Will

Experiment + interpretation both debated; can't simply conclude this.

### 11.3 Enhancement = Pure Personal Choice

Involves social fairness, coercive pressure, not purely autonomous.

### 11.4 Organoid = Mini Human Brain

Far from complete structure / input; sentience extremely unlikely (currently).

### 11.5 Neuroscience → Moral Conclusions

Is-ought gap: description ≠ prescription.

---

## 12. Related Concepts

- **Same section**: [Research Methods](Research_Methods.en.md), [Levels of Analysis](Levels_of_Analysis.en.md)
- **Cognition**: [Consciousness](../04_Cognitive_Neuroscience/Consciousness.en.md), [Social Cognition](../04_Cognitive_Neuroscience/Social_Cognition.en.md)
- **Frontiers**: [Neuralink](../07_Neurotech_Frontiers/Neuralink.en.md)

---

## References

1. **Roskies, A.** "Neuroethics for the new millennium." *Neuron*, 2002.
2. **Ienca, M. & Andorno, R.** "Towards new human rights in the age of neuroscience and neurotechnology." *Life Sci Soc Policy*, 2017.
3. **Owen, A. M. et al.** "Detecting awareness in the vegetative state." *Science*, 2006.
4. **Yuste, R. et al.** "Four ethical priorities for neurotechnologies and AI." *Nature*, 2017.
