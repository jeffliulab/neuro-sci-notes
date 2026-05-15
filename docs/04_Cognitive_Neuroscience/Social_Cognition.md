# 社交认知 (Social Cognition)

> *Social cognition 是 brain 理解 others 心理状态的能力:Theory of Mind (ToM)、empathy、face recognition、social hierarchy。Mirror neurons (1992 Rizzolatti) + mPFC + TPJ + STS 主体。Autism 与 social cognition 缺陷强相关。是 human 与 LLM 重要区分维度。*
>
> **难度**:Intermediate
> **前置知识**:[Cortex](../01_Neuroanatomy/Cortex.md)、[Emotion](Emotion.md)

---

## 1. 核心概念

- **Theory of Mind (ToM)**: 推断他人 belief/desire/intention
- **Empathy**: 共情(affective + cognitive)
- **Mentalizing**: 主动 model 他人 mental state
- **Joint attention**: 共享 attention 焦点
- **Face perception**: FFA 等区识别身份 + 表情

---

## 2. 神经基础

```
       mPFC (mentalizing)
      /
TPJ ─── 推断 others 意图
      \
       STS (action observation)

PCC + precuneus (self vs other)
Amygdala (emotion + threat detection)
Insula (interoception, empathy)
Mirror neurons (premotor + parietal)
```

---

## 3. Mirror Neuron System

- Rizzolatti 1992 在 macaque F5 发现
- Fire 当 monkey 做 action **AND** 看到 others 做 same action
- 人类 inferior frontal gyrus + inferior parietal lobule 有 mirror system
- 假说:imitation、language evolution、empathy 基础(尚有争议)

---

## 4. Theory of Mind 测试

- **False Belief Task** (Sally-Anne, Wimmer & Perner 1983):
  - Sally 放球在篮子,离开
  - Anne 把球移到盒子
  - 问儿童:Sally 回来会去哪找球?
  - 4 岁前:回答"盒子"(不能 model 他人 belief)
  - 4-5 岁:发展出 ToM,回答"篮子"
- LLM (GPT-4) 通过 ToM 测试,但是否真有 ToM 有争议

---

## 5. Empathy 两类

- **Affective empathy**: 感受 others emotion(insula、ACC)
- **Cognitive empathy**: 理解 others perspective(mPFC、TPJ)
- 心理变态(psychopathy):affective ↓,cognitive 正常或 ↑

---

## 6. 病理

- **Autism Spectrum Disorder (ASD)**: ToM、joint attention 缺陷
- **Psychopathy**: affective empathy 缺,但 cognitive empathy 完好
- **Frontotemporal Dementia (FTD)**: social cognition 退化
- **Williams Syndrome**: hyper-social,但其他认知缺陷

---

## 7. PyTorch — 简化 ToM Model

```python
import torch

class TheoryOfMindModel(torch.nn.Module):
    """Predict another agent's belief based on observation."""
    def __init__(self, state_dim=10, hidden=64):
        super().__init__()
        self.observer = torch.nn.GRU(state_dim, hidden, batch_first=True)
        self.belief_head = torch.nn.Linear(hidden, state_dim)
    
    def forward(self, other_observations):
        """Given what 'other' saw, infer their belief."""
        h, _ = self.observer(other_observations)
        belief = self.belief_head(h[:, -1])  # last hidden state
        return belief
```

---

## 8. 进化视角

- Dunbar (1992): 灵长类 neocortex size ∝ social group size
- "Social brain hypothesis": 大 brain 进化为应对 social complexity
- Human ~ 150 (Dunbar number) 维持稳定社交关系

---

## 9. AI / LLM 与 ToM

- GPT-4 通过 Sally-Anne 等 ToM benchmark
- 但争议:LLM 是 statistical pattern matching 还是真有 mental state model?
- Multi-agent reasoning 仍是 LLM 弱项

---

## 10. Common Pitfalls

### 10.1 Mirror Neurons 神化

并非"empathy细胞";功能复杂得多。

### 10.2 ToM ≠ Empathy

ToM 是认知能力;empathy 涉感受。

### 10.3 Autism 不缺 empathy

ASD 是 social communication 障碍;empathy 可能正常或 atypical。

### 10.4 LLM ToM 测试

通过测试 ≠ 真理解;可能是 training data 中有 pattern。

### 10.5 Mentalizing Network 不仅社交

也涉自我反思、自传记忆。

---

## 11. Related Concepts

- **同节**:[Emotion](Emotion.md)、[Consciousness](Consciousness.md)
- **解剖**:[Cortex](../01_Neuroanatomy/Cortex.md)
- **疾病**:Autism

---

## References

1. **Premack & Woodruff** "Does the chimpanzee have a theory of mind?" *BBS*, 1978.
2. **Rizzolatti, G. et al.** "Premotor cortex and the recognition of motor actions." *Cogn Brain Res*, 1996.
3. **Frith, U. & Frith, C. D.** "Mechanisms of social cognition." *Annu Rev Psychol*, 2012.
4. **Dunbar, R. I. M.** "The social brain hypothesis." *Evol Anthropol*, 1998.
