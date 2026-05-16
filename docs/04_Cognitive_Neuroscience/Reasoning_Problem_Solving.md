# 推理与问题解决 (Reasoning & Problem Solving)

> *推理 = 从前提推结论:演绎、归纳、溯因、类比。问题解决 = 搜索状态空间达目标。PFC(尤 rostrolateral)+ 顶叶 + 工作记忆主体。人类推理充满启发式偏差(Kahneman/Tversky)。是 human-LLM 比较最热维度(链式推理、System 2)。*
>
> **难度**:Intermediate
> **前置知识**:[Executive Function](Executive_Function.md)、[Decision_Making](Decision_Making.md)

---

## 1. 推理类型

| 类型 | 方向 | 例 |
|---|---|---|
| **演绎** | 一般→特殊(必然) | 三段论 |
| **归纳** | 特殊→一般(概率) | 科学归纳 |
| **溯因** | 观察→最佳解释 | 诊断 |
| **类比** | 源域→目标域映射 | 比喻、迁移 |

---

## 2. 神经基础

- **Rostrolateral PFC (RLPFC/BA10)**:关系整合、类比、多约束
- **dlPFC**:工作记忆维持前提
- **顶叶(IPS)**:关系 / 数量表征
- **Fronto-parietal network**:fluid reasoning(与 g 因子相关)
- 损伤 → 抽象 / 类比缺陷

---

## 3. 经典任务

- **Raven's Progressive Matrices**:fluid intelligence 金标准
- **Wason selection task**:演绎(多数人错!→ 内容效应)
- **Tower of Hanoi**:规划 / 子目标
- **类比 A:B::C:?**:关系映射(RLPFC)
- **三段论**:演绎 + belief bias

---

## 4. 双系统 (Kahneman)

- **System 1**:快、自动、启发式、直觉
- **System 2**:慢、费力、规则、analytic
- 推理偏差源于 S1 越权 + S2 监督不足
- 与 [Metacognition](Metacognition.md) 相关

---

## 5. 启发式与偏差

| 偏差 | 描述 |
|---|---|
| Confirmation bias | 偏好证实假设 |
| Belief bias | 结论可信度影响逻辑判断 |
| Availability | 易想起 = 高估概率 |
| Anchoring | 锚定初始值 |
| Base-rate neglect | 忽略基率 |
| Conjunction fallacy | "Linda 问题" |

---

## 6. PyTorch — 类比关系映射(玩具)

```python
import torch

def analogy_solve(a, b, c, candidates):
    """A:B :: C:? — vector relational mapping (word2vec-style)."""
    relation = b - a                       # extract relation
    target = c + relation                  # apply to C
    sims = torch.nn.functional.cosine_similarity(
        target.unsqueeze(0), candidates)
    return candidates[sims.argmax()]       # best D

# king - man + woman ≈ queen — same relational structure as RLPFC analogy
```

---

## 7. 问题解决

- **状态空间搜索**:初始 → 操作 → 目标(Newell & Simon)
- **手段-目的分析**(means-ends)
- **顿悟**(insight):表征重构(aha! — 右 ATL/扣带)
- **专家 vs 新手**:chunking + 模式识别(见 [Working Memory](Working_Memory.md))
- **功能固着**、心理定势(障碍)

---

## 8. 与 AI / LLM

- LLM "chain-of-thought" ≈ 外显 System 2 步骤
- LLM 仍有类似人的偏差(训练数据继承)
- 关系推理 / 系统泛化 / 组合性:LLM 弱项(争议)
- RLPFC 关系整合 ↔ Transformer 关系归纳偏置研究

---

## 9. 个体差异

- Fluid reasoning ≈ g 因子核心
- 与工作记忆容量高相关(非等同)
- "理性"(rationality)与智力可分离(Stanovich)
- 训练迁移有限(同 brain training 争议)

---

## 10. Common Pitfalls

### 10.1 人类是逻辑推理者

充满启发式偏差;规范逻辑非默认。

### 10.2 演绎 = 归纳

演绎必然、归纳概率;神经 + 错误模式不同。

### 10.3 偏差 = 缺陷

启发式生态有效(快 + 省);偏差是 trade-off。

### 10.4 LLM CoT = 真推理

可能是 pattern;忠实性(faithfulness)争议未解。

### 10.5 推理 = IQ

相关;但 rationality ≠ intelligence(可分离)。

---

## 11. Related Concepts

- **同节**:[Executive Function](Executive_Function.md)、[Decision_Making](Decision_Making.md)、[Metacognition](Metacognition.md)、[Working Memory](Working_Memory.md)
- **解剖**:[Cortex](../01_Neuroanatomy/Cortex.md)(RLPFC)
- **AI**: chain-of-thought、关系推理

---

## References

1. **Kahneman, D.** *Thinking, Fast and Slow*. 2011.
2. **Knowlton, B. J. et al.** "A neurocomputational system for relational reasoning." *Trends Cogn Sci*, 2012.
3. **Newell, A. & Simon, H. A.** *Human Problem Solving*. 1972.
4. **Tversky, A. & Kahneman, D.** "Judgment under uncertainty: heuristics and biases." *Science*, 1974.
