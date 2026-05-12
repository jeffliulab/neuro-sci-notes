# Hierarchical Planning: BCI + LLM + Robot

**BCI + LLM + robot** (BLR for short) is one of the most active AI frontiers of 2024–2026. It takes the **low-bandwidth intent** extracted by a BCI, has an LLM expand it into a **structured plan**, and then lets a robot **execute** it — forming a complete Intention-to-Action pipeline.

## 1. Architecture Overview

```
┌─────────────────────────────────────────────────────┐
│  Brain                                              │
│  ├ PPC/PFC: high-level intent ("go to kitchen, get water") │
│  └ M1: low-level kinematics                         │
└─────────────────┬───────────────────────────────────┘
                  ↓  neural signals
┌─────────────────────────────────────────────────────┐
│  BCI decoder                                        │
│  ├ NDT3/CEBRA: neural → embedding                   │
│  ├ Speech BCI: neural → words                       │
│  └ Intent classifier: → structured intent           │
└─────────────────┬───────────────────────────────────┘
                  ↓  natural language / structured intent
┌─────────────────────────────────────────────────────┐
│  LLM planner                                        │
│  ├ parse the intent                                 │
│  ├ decompose into sub-goals                         │
│  ├ generate an action sequence                      │
│  └ error recovery / dialogue clarification          │
└─────────────────┬───────────────────────────────────┘
                  ↓  action sequence (ROS2 / PDDL)
┌─────────────────────────────────────────────────────┐
│  robot execution                                    │
│  ├ motion planning (MoveIt, RRT*)                   │
│  ├ visual perception (SAM, CLIP)                    │
│  └ control (PID, MPC)                               │
└─────────────────────────────────────────────────────┘
```

Each layer handles a different granularity of abstraction — this is the essence of **hierarchical planning**.

## 2. Why We Need an LLM Layer

### Without the LLM

Every single action must be specified in detail via BCI:

- "forward 10 cm" "grasp" "lift" "left 30 cm" ...
- BCI bandwidth is insufficient and the experience is terrible

### With the LLM

The user says "give me a glass of water", and the LLM produces a 20+ step action sequence:

1. identify "water" = the water bottle in the kitchen
2. plan motion to the kitchen
3. grasp the bottle
4. return to the user
5. pour water into the user's cup
6. deliver it to the user's mouth

The BCI only has to carry **semantic-level intent**.

### Core capabilities provided by the LLM

- **common-sense reasoning**: "water" lives in the kitchen, the coffee machine produces coffee
- **language understanding**: vague expressions ("I'm thirsty")
- **error recovery**: when the robot reports "there is no water in the kitchen", the LLM suggests an alternative
- **multi-turn dialogue**: the LLM adapts when the user corrects it

## 3. Representative Systems

### HiCRISP (2023)

**Chen et al.** introduced **HiCRISP (Hierarchical Closed-loop Robotic Intelligent Self-correction Planner)**:

- the LLM generates task-level plans
- closed-loop monitoring + self-correction
- demonstrated in BCI + robot scenarios

### PaLM-E (Google 2023)

A **multimodal LLM** unifying vision + language + action.

- input: image + user instruction
- output: robot action sequence
- combined with a BCI language interface it becomes a brain-controlled PaLM-E

### RT-2 (Google 2023)

**Vision-Language-Action (VLA)** model:

- treats robot actions as language tokens
- emits motion commands directly from the LLM
- a BCI can feed in as a "text prompt generator"

### Voyager (Wang 2023)

**LLM as a long-horizon planning agent**:

- skill discovery, skill library, self-reflection
- originally designed for Minecraft, but it provides a template for BCI assistance

## 4. BCI-LLM Interface Design

### Interface 1: natural language

BCI → speech/handwriting → LLM

**Pros**: LLMs accept it natively.
**Cons**: low bandwidth, ~60 words per minute.

Applicable: Willett 2023 speech BCI + GPT-4.

### Interface 2: structured intent

BCI → JSON / slot filling → LLM

```json
{"action": "fetch", "object": "water", "target": "me"}
```

**Pros**: short and high-certainty.
**Cons**: the intent vocabulary is limited.

### Interface 3: neural embedding

BCI → latent-space vector → LLM (as a soft prompt)

