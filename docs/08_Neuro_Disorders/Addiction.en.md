# Addiction (Substance Use Disorder)

> *Addiction is a chronic relapsing brain disease: the mesolimbic dopamine system is hijacked → compulsive use + loss of control + craving despite harm. Three-stage cycle (Koob): binge/intoxication → withdrawal/negative affect → preoccupation/anticipation. Involves VTA-NAcc-PFC. A classic RL dysregulation — wanting ≠ liking.*
>
> **Difficulty**: Intermediate
> **Prerequisites**: [Reward System](../03_Systems_Neuroscience/Reward_System.en.md), [Reinforcement Learning Brain](../05_Computational_Neuroscience/Reinforcement_Learning_Brain.en.md)

---

## 1. Definition

- DSM-5 "Substance Use Disorder": uncontrolled use + craving + tolerance + withdrawal + use despite consequences
- Chronic + relapsing
- Behavioral addiction (gambling) shares circuit — DSM-5 includes gambling disorder

---

## 2. Three-Stage Cycle (Koob & Volkow)

```
Binge/Intoxication  → NAcc/VTA (DA reward, reinforcement)
       ↓
Withdrawal/Neg affect → amygdala/CRF (negative emotion, anti-reward)
       ↓
Preoccupation/Anticipation → PFC (craving, loss of control, cue-driven)
       ↓ (back to binge)
```

---

## 3. Neural Mechanism

- **Acute**: ↑ mesolimbic DA (VTA→NAcc) — common to all addictive substances
- **Tolerance**: DA receptor / signaling downregulation → anhedonia
- **Anti-reward**: CRF/dynorphin → withdrawal negative emotion
- **PFC dyscontrol**: ↓ prefrontal regulation → impulsivity + compulsion
- **Cue-induced craving**: glutamatergic, incentive sensitization

---

## 4. Wanting ≠ Liking (Berridge)

- **Wanting** (incentive salience): DA-mediated, pathologically amplified
- **Liking** (hedonic): opioid/endocannabinoid, **reduced** in addiction
- → Addicts strongly crave but no longer enjoy ("wanting without liking")
- Classic RL "value" insufficient — incentive sensitization theory

---

## 5. RL View

- Drug = unnatural large RPE (directly pharmacologically ↑ DA, not suppressed by prediction)
- → Excessive value learning, habit (dorsal striatum) takes over
- Model-free habit > goal-directed control
- See [Reinforcement Learning Brain](../05_Computational_Neuroscience/Reinforcement_Learning_Brain.en.md)

---

## 6. PyTorch — Drug-Induced Unnatural RPE

```python
import torch

def addiction_rpe(natural_reward=1.0, drug_pharmacological=3.0, predicted=1.0):
    """Drugs add pharmacological DA that prediction can't cancel."""
    natural_rpe = natural_reward - predicted          # learnable -> 0
    # Drug directly raises DA regardless of prediction (Redish 2004)
    drug_rpe = (drug_pharmacological - predicted) + drug_pharmacological * 0.5
    return natural_rpe, drug_rpe   # drug_rpe stays positive -> over-learning
```

---

## 7. Mechanisms by Substance

| Substance | Main mechanism |
|---|---|
| Opioids | μ-opioid → inhibit GABA → ↑ VTA DA |
| Cocaine | Block DAT → ↑ synaptic DA |
| Amphetamine | Reverse DAT + promote DA release |
| Nicotine | nAChR → ↑ VTA DA |
| Alcohol | GABA-A↑, NMDA↓, indirect DA |
| Cannabis | CB1 → disinhibit DA |

→ Common endpoint: ↑ mesolimbic DA.

---

## 8. Treatment

### 8.1 Medication

- **Opioid**: methadone/buprenorphine (substitution), naltrexone (antagonist), naloxone (rescue)
- **Alcohol**: naltrexone, acamprosate, disulfiram
- **Nicotine**: NRT, varenicline, bupropion

### 8.2 Psychological / Social

- CBT, motivational interviewing, contingency management (among strongest evidence)
- 12-step, community support
- Relapse prevention (cue management)

### 8.3 Experimental

- DBS (NAcc), TMS, psychedelic-assisted (psilocybin for alcohol)

---

## 9. Relapse + Chronic Disease Model

- High relapse rate (like diabetes/hypertension chronic management)
- Cue, stress, drug itself trigger (reinstatement model)
- Long-term PFC + striatal adaptation → lasting vulnerability
- "Brain disease model" reduces stigma vs over-determinism debate

---

## 10. Common Pitfalls

### 10.1 = Weak Will / Moral Issue

A chronic brain disease (circuit hijack), not mere choice / character.

### 10.2 Addicts Enjoy the Drug

Wanting ≫ liking; later often don't enjoy yet still crave.

### 10.3 Withdrawal = Cure

Withdrawal is just the start; relapse risk persists long-term.

### 10.4 Only DA / Reward

Includes anti-reward (CRF), PFC dyscontrol, habit system.

### 10.5 Substitution Therapy = "Swapping Drugs"

Methadone/buprenorphine stabilize circuit, reduce harm, strong evidence.

---

## 11. Related Concepts

- **Same section**: [Depression](Depression.en.md), [Anxiety_Disorders](Anxiety_Disorders.en.md), [ADHD](ADHD.en.md)
- **Systems**: [Reward System](../03_Systems_Neuroscience/Reward_System.en.md)
- **Anatomy**: [Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.en.md), [Amygdala](../01_Neuroanatomy/Amygdala.en.md)
- **Computational**: [Reinforcement Learning Brain](../05_Computational_Neuroscience/Reinforcement_Learning_Brain.en.md)

---

## References

1. **Koob, G. F. & Volkow, N. D.** "Neurobiology of addiction: a neurocircuitry analysis." *Lancet Psychiatry*, 2016.
2. **Robinson, T. E. & Berridge, K. C.** "The incentive sensitization theory of addiction." *Brain Res Rev*, 1993.
3. **Redish, A. D.** "Addiction as a computational process gone awry." *Science*, 2004.
4. **Volkow, N. D. et al.** "Neurobiologic advances from the brain disease model of addiction." *NEJM*, 2016.
