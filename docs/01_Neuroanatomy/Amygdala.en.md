# Amygdala

> *Amygdala is an almond-shaped structure in the temporal lobe, ~13 nuclei groups. Core functions: fear conditioning, emotion processing, social/threat detection. LeDoux's 1990s "fear circuit" is the classic model. Damage → Klüver-Bucy syndrome (no fear). Key structure in PTSD and anxiety disorders.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Cortex](Cortex.en.md), [Emotion](../04_Cognitive_Neuroscience/Emotion.en.md)

---

## 1. Anatomical Location

- In medial temporal lobe, anterior to hippocampus
- Bilateral
- Three main nuclei groups:
  - **Basolateral (BLA)**: input (thalamus, cortex)
  - **Central (CeA)**: output (brainstem, hypothalamus)
  - **Cortical-like (CoA)**: olfactory
- Volume: ~ 1.7 cm³

---

## 2. Pathways

```
Sensory thalamus ──(low road, fast)── Amygdala BLA
       │
       └──→ Cortex ──(high road, accurate)── Amygdala BLA
                                                │
                                          Central nucleus
                                          /         \
                                Hypothalamus    Brainstem
                                  (autonomic)   (freeze, startle)
```

- **Low road** (LeDoux): thalamus → BLA, 12 ms, crude but fast
- **High road**: thalamus → cortex → BLA, slow but precise
- Evolutionary design: trade-off speed vs accuracy

---

## 3. Fear Conditioning (Classic Experiment)

- Pavlovian: CS (tone) + US (shock) → CR (freeze)
- LeDoux: damaging BLA → no conditioning
- LTP at BLA-thalamus synapses
- Key receptors: NMDA, AMPA, CaMKII

---

## 4. Output + Behavior

| Pathway | Behavior |
|---|---|
| → Lateral hypothalamus | BP ↑ |
| → PVN hypothalamus | HPA axis (cortisol) |
| → PAG (midbrain) | freezing |
| → Pons (locus coeruleus) | NE arousal |
| → Nucleus reticularis | startle reflex |

---

## 5. Beyond Fear

- **Reward**: BLA also responds to positive stimuli
- **Salience**: prominence (any emotional intensity)
- **Social**: facial expression (esp. fear face)
- **Decision**: interacts with OFC for value computation

---

## 6. Klüver-Bucy Syndrome (1939)

- Bilateral amygdalectomy monkey
- Loses fear (walks toward snake)
- Hyperorality (everything to mouth)
- Hypersexuality
- Visual agnosia
- Humans: HSV encephalitis → rare Klüver-Bucy

---

## 7. PyTorch — Fear Conditioning Sim

```python
import torch

class AmygdalaFearLearning(torch.nn.Module):
    """Pavlovian fear conditioning."""
    def __init__(self):
        super().__init__()
        self.W_cs = torch.nn.Parameter(torch.tensor(0.0))  # CS → fear
        self.lr = 0.5
    
    def forward(self, cs, us):
        """cs: tone, us: shock (binary)."""
        predicted_fear = torch.sigmoid(self.W_cs * cs)
        return predicted_fear
    
    def learn(self, cs, us):
        """Rescorla-Wagner: ΔW = lr * (US - W*CS) * CS."""
        with torch.no_grad():
            pe = us - self.W_cs * cs
            self.W_cs += self.lr * pe * cs
```

---

## 8. Pathology + Psych Disease

- **PTSD**: amygdala hyperactivity + mPFC failure to inhibit
- **Anxiety disorder**: amygdala over-response
- **Autism**: abnormal amygdala volume, fear recognition deficits
- **Psychopathy**: amygdala shrunk / low response
- **Urbach-Wiethe**: rare genetic disease → bilateral amygdala calcification → no fear (Patient SM classic case)

---

## 9. Treatment

- **Exposure therapy**: extinction (new inhibitory learning)
- **SSRI**: improves anxiety
- **Propranolol**: blocks NE → weakens memory reconsolidation
- **MDMA-assisted therapy**: experimental PTSD treatment
- **DBS (experimental)**: BLA stimulation

---

## 10. Common Pitfalls

### 10.1 "Fear Center" Oversimplified

Amygdala processes salience, not only fear.

### 10.2 Low Road Absolute

LeDoux's model is simplified; cortical input also fast.

### 10.3 Klüver-Bucy Total Fearlessness

Patient SM still has startle reactions (via other pathways).

### 10.4 Amygdala = Anxiety

Anxiety involves hippocampus, PFC, not only amygdala.

### 10.5 Reconsolidation Simple

Propranolol's reconsolidation blocking inconsistent in humans.

---

## 11. Related Concepts

- **Same section**: [Cortex](Cortex.en.md), [Hippocampus Anatomy](Hippocampus_Anatomy.en.md), [Brainstem](Brainstem.en.md)
- **Cognition**: [Emotion](../04_Cognitive_Neuroscience/Emotion.en.md), [Decision Making](../04_Cognitive_Neuroscience/Decision_Making.en.md)
- **Disease**: PTSD, Anxiety

---

## References

1. **LeDoux, J. E.** *The Emotional Brain*. Simon & Schuster, 1996.
2. **Phelps, E. A. & LeDoux, J. E.** "Contributions of the amygdala to emotion processing." *Neuron*, 2005.
3. **Adolphs, R. et al.** "Impaired recognition of emotion in facial expressions following bilateral damage to the human amygdala." *Nature*, 1994.
4. **Davis, M. & Whalen, P. J.** "The amygdala: vigilance and emotion." *Mol Psychiatry*, 2001.
