# 分析层次 (Marr's Levels of Analysis)

> *David Marr 1982 提出理解任何信息处理系统(brain 或 AI)需 3 个层次:computational (what & why)、algorithmic (how representation + process)、implementational (physical substrate)。这是认知科学 + neuroscience + AI 的共同方法论框架。理解层次混淆是许多 neuro-AI 争论的根源。*
>
> **难度**:Beginner-Intermediate
> **前置知识**:无

---

## 1. 三层次

| 层次 | 问题 | 例(视觉) |
|---|---|---|
| **Computational** | 解决什么问题?为何? | 从 2D 图恢复 3D 结构 |
| **Algorithmic** | 用什么表征 + 算法? | edge detection → 表面重建 |
| **Implementational** | 物理上如何实现? | V1 simple cells, neurons |

---

## 2. Computational Level

- 系统计算**什么** + **为什么**(目标 + 约束)
- 与具体实现无关
- 例:加法器的目标是"做加法",不论是算盘还是 CPU
- Marr 强调:这层最易被忽视但最重要

---

## 3. Algorithmic Level

- **表征** (representation):信息如何编码?
- **算法**:如何从输入算到输出?
- 同一 computation 可有多种 algorithm
- 例:排序可用 quicksort 或 mergesort

---

## 4. Implementational Level

- 物理 substrate:neurons / silicon / 齿轮
- 同一 algorithm 可多种实现
- Neuroscience 大量工作在此层(电生理、imaging)

---

## 5. 为何分层重要

- 不同层次的解释**互补**,非竞争
- "neuron 怎样 fire"(impl) ≠ "brain 解决什么问题"(comp)
- 跨层混淆 → 无效争论
- Marr:仅研究 implementation 无法理解 function("理解羽毛≠理解飞行")

---

## 6. 与 AI 对应

| Marr | AI |
|---|---|
| Computational | 任务定义 + loss function |
| Algorithmic | 模型架构 + 训练算法 |
| Implementational | GPU / TPU / 硬件 |

LLM:computational = next-token prediction;algorithmic = Transformer;impl = CUDA。

---

## 7. PyTorch — 三层次示意

```python
import torch

# Computational: WHAT — minimize prediction error (the goal)
def loss_fn(pred, target):
    return ((pred - target) ** 2).mean()

# Algorithmic: HOW — gradient descent on a linear model (representation+process)
class LinearModel(torch.nn.Module):
    def __init__(self): 
        super().__init__()
        self.w = torch.nn.Linear(10, 1)
    def forward(self, x): return self.w(x)

# Implementational: physical — runs on CPU or CUDA
device = 'cuda' if torch.cuda.is_available() else 'cpu'
model = LinearModel().to(device)
```

---

## 8. 历史

- **Marr & Poggio 1976**: 立体视觉,首用框架
- **Marr 1982** *Vision*: 完整阐述(死后出版)
- 影响整个 cognitive science
- 后续:Poggio 添加 "learning" 第四层(2012)

---

## 9. 现代讨论

- **Connectomics**:有人批评"只在 implementation 层"
- **Deep learning ↔ brain**:常混 algorithmic 与 implementational
- **Normative models**:强调 computational level(为何这样设计)
- Krakauer 2017:neuroscience 过度依赖 implementation,缺 behavior(computational)

---

## 10. Common Pitfalls

### 10.1 层次有优劣

3 层互补,无高低;不同问题需不同层。

### 10.2 Implementation 决定一切

知道每 neuron 不等于理解 function(需 computational)。

### 10.3 Computational = 数学

是"目标 + 约束"的抽象描述,不仅是公式。

### 10.4 一 algorithm 一 implementation

多对多映射;同算法多硬件,同硬件多算法。

### 10.5 Marr 框架过时

仍是 neuro-AI 跨学科沟通的核心框架。

---

## 11. Related Concepts

- **同节**:[Research Methods](Research_Methods.md)、[Neuroscience History](Neuroscience_History.md)
- **计算**:[Predictive Coding](../05_Computational_Neuroscience/Predictive_Coding.md)、[Bayesian Brain](../05_Computational_Neuroscience/Bayesian_Brain.md)
- **AI**: 任务 vs 架构 vs 硬件

---

## References

1. **Marr, D.** *Vision*. W.H. Freeman, 1982.
2. **Marr, D. & Poggio, T.** "From understanding computation to understanding neural circuitry." *AI Memo*, 1976.
3. **Poggio, T.** "The levels of understanding framework, revised." *Perception*, 2012.
4. **Krakauer, J. W. et al.** "Neuroscience needs behavior." *Neuron*, 2017.
