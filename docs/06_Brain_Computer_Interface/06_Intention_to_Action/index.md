# 意图到行动（I2A）

**本章是整个脑机接口主题的核心章节**。它把前面所有解码技术（第 04、05 章）和后面所有应用（第 07、08、09 章）串联成一条完整管线：

> 神经信号 → 意图提取 → 行动计划（LLM / POMDP）→ 执行 → 感觉反馈 → 神经信号 ...

传统 BCI 停留在 *kinematic decoding*（解码关节角度、速度、力）。现代 BCI 把意图解码与 LLM / RL 规划器结合，让用户只需"想喝水"而不是"想让手肘弯 30°、手腕旋 45°"。这正是 "intention-to-action" 范式与上一代 BCI 的根本区别。

**为什么这章重要。** I2A 范式让 BCI 从"控制接口"升级为"代理接口"——用户不再操控末端执行器，而是表达高层目标，由 LLM / POMDP 翻译成低层动作序列。这同时也是第 10 章把 BCI 与具身智能 / 类人智能连接起来的关键桥梁：当意图本身就是规划器的输入时，BCI 就成了世界模型与生物大脑共享潜空间的接口。

**学习路径。** 建议第一遍只读「运动意图解码」与「标志性系统案例」——前者把 kinematic 与 goal-level 两种解码层次说清楚（**Shanechi 2024 DPAD** 是当下最重要的工作之一），后者用 Pitt arm / BrainGate coffee / Walk Again 三个案例把抽象管线落到真实场景。第二遍再依次读「共享自主」（POMDP 与 hindsight optimization）、「分层规划 BCI_LLM_机器人」（**BrainBody-LLM / HiCRISP / ROS2** 管线）、「闭环控制与 CLDA」（co-adaptation 在线校准）。

**本章内容：**

- **[运动意图解码](运动意图解码.md)** — kinematic vs goal-level intent；DPAD（Shanechi 2024）
- **[共享自主](共享自主.md)** — Javdani-Srinivasa POMDP；hindsight optimization；BCI 场景下的人机协作
- **[分层规划 BCI_LLM_机器人](分层规划_BCI_LLM_机器人.md)** — BrainBody-LLM、HiCRISP、ROS2 管线
- **[闭环控制与 CLDA](闭环控制与CLDA.md)** — CLDA、SmoothBatch、co-adaptation
- **[标志性系统案例](标志性系统案例.md)** — Pitt arm (Collinger 2013)、BrainGate coffee (Hochberg 2012)、Walk Again (Nicolelis 2016)