**Pros**: retains full neural information.
**Cons**: requires trained alignment.
**Frontier**: **NeuroLM** (2024) attempts to train neural-language alignment directly.

## 5. Challenges of Putting LLMs in the Loop

### Latency

LLM inference takes 500 ms – 2 s, too slow for real-time interaction.
Solution: **edge LLMs** (Llama-3 / Phi) + cloud GPT hybrid.

### Hallucinations

The LLM may invent actions or misplace objects.
Solutions:

- **Grounding**: the LLM may only call skills the robot already possesses
- **visual verification**: use CLIP to confirm the object exists before execution
- **user confirmation**: require BCI confirmation for critical steps

### Safety

The LLM can be coaxed (or attacked) into issuing dangerous actions.
Solution: **Constitutional AI**-style rule constraints.

## 6. Training Strategies

### SFT: supervised fine-tuning

- collect (BCI intent, LLM plan, robot result) triples
- fine-tune the LLM so it understands BCI scenarios better

### RLHF: reinforcement learning from human feedback

- users rate how good each plan is
- PPO optimises the LLM for user preference

### In-context prompting

- give the LLM the current environment + skill-library description
- zero-shot / few-shot planning
- well suited for rapid iteration

## 7. Open-Source Tooling

| Tool | Layer | Function |
| --- | --- | --- |
| **MNE / Kilosort** | BCI decoding | preprocessing |
| **NDT3 / CEBRA** | BCI decoding | latent space |
| **LangChain** | LLM | planning, tool calling |
| **Voyager / CoT-Robotics** | LLM | skill learning |
| **ROS2** | robot | communication |
| **MoveIt** | robot | motion planning |
| **SAM / CLIP** | vision | object recognition |

## 8. Regulation and Deployment

Regulating a BLR system is complex:

- **BCI layer**: FDA / NMPA medical device
- **LLM layer**: EU AI Act high-risk AI
- **robot layer**: ISO 10218 (industrial), ISO 13482 (service)

Compliance path: **separate certification at each layer + whole-system certification.** A fully commercial BLR system is not expected before 2027–2030.

## 9. Correspondence with Human-Like Intelligence

The BLR pipeline mirrors the **Human_Like_Intelligence / world_model / JEPA** line of thinking:

| Human-like intelligence | BLR |
| --- | --- |
| predictive coding (sensation → internal state) | BCI decoding |
| world model (internal state → action) | LLM planning |
| motor control (action → output) | robot execution |
| environmental feedback | vision / haptics loop |

BLR **is not a simulation of AGI**, but it is an engineering model of **"read real biological intelligence + inject artificial intelligence"** — this complementary structure is the root of where BCI research and human-like-intelligence research converge.

## 10. Landmark Milestones

- **2022**: Microsoft + Synchron demo of Apple Vision OS BCI control
- **2023**: UCSF Metzger avatar: BCI → facial motion + speech
- **2024 CES**: Synchron + Apple Vision Pro demo
- **2024-Q4**: Neuralink patient holds everyday conversations using BCI + voice assistant
- **2026 expected**: full BCI + LLM + robotic-arm assisted-living demo

## 11. Chain of Reasoning

1. **Insufficient BCI bandwidth** means we need a **higher-level "expander"** — the LLM is the best candidate.
2. **Hierarchical planning**: BCI extracts intent → LLM expands the plan → robot executes.
3. **Three interface designs** (natural language, structured, embedding), each with trade-offs.
4. **Latency, hallucination, and safety** are the core engineering challenges for BLR.
5. **BLR is the convergence point of BCI work and human-like-intelligence work** — the mainstream research direction after 2024.

## References

- Chen et al. (2023). *HiCRISP: An LLM-driven hierarchical closed-loop robotic intelligent self-correction planner.* arXiv:2309.12089.
- Driess et al. (2023). *PaLM-E: an embodied multimodal language model.* arXiv. https://palm-e.github.io/
- Brohan et al. (2023). *RT-2: vision-language-action models transfer web knowledge to robotic control.* CoRL.
- Wang et al. (2023). *Voyager: an open-ended embodied agent with large language models.* arXiv. https://voyager.minedojo.org/
- Metzger et al. (2023). *A high-performance neuroprosthesis for speech decoding and avatar control.* Nature. https://www.nature.com/articles/s41586-023-06443-4
