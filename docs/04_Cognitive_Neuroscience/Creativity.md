# 创造力 (Creativity)

> *创造力 = 产生既**新颖**又**有用**的想法。非单一脑区:依赖 default mode network(生成)↔ executive control network(评估)动态切换。发散思维 vs 收敛思维。与 LLM 生成能力对比是当代焦点(AI 能否"真创造")。*
>
> **难度**:Intermediate
> **前置知识**:[Executive Function](Executive_Function.md)、[Mental_Imagery](Mental_Imagery.md)

---

## 1. 定义

- 创造性产品 = **新颖**(novel) + **有用/恰当**(useful/appropriate)
- 双标准缺一不可(纯随机新 ≠ 创造)
- 层级:little-c(日常)↔ Big-C(变革性)

---

## 2. 发散 vs 收敛

| | 发散思维 | 收敛思维 |
|---|---|---|
| 方向 | 多解、扩散 | 单一最优解 |
| 测量 | AUT(替代用途)、流畅/灵活/独创 | RAT(远距联想) |
| 网络 | DMN 主 | ECN 主 |

创造 = 二者**动态协作**。

---

## 3. 网络模型

```
Default Mode Network (DMN)
  ↳ 自发生成、联想、心智漫游
        ⇅ 动态切换 + 耦合
Executive Control Network (ECN)
  ↳ 评估、筛选、约束、目标导向
Salience Network
  ↳ 在两者间切换调度
```

→ 创造非"右脑"神话;是网络间动态交互(Beaty 2016)。

---

## 4. 神经证据

- 高创造者:DMN-ECN **耦合增强**(通常二者反相关)
- Aha! 顿悟:右 ATL + 前扣带(重构)
- 多巴胺:探索 / 新异(适度;倒 U)
- 额叶损伤:有时去抑制 → 异常"创造"(paradoxical)

---

## 5. PyTorch — 发散生成 + 收敛筛选

```python
import torch

def creative_process(seed, n_candidates=20, temperature=1.5):
    """DMN: high-temperature divergent generation; ECN: convergent select."""
    # Divergent: many varied candidates (high entropy)
    candidates = seed + temperature * torch.randn(n_candidates, seed.numel())
    # Convergent: score novelty * usefulness, pick best
    novelty = (candidates - seed).norm(dim=1)
    usefulness = torch.sigmoid(-(candidates.abs().mean(dim=1) - 1.0))
    score = novelty * usefulness                    # both required
    return candidates[score.argmax()]
```

---

## 6. 阶段模型 (Wallas 1926)

1. **Preparation**:沉浸问题
2. **Incubation**:放下 → 无意识加工(DMN / 睡眠)
3. **Illumination**:顿悟(aha!)
4. **Verification**:评估 + 实现(ECN)

孵化效应:离开问题反促解(注意 + DMN)。

---

## 7. 个体 + 状态因素

- **开放性**(Big Five):最稳健人格相关
- 适度精神病理(轻躁、schizotypy)与创造弱相关(争议)
- 心境:正性 + 放松 → 发散;但约束有时助创造
- 专长悖论:需领域知识但易固着(见 [Reasoning_Problem_Solving](Reasoning_Problem_Solving.md))

---

## 8. 与 AI / LLM

- LLM 生成 = 高 temperature 采样 + 训练分布重组
- 争议:是"组合创造"还是"真新颖"?(无意图 / 评估 / 世界 grounding)
- 人机协作创造("centaur")
- DMN-ECN 切换 ↔ generate-then-select 架构

---

## 9. 培养创造力

- 发散训练(brainstorm、SCAMPER)有限迁移
- 跨域知识 + 类比(见 [Reasoning_Problem_Solving](Reasoning_Problem_Solving.md))
- 孵化(休息 / 睡眠 — 见 [Sleep_Wake](../03_Systems_Neuroscience/Sleep_Wake.md))
- 心理安全环境(降评估焦虑)

---

## 10. Common Pitfalls

### 10.1 创造 = 右脑

伪科学;是 DMN-ECN 全脑网络交互。

### 10.2 新颖 = 创造

须新颖 **且** 有用;纯随机不算。

### 10.3 创造 = 发散思维

需发散 **+** 收敛动态协作。

### 10.4 创造与智力无关

中度相关("阈值假说"争议:IQ~120 后弱化)。

### 10.5 LLM = 真创造

组合重组 vs 意图性新颖,哲学 + 实证争议未决。

---

## 11. Related Concepts

- **同节**:[Executive Function](Executive_Function.md)、[Mental_Imagery](Mental_Imagery.md)、[Reasoning_Problem_Solving](Reasoning_Problem_Solving.md)、[Consciousness](Consciousness.md)
- **系统**:[Sleep_Wake](../03_Systems_Neuroscience/Sleep_Wake.md)、[Reward_System](../03_Systems_Neuroscience/Reward_System.md)
- **AI**: 生成模型、人机协作

---

## References

1. **Beaty, R. E. et al.** "Creative cognition and brain network dynamics." *Trends Cogn Sci*, 2016.
2. **Guilford, J. P.** "Creativity." *Am Psychol*, 1950.
3. **Wallas, G.** *The Art of Thought*. 1926.
4. **Dietrich, A. & Kanso, R.** "A review of EEG, ERP, and neuroimaging studies of creativity and insight." *Psychol Bull*, 2010.
