# 数字认知 (Numerical Cognition)

> *数字认知研究脑如何表征 + 操作数量。Approximate Number System (ANS):天生、对数压缩、Weber 定律,人婴 + 动物共有。Exact 符号数:文化习得,IPS(顶内沟)是核心。Dehaene "number sense"。Dyscalculia 是其障碍。连接感知、空间(心理数轴)与教育。*
>
> **难度**:Intermediate
> **前置知识**:[Somatosensory](../03_Systems_Neuroscience/Somatosensory.md)、[Reasoning_Problem_Solving](Reasoning_Problem_Solving.md)

---

## 1. 两套系统

| 系统 | 特征 |
|---|---|
| **ANS**(approximate) | 天生、无符号、对数、近似、动物共有 |
| **Exact symbolic** | 文化习得、精确、需语言/符号 |
| **Subitizing** | ≤ 4 个瞬时精确计数(并行) |

---

## 2. ANS 性质

- **Weber 定律**:辨别 ratio 依赖($\Delta n / n$ 恒定)
- **距离效应**:8 vs 9 比 2 vs 9 难
- **大小效应**:大数辨别更难
- 婴儿(Xu & Spelke)、猴、鱼、蜂 均有 → 演化古老

---

## 3. 神经基础

- **IPS(intraparietal sulcus)**:数量表征核心(跨符号/非符号/通道)
- HIPS(horizontal IPS):抽象数量
- 角回:符号算术检索(verbal)
- PFC:工作记忆 + 复杂计算
- "Number neurons"(猴 PFC/IPS,Nieder)— 调谐到特定 numerosity

---

## 4. 心理数轴 (Mental Number Line)

- 数 ↔ 空间映射(小左大右,西方文化)
- **SNARC 效应**:小数左手快,大数右手快
- 与空间注意 / 顶叶重叠
- 文化依赖(阅读方向影响方向)

---

## 5. PyTorch — Numerosity 调谐 + 对数

```python
import torch

def number_neuron(n, preferred, sigma=0.3):
    """Tuning curve on LOG scale (Weber-Fechner) — like IPS number neurons."""
    return torch.exp(-((torch.log(torch.tensor(float(n)))
                        - torch.log(torch.tensor(float(preferred)))) ** 2)
                     / (2 * sigma ** 2))

# Distance + size effects emerge naturally from log-Gaussian tuning
```

---

## 6. 发展

- 婴儿即有 ANS(大数比、加减期望违背)
- ANS acuity 与后期数学成就相关(争议强度)
- 符号习得 = 建立 ANS↔符号映射("symbol grounding")
- 手指计数 → 体现 embodied(见 [Embodied_Cognition](Embodied_Cognition.md))

---

## 7. Dyscalculia(计算障碍)

- ~ 3-7%,数学领域特异学习障碍
- 假说:ANS 缺陷 / 符号-数量映射缺陷 / 工作记忆
- IPS 结构/激活异常
- 与 dyslexia 共病但可分离

---

## 8. 动物数感

- 猴:序数 + 基数 + 简单算术
- 蜂:0 概念、加减
- 鱼 / 鸟:数量辨别
- → ANS 是跨物种保守(非语言依赖)

---

## 9. 与 AI

- NN 训练计数 → 自发 numerosity 调谐(类 IPS,Nasr 2019)
- LLM 算术弱(tokenization + 无符号 grounding)
- "Number sense" 是 grounding 问题缩影
- 工具增强(calculator)≈ 人用外部符号

---

## 10. Common Pitfalls

### 10.1 数感 = 算术能力

ANS(近似)≠ 符号精确算术;可双分离。

### 10.2 数字表征线性

ANS 是对数压缩(Weber);儿童早期更对数。

### 10.3 仅人类有数感

ANS 跨物种(猴/蜂/鱼);非语言依赖。

### 10.4 Subitizing = 快速计数

≤ 4 并行直接感知,机制不同于序列计数。

### 10.5 心理数轴普适

方向文化依赖(阅读方向)。

---

## 11. Related Concepts

- **同节**:[Reasoning_Problem_Solving](Reasoning_Problem_Solving.md)、[Working Memory](Working_Memory.md)、[Embodied_Cognition](Embodied_Cognition.md)
- **解剖**:[Cortex](../01_Neuroanatomy/Cortex.md)(IPS)
- **疾病**:Dyscalculia
- **AI**: numerosity in NN、symbol grounding

---

## References

1. **Dehaene, S.** *The Number Sense*. Oxford, 1997.
2. **Nieder, A.** "The neuronal code for number." *Nat Rev Neurosci*, 2016.
3. **Xu, F. & Spelke, E. S.** "Large number discrimination in 6-month-old infants." *Cognition*, 2000.
4. **Nasr, K. et al.** "Number detectors spontaneously emerge in a deep neural network." *Sci Adv*, 2019.
