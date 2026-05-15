# 行为神经科学 (Behavioral Neuroscience)

> *行为是 neuroscience 的最终因变量。"Neuroscience needs behavior"(Krakauer 2017):无良好行为量化,神经数据无意义。从经典 ethology (Tinbergen 4 问) 到现代 computational ethology (DeepLabCut 无标记追踪)。行为范式设计决定能问什么问题。*
>
> **难度**:Beginner-Intermediate
> **前置知识**:[Research Methods](Research_Methods.md)

---

## 1. 为何行为是核心

- 行为 = brain 的 output(最终因变量)
- 神经活动只有相对行为才有意义
- Krakauer 2017:过度还原(neuron-first)忽视 behavior 结构
- "理解 brain = 解释 behavior 如何产生"

---

## 2. Tinbergen 四问 (1963)

理解任一行为需问 4 个层次:
1. **Causation** (机制):什么 trigger?(proximate)
2. **Development** (个体发育):如何 ontogeny 形成?
3. **Function** (适应价值):为何 survival/reproduction 有利?(ultimate)
4. **Evolution** (系统发育):演化史?

→ 类似 Marr levels(见 [Levels of Analysis](Levels_of_Analysis.md))。

---

## 3. 经典 ethology

- Lorenz:imprinting(印随)
- Tinbergen:supernormal stimuli、fixed action patterns
- von Frisch:蜜蜂舞蹈语言
- 三人 1973 Nobel(动物行为)

---

## 4. 行为范式

| 范式 | 测量 |
|---|---|
| Morris water maze | 空间记忆 |
| Fear conditioning | 关联学习 |
| Operant (Skinner box) | 强化学习 |
| Open field | 焦虑 / 探索 |
| Forced swim | 抑郁样(争议) |
| 2AFC / random dots | 知觉决策 |
| Delayed match-to-sample | 工作记忆 |
| Reaching task | 运动控制 |

---

## 5. Computational Ethology

- **DeepLabCut** (Mathis 2018):无标记姿态估计(transfer learning)
- **SLEAP**、**MoSeq**(行为 motif 无监督分解)
- 自动量化自然行为(非简化任务)
- 与神经记录同步

---

## 6. PyTorch — 行为分类(姿态 → 行为)

```python
import torch
import torch.nn as nn

class BehaviorClassifier(nn.Module):
    """Pose keypoints → behavior label (groom/walk/rear)."""
    def __init__(self, n_keypoints=15, n_behaviors=5):
        super().__init__()
        self.lstm = nn.LSTM(n_keypoints*2, 64, batch_first=True)
        self.fc = nn.Linear(64, n_behaviors)
    def forward(self, pose_seq):
        # pose_seq: (B, T, n_keypoints*2)
        h, _ = self.lstm(pose_seq)
        return self.fc(h[:, -1])
```

---

## 7. 心理物理学 (Psychophysics)

- 量化 stimulus → percept 关系
- **Weber-Fechner law**: $\Delta I / I = $ const
- **Stevens power law**: $\psi = k I^a$
- **Signal detection theory**: d' (sensitivity) + criterion
- 桥接行为与神经编码

---

## 8. 行为 vs 神经:因果

- Correlation:neuron firing ↔ behavior
- 需 perturbation:optogenetics / lesion 证 causal
- 但 perturbation 也有 off-target、compensation
- 行为读出必须 sensitive(否则漏检效应)

---

## 9. 自然行为 vs 实验室任务

- 实验室任务:可控但 unnatural、over-trained
- 自然行为:生态有效但难量化
- 趋势:naturalistic + 高维行为追踪 + 神经记录

---

## 10. Common Pitfalls

### 10.1 神经数据 > 行为

无良好行为量化,神经数据无解释力(Krakauer)。

### 10.2 行为简单

行为高维 + 序列结构;过简任务丢信息。

### 10.3 实验室任务 = 自然行为

Over-trained 任务可能非自然 strategy。

### 10.4 Correlation = 机制

需 causal perturbation;但 perturbation 也有 confound。

### 10.5 强迫游泳 = 抑郁

模型效度争议;勿过度解读动物模型。

---

## 11. Related Concepts

- **同节**:[Research Methods](Research_Methods.md)、[Levels of Analysis](Levels_of_Analysis.md)、[Comparative Neuroscience](Comparative_Neuroscience.md)
- **认知**:[Decision Making](../04_Cognitive_Neuroscience/Decision_Making.md)
- **系统**:[Motor System](../03_Systems_Neuroscience/Motor_System.md)
- **AI**: pose estimation、行为分类

---

## References

1. **Tinbergen, N.** "On aims and methods of ethology." *Z Tierpsychol*, 1963.
2. **Krakauer, J. W. et al.** "Neuroscience needs behavior: correcting a reductionist bias." *Neuron*, 2017.
3. **Mathis, A. et al.** "DeepLabCut: markerless pose estimation of user-defined body parts with deep learning." *Nat Neurosci*, 2018.
4. **Datta, S. R. et al.** "Computational neuroethology: a call to action." *Neuron*, 2019.
