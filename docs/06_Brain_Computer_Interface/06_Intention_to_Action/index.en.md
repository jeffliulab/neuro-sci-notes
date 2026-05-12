# Intention-to-Action (I2A)

**This is the core chapter of the entire BCI topic.** It weaves together all the decoding techniques from earlier chapters (04, 05) and all the downstream applications (07, 08, 09) into a single complete pipeline:

> neural signals → intent extraction → action planning (LLM / POMDP) → execution → sensory feedback → neural signals ...

Traditional BCIs stopped at *kinematic decoding* (decoding joint angles, velocities, forces). Modern BCIs couple intent decoding with LLM / RL planners, so the user only has to think "I want a drink of water" rather than "bend the elbow 30°, rotate the wrist 45°." This is exactly what sets the "intention-to-action" paradigm apart from the previous generation of BCIs.

**Why this chapter matters.** The I2A paradigm upgrades BCI from a "control interface" to an "agent interface" — the user no longer operates the end-effector but expresses a high-level goal, which an LLM / POMDP translates into a low-level action sequence. This is also the bridge that lets Chapter 10 connect BCI to embodied and human-like intelligence: when intent itself is the input to the planner, BCI becomes the interface where world models share a latent space with the biological brain.

**Recommended reading order.** On the first pass, read only *Motor Intent Decoding* and *Landmark System Case Studies* — the former makes the kinematic vs. goal-level distinction precise (**Shanechi 2024 DPAD** is one of the most important recent works), and the latter grounds the abstract pipeline in three real systems: Pitt arm, BrainGate coffee, Walk Again. On a second pass, work through *Shared Autonomy* (POMDP and hindsight optimization), *Hierarchical Planning: BCI + LLM + Robot* (**BrainBody-LLM / HiCRISP / ROS2** pipeline), and *Closed-loop Control and CLDA* (co-adaptive online calibration).

**Contents of this chapter:**

- **[Motor Intent Decoding](运动意图解码.md)** — kinematic vs goal-level intent; DPAD (Shanechi 2024)
- **[Shared Autonomy](共享自主.md)** — the Javdani-Srinivasa POMDP; hindsight optimization; human-machine collaboration in BCI settings
- **[Hierarchical Planning: BCI + LLM + Robot](分层规划_BCI_LLM_机器人.md)** — BrainBody-LLM, HiCRISP, the ROS2 pipeline
- **[Closed-loop Control and CLDA](闭环控制与CLDA.md)** — CLDA, SmoothBatch, co-adaptation
- **[Landmark System Case Studies](标志性系统案例.md)** — Pitt arm (Collinger 2013), BrainGate coffee (Hochberg 2012), Walk Again (Nicolelis 2016)
