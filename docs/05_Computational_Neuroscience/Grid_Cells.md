# Grid Cells — 神经空间地图

> *Grid cells 是 entorhinal cortex (EC) 中神经元,fire 当 animal 位于 hexagonal 网格 vertices(等距点)。Moser & Moser 2005 发现(Nobel 2014)。与 hippocampus place cells 协作 → 空间地图。Grid 提供 metric (距离 / 方向),place 提供 episodic memory。Banino 2018 RNN 自然 emerge grid-like pattern,引深 AI 与 brain 关联。*
>
> **难度**:Advanced
> **前置知识**:[Hippocampus_Anatomy](../01_Neuroanatomy/Hippocampus_Anatomy.md)、[Hippocampus_Memory](../03_Systems_Neuroscience/Hippocampus_Memory.md)

---

## 1. 发现

- O'Keefe 1971: hippocampus place cells
- Moser & Moser 2005: medial entorhinal cortex (MEC) grid cells
- 2014 Nobel Prize in Physiology
- 同 lab 还发现 head direction cells、border cells、speed cells

---

## 2. Grid Pattern

```
Rat 在 1 m × 1 m 空间走,某 MEC neuron firing:

. . X . . X . . X .
. X . . X . . X . .
X . . X . . X . . X
. . X . . X . . X .
. X . . X . . X . .

(60° spaced equilateral triangle grid)
```

- 周期 ~ 30 cm-1 m
- 多 spatial frequency(MEC dorsal-ventral 增大)
- 同 module 内 neuron 相同 frequency,不同 phase

---

## 3. 神经基础

- Layer II MEC stellate cells
- Continuous attractor network (CAN) 模型
- Inhibitory ring + path integration
- Border cells 修正漂移

---

## 4. Modular 组织

- **Modules** ~ 5-10 个
- 每 module 内同 spatial period,~ √2 ratio between modules
- 类似数字编码(高低位)
- 理论上可 encode 大空间

---

## 5. Path Integration

- 用 self-motion 信号 (速度 + 方向) 更新位置
- Grid cells 不需 visual cue 也能 maintain pattern(dark room)
- 与 dead reckoning 相同

---

## 6. PyTorch — Grid-like Pattern from RNN

```python
import torch
import torch.nn as nn

class PathIntegrationRNN(nn.Module):
    """Banino 2018 类似 setup."""
    def __init__(self, hidden=128, n_grid=512):
        super().__init__()
        self.rnn = nn.LSTM(input_size=2, hidden_size=hidden, batch_first=True)
        self.grid_head = nn.Linear(hidden, n_grid)
        self.pos_head = nn.Linear(hidden, 2)
    
    def forward(self, velocities):
        # velocities: (B, T, 2)
        h, _ = self.rnn(velocities)
        positions = self.pos_head(h)
        grid_act = self.grid_head(h)
        return positions, grid_act
```

训练这个 network 预测 position → hidden unit 自然 emerge grid-like firing!

---

## 7. Grid 与 Place 关系

- MEC grid → Hippocampus CA3 place
- 多 grid module combined → 唯一 place(类似 Fourier basis)
- Reverse path: place cell 可 generate / replay sequences

---

## 8. Grid 抽象 use

Grid 不限空间:
- **Concept space**: 2D 任务空间(Constantinescu 2016)
- **Time**: temporal grid cells
- **Social space**:friend 数 + status?
- LLM internal grid-like representation?(实验探索)

---

## 9. 病理

- **Alzheimer**: EC 早期受损 → spatial 迷失早 symptom
- **Aging**: grid drift
- **3xTg-AD mouse**: grid module degrade

---

## 10. AI / Banino 2018

- DeepMind RNN 训练 do path integration
- Hidden units emerge hexagonal grid pattern
- 类似 brain
- 但 follow-up 显示 architecture choice 重要;非必然 emerge

---

## 11. Common Pitfalls

### 11.1 Grid = 唯一位置

不;一 grid 周期对 多位置 → 需多 module 联合 decode。

### 11.2 Grid 只 spatial

不;abstract space (concept) 也有 grid。

### 11.3 Path Integration 完美

实际 漂移,需 visual cue / border cells correct。

### 11.4 RNN 训练自动 emerge grid

Architecture 影响大;并非任何 RNN。

### 11.5 Grid = Place 同

不同 brain region,不同 properties (周期 vs 单峰)。

---

## 12. Related Concepts

- **同节**:[Hopfield_Networks](Hopfield_Networks.md)、[Predictive_Coding](Predictive_Coding.md)
- **解剖**:[Hippocampus_Anatomy](../01_Neuroanatomy/Hippocampus_Anatomy.md)
- **系统**:[Hippocampus_Memory](../03_Systems_Neuroscience/Hippocampus_Memory.md)
- **AI**: RNN spatial reasoning

---

## References

1. **Hafting, T. et al.** "Microstructure of a spatial map in the entorhinal cortex." *Nature*, 2005.
2. **Moser, E. I. et al.** "Grid cells and cortical representation." *Nat Rev Neurosci*, 2014.
3. **Banino, A. et al.** "Vector-based navigation using grid-like representations in artificial agents." *Nature*, 2018.
4. **Constantinescu, A. O., O'Reilly, J. X., Behrens, T. E. J.** "Organizing conceptual knowledge in humans with a gridlike code." *Science*, 2016.
