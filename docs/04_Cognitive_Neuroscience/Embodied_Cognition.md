# 具身认知 (Embodied Cognition)

> *Embodied cognition 主张认知不是抽象符号操作,而**根植于身体、感觉运动、环境互动**。挑战经典"computer metaphor"。证据:动作-语言耦合、镜像系统、grounded language、概念隐喻。对 AI 影响大:符号 grounding 问题、具身智能(robot learning)。是 LLM "缺身体"批评的理论基础。*
>
> **难度**:Intermediate
> **前置知识**:[Social_Cognition](Social_Cognition.md)、[Language](Language.md)

---

## 1. 核心主张

- 认知 ≠ 离身符号计算(反 classic cognitivism)
- 概念**根植**于感觉运动经验(grounding)
- 身体形态 + 环境构成认知的一部分("4E":embodied, embedded, enacted, extended)

---

## 2. 证据线

| 证据 | 描述 |
|---|---|
| Action-language coupling | 读"踢"激活运动皮层腿区(Pulvermüller) |
| Mirror system | 观察动作 → 自身运动表征(见 [Social_Cognition](Social_Cognition.md)) |
| Conceptual metaphor | "温暖的人""沉重的决定"(Lakoff & Johnson) |
| Sensorimotor simulation | 理解 = 心智模拟感觉运动 |
| Gesture-thought | 手势辅助思维(非仅交流) |

---

## 3. 4E 认知

- **Embodied**:身体结构塑造认知
- **Embedded**:环境支撑(offloading)
- **Enacted**:认知 = 主动感觉运动循环(Varela)
- **Extended**:工具/笔记是认知系统延伸(Clark & Chalmers)

---

## 4. Grounded Cognition (Barsalou)

- 概念 = 感觉运动状态的**部分重演**(simulation)
- 反对 amodal symbol(传统命题表征)
- 知觉符号系统(perceptual symbol systems)
- fMRI:概念激活相应感觉运动区

---

## 5. PyTorch — Grounded vs Symbolic 表征

```python
import torch

# Symbolic (amodal): arbitrary one-hot, no sensorimotor content
symbolic_cup = torch.eye(100)[7]                     # token #7

# Grounded: concept = bundle of sensorimotor features
grounded_cup = torch.tensor([
    0.9,  # graspable (motor)
    0.8,  # contains liquid (function)
    0.6,  # cylindrical (visual)
    0.3,  # warm-if-coffee (tactile)
])
# Grounded supports inference/transfer; symbolic does not (grounding problem)
```

---

## 6. 对 AI 的意义

- **Symbol grounding problem**(Harnad 1990):符号意义从何来?
- LLM 批评:文本训练 → 无感觉运动 grounding("stochastic parrot" 争议)
- **Embodied AI**:robot 通过身体互动学习(见 ai-notes 具身智能)
- 多模态 + 具身 是 grounding 一条路径

---

## 7. 强 vs 弱具身

- **强**:认知**必需**身体(无身体无概念)
- **弱**:身体**影响/塑造**认知但非必需
- 多数证据支持弱;强版有反例(抽象概念、先天截肢者概念)
- 当前共识:simulation 是 cognition 的**一部分**机制

---

## 8. 应用

- 教育:具身学习(手势 / 操作 → 数学/科学,见 [Numerical_Cognition](Numerical_Cognition.md))
- 康复:镜像疗法、动作观察治疗
- HCI / VR:具身交互
- 临床:抽象概念障碍(语义性痴呆)

---

## 9. 批评

- 抽象概念(民主、质数)难纯感觉运动 grounding
- 神经激活 ≠ 因果必需(可能 epiphenomenal)
- "Grounding by association" vs 真 constitutive
- LLM 能力上升 → 重燃"需否身体"辩论

---

## 10. Common Pitfalls

### 10.1 一切认知需身体(强版)

抽象概念反例;多数支持弱具身。

### 10.2 神经激活 = 必需

相关 ≠ 因果;需 lesion / TMS 验证 constitutive。

### 10.3 具身 = 反符号一切

是补充/修正经典符号论,非完全否定。

### 10.4 LLM 无身体 = 无理解

争议未决;多模态 + 具身是开放路径。

### 10.5 隐喻 = 纯修辞

Lakoff:概念隐喻塑造推理(非仅语言装饰)。

---

## 11. Related Concepts

- **同节**:[Social_Cognition](Social_Cognition.md)、[Language](Language.md)、[Numerical_Cognition](Numerical_Cognition.md)、[Consciousness](Consciousness.md)
- **系统**:[Motor System](../03_Systems_Neuroscience/Motor_System.md)
- **AI**: symbol grounding、embodied AI — https://jeffliulab.github.io/ai-notes/08_Embodied_Intelligence/

---

## References

1. **Barsalou, L. W.** "Grounded cognition." *Annu Rev Psychol*, 2008.
2. **Lakoff, G. & Johnson, M.** *Metaphors We Live By*. 1980.
3. **Pulvermüller, F.** "Brain mechanisms linking language and action." *Nat Rev Neurosci*, 2005.
4. **Clark, A. & Chalmers, D.** "The extended mind." *Analysis*, 1998.
