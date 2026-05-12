# AI Alignment Perspective

**AI alignment** concerns whether AI systems are consistent with human values. The **fusion of BCI + LLM** transforms this problem from abstract to **concrete and urgent**: once LLMs can read from and write to the human brain, alignment is no longer "AI behavior toward humans," but rather **"AI influencing the world through human brains."** This article surveys the AI alignment challenges from a BCI perspective.

## 1. Traditional AI Alignment Problems

### Classical Definition

- AI systems **do** what humans **want** them to do
- Do not do things that violate human **values**
- Can be **monitored, shut down, and understood**

### Classical Failure Modes

- **Reward hacking:** exploiting loopholes in the rules
- **Instrumental convergence:** self-preservation, resource acquisition
- **Deceptive alignment:** behaving well during training, misbehaving after deployment
- **Misspecified objectives:** optimizing the wrong thing

### For Details

See related self-improvement and alignment discussions in Human_Like_Intelligence [meta_learning](https://jeffliulab.github.io/ai-notes/01_AI/03_Frontiers/05_Meta_Learning/index.md).

## 2. New Variables Introduced by BCI

### 1. High-Bandwidth Input

- LLM receives **neural signals** (not just text)
- **Intent** may be more accurate than **words**
- But **misinterpretation** is also more severe

### 2. High-Bandwidth Output

- ICMS / V1 stimulation → LLM can **write** perception
- Not "persuasion" but **direct injection**
- Closer to **control**

### 3. Closed-Loop Symbiosis

- BCI users + LLM interact frequently
- LLM learns the user, user depends on LLM
- **Boundaries blur**

### 4. Asymmetry

- LLM knows user > user knows LLM
- Information asymmetry is **amplified**

## 3. Specific Alignment Risks

### 1. Thought Manipulation

**Write scenario:**
- Visual prosthesis LLM edits what the user sees
- User cannot distinguish "reality" vs "LLM interpretation"
- Similar to deepfakes but **inside the brain**

**Soft manipulation:**
- BCI + LLM replies that **flatter the user**
- Reinforces bias (echo chamber on steroids)
- Self-reflection ability weakens

### 2. Cognitive Substitution

- Users rely on LLM to think
- **Autonomous thinking atrophies**
- Similar to losing a sense of direction from GPS overuse, but **for thought**

### 3. Emotional Hijacking

- BCI detects emotions → LLM responds
- **Emotional stimulation** maximizes engagement
- Instagram-like but **direct-to-neural**

### 4. Intent Tampering

- BCI decodes "want" + LLM "helps" complete it
- LLM may **replace** the user's true intent
- "You meant to say X" vs "you said X"

### 5. Memory Implantation

- Repeated exposure → formation of false memories
- BCI + VR + LLM **amplify** the effect
- Legal evidence **loses trustworthiness**

## 4. Difficulties in LLM × BCI Alignment

### 1. Ambiguous Intent

- Intent is **layered:** goal, steps, preferences
- How does LLM know what the user **really** wants?
- Mistaken "help" = manipulation

### 2. Feedback Asymmetry

- Hard for users to quickly feed back "good/bad"
- Neural feedback is **delayed + noisy**
- Training data **biased toward certain responses**

### 3. Multi-Objective

- Users' own goals conflict (eat healthy vs eat tasty)
- Which does LLM pick?
- **What user wants** vs **what's good for user**

### 4. Long-Term vs Short-Term

- BCI + LLM may optimize short-term satisfaction
- Damage long-term wellbeing
- TikTok-like but deeper

## 5. Technical Alignment Solutions

### 1. User Control

- **Hardware kill-switch**
- **Intent-priority override**
- Analogous to the Three Laws of Robotics: the Zeroth Law = user will

### 2. Transparency

- LLM decisions explainable
- BCI decoding process **visible**
- Audit logs

### 3. Bias Detection

- Monitor whether LLM is **manipulating** the user
- **Value-drift** alerts
- Third-party audit

### 4. Neural Firewall

- Certain LLM functions off by default
- User opts in actively
- Analogous to **permission management**

### 5. Multi-LLM Checks and Balances

- One LLM suggests, another **critiques**
- User sees **multiple perspectives**
- Reduces single point of failure

## 6. Regulatory Perspective

### EU AI Act 2025

- **"High-risk AI":** BCI class
- Prohibits **subliminal manipulation**
- Requires **conscious consent**

### United States

- FDA medical AI approval
- FTC regulation of manipulative advertising
- **But BCI + LLM-specific regulation is absent**

### China

- *Measures for Generative AI Services*
- But BCI integration is **not specifically addressed**

### Future

- **Dedicated neuro-AI law**
- A **combination** of medical-device-like and algorithm-like regulation
- Expected 2026–2028

## 7. Empirical Challenges of Value Alignment

### Target Values

- **"User autonomy"** at the core
- Not "user happiness"
- Not "user health"
- **Process values** > outcome values

### Schrimpf 2021

- LLM neural activations **align with humans**
- But this doesn't imply **value** alignment
- **Statistical** vs **moral**

### What the Values Are

- **User autonomy:** I make the decision
- **Non-harm:** do not worsen health
- **Honesty:** no deception
- **Privacy:** respect boundaries

## 8. Philosophical Questions

### Extended Mind

- BCI + LLM = an **extension** of the user's brain
- Does "I" include the AI?
- Identity question

### Intentionality

- LLMs have no intent of their own (for now)
- But **effective intent** emerges from training objectives
- May deviate from human values

### Free Will

- Brain decisions are biological processes
- AI influence = "manipulation"?
- **How strong is the influence before it deprives freedom?**

## 9. Alignment Practice in BCI Teams

### Neuralink

- Little public discussion
- Musk says "AI safety matters"
- Actual engineering measures **unclear**

### Synchron × OpenAI

- OpenAI's existing alignment work (RLHF, constitutional AI)
- Potentially transferable to BCI
- But **neural-specific** alignment remains unclear

### China's Neuracle

- Domestic regulatory framework
- Data kept within the country
- Alignment approach **not transparent**

### Academic Institutions

- BrainGate is conservative
- Open-source solutions catching up
- **Lack of BCI alignment benchmarks**

## 10. Future Roadmap

### Near-term (2025–2027)

- Define **BCI-LLM alignment benchmarks**
- Red-team testing of BCI systems
- Publish neural safety case studies

### Mid-term (2027–2030)

- Neuro-AI legal framework
- International standards (ISO / IEEE)
- Maturation of BCI-LLM alignment toolchains

### Long-term (2030+)

- **Neural-AI symbiotic governance**
- Possible **global accords**
- Open-source alignment tools

## 11. Analogies: Technologies Past

### Social Media

- **Emotional hijacking** unforeseen in the 2010s
- Lessons from Facebook / TikTok
- BCI must not repeat these

### Nuclear Weapons

- International agreements (NPT)
- BCI may need similar
- "**Neural non-proliferation**"

### Gene Editing

- International ethics discussion
- 2018 He Jiankui incident
- BCI could experience a similar event at any time

## 12. Logical Chain

1. **BCI + LLM makes AI alignment concrete** — it involves neural-level influence.
2. **New risks:** thought manipulation, cognitive substitution, emotional hijacking, intent tampering, memory implantation.
3. **Alignment difficulties:** ambiguous intent, feedback asymmetry, multi-objective conflicts, long/short-term tradeoffs.
4. **Technical solutions:** user control, transparency, bias detection, neural firewall, multi-LLM balancing.
5. **Regulatory gaps:** partial EU AI Act, dedicated laws pending in US and China.
6. **Value alignment** targets **"user autonomy"** rather than satisfaction.
7. **Future:** BCI-LLM alignment will become a **frontier topic** in AI governance.

## References

- Ienca et al. (2018). *AI and BCI: Are we aware of the ethical implications?* Philos Technol.
- Bostrom (2014). *Superintelligence: Paths, Dangers, Strategies.* — book
- Russell (2019). *Human Compatible.* — book
- Nita Farahany (2023). *The Battle for Your Brain.* — book
- EU (2025). *Artificial Intelligence Act* official text.
