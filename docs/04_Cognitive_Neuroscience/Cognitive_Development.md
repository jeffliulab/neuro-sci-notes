# 认知发展 (Cognitive Development)

> *认知如何从婴儿到成人构建?Piaget 阶段论 → 现代:婴儿核心知识(core knowledge)远超 Piaget 估计 + 渐进 + 领域特异。Theory of mind ~ 4 岁、EF 至 25 岁(PFC 髓鞘化)。Nativism vs empiricism → 现代建构主义 + Bayesian "child as scientist"。是 AI developmental learning 灵感源。*
>
> **难度**:Intermediate
> **前置知识**:[Neurodevelopment](../00_Foundations/Neurodevelopment.md)、[Social_Cognition](Social_Cognition.md)

---

## 1. Piaget 四阶段(经典,部分被修正)

| 阶段 | 年龄 | 标志 |
|---|---|---|
| Sensorimotor | 0-2 | 物体永久性 |
| Preoperational | 2-7 | 符号 / 语言;自我中心、无守恒 |
| Concrete operational | 7-11 | 守恒、逻辑(具体) |
| Formal operational | 11+ | 抽象、假设演绎 |

→ 框架开创性,但低估婴儿能力 + 阶段过刚性。

---

## 2. Core Knowledge(现代修正)

婴儿天生(或极早)有领域特异系统(Spelke):
- **物体**(永久性、连续性)— 远早于 Piaget(违背期望范式)
- **数量**(ANS,见 [Numerical_Cognition](Numerical_Cognition.md))
- **主体/意图**(goal-directed)
- **空间**(几何)
- 后续学习在这些核心系统上建构

---

## 3. 关键里程碑

- 物体永久性:~ 3-4 月(看时间法)而非 Piaget 8 月
- 联合注意:~ 9-12 月
- Theory of Mind(false belief):~ 4 岁(隐式更早)— 见 [Social_Cognition](Social_Cognition.md)
- 语言爆发:~ 18-24 月
- EF / 抽象推理:青春期-25 岁(PFC,见 [Executive Function](Executive_Function.md))

---

## 4. PyTorch — 违背期望(VOE)范式逻辑

```python
import torch

def violation_of_expectation(predicted, observed, infant_model):
    """Longer looking = surprise = expectation violated (Spelke method)."""
    surprise = (predicted - observed).abs().mean()
    looking_time = 1.0 + 3.0 * torch.sigmoid(surprise - 0.5)
    # If infant has 'object permanence' prior, impossible event -> high surprise
    return looking_time
```

---

## 5. 学习机制

- **统计学习**:婴儿提取语音 / 视觉转移概率(Saffran 1996)
- **Bayesian "child as scientist"**:主动假设检验(Gopnik)
- **社会学习**:模仿、自然教学(natural pedagogy)
- **建构主义**:核心知识 + 经验 → 渐进精化
- 非纯白板,非纯先天 → 交互

---

## 6. Nature vs Nurture(已超越二分)

- 极端 nativism / empiricism 均被否定
- 现代:**probabilistic epigenesis**、基因×环境交互
- 经验依赖可塑性 + critical periods(见 [Neurodevelopment](../00_Foundations/Neurodevelopment.md))
- 个体差异 = 遗传 + 环境 + 随机发育噪声

---

## 7. 神经发展关联

- 感觉皮层早成熟 → 关联/PFC 最晚(~25 岁)
- Synaptic 过量 → 修剪(经验塑形)
- 髓鞘化由后向前(PFC 最后)→ 解释青少年风险决策
- 关键期(语言、视觉)

---

## 8. 与 AI / Developmental ML

- "Child as scientist" ↔ 主动学习、好奇心驱动 RL
- Core knowledge ↔ 归纳偏置(inductive bias)、object-centric models
- Curriculum learning ↔ 发展顺序
- Few-shot / 快速概括:儿童远超当前 AI(样本效率差异)

---

## 9. 临床

- 发育迟缓 / ID
- 自闭症:社会认知 + ToM 轨迹异常(见 [Autism](../08_Neuro_Disorders/Autism.md))
- 剥夺(罗马尼亚孤儿)→ 敏感期 + 可塑性证据
- 早期干预窗口

---

## 10. Common Pitfalls

### 10.1 Piaget 阶段严格 + 完整

低估婴儿;阶段非刚性,领域特异 + 渐进。

### 10.2 婴儿是白板

Core knowledge:物体/数/主体先天系统。

### 10.3 Nature vs Nurture 二选一

已被交互论 / 概率表观遗传取代。

### 10.4 ToM 4 岁突然出现

显式 ~4 岁,隐式更早(渐进)。

### 10.5 发展 = 单调进步

有 U 型 / 退行 / 重组(非线性)。

---

## 11. Related Concepts

- **同节**:[Social_Cognition](Social_Cognition.md)、[Executive Function](Executive_Function.md)、[Numerical_Cognition](Numerical_Cognition.md)、[Language](Language.md)
- **基础**:[Neurodevelopment](../00_Foundations/Neurodevelopment.md)
- **疾病**:[Autism](../08_Neuro_Disorders/Autism.md)
- **AI**: developmental learning、inductive bias

---

## References

1. **Piaget, J.** *The Origins of Intelligence in Children*. 1952.
2. **Spelke, E. S. & Kinzler, K. D.** "Core knowledge." *Dev Sci*, 2007.
3. **Gopnik, A. et al.** *The Scientist in the Crib*. 1999.
4. **Saffran, J. R. et al.** "Statistical learning by 8-month-old infants." *Science*, 1996.
