# 认知地图 (Cognitive Maps)

> *Cognitive map = 脑对空间(及抽象关系)的内部结构化表征。Tolman 1948 行为提出 → O'Keefe place cells + Moser grid cells 生理证实(见 [Grid Cells](../05_Computational_Neuroscience/Grid_Cells.md))。海马-内嗅系统不仅编码空间,还泛化到概念/社会"空间"(Behrens 2018)。是导航 + 关系推理 + 记忆的统一框架。*
>
> **难度**:Intermediate
> **前置知识**:[Memory_Systems](Memory_Systems.md)、[Grid Cells](../05_Computational_Neuroscience/Grid_Cells.md)

---

## 1. 起源 — Tolman (1948)

- 行为主义时代:鼠走迷宫非纯 S-R 链
- "Latent learning"+ 抄近路 → 内部**地图**
- 反纯刺激-反应,提出认知表征
- 数十年后被神经生理证实

---

## 2. 空间细胞家族

| 细胞 | 编码 | 区域 |
|---|---|---|
| Place cell | 特定位置 | 海马(O'Keefe 1971) |
| Grid cell | 六边形栅格 | MEC(Moser 2005) |
| Head direction | 朝向 | presubiculum 等 |
| Border/boundary | 边界 | subiculum/MEC |
| Speed cell | 移动速度 | MEC |
| Object-vector | 相对物体方位 | MEC |

→ 联合构成度量 + 拓扑地图(见 [Grid Cells](../05_Computational_Neuroscience/Grid_Cells.md))。

---

## 3. 地图类型

- **Egocentric**(自我中心):相对身体(顶叶)
- **Allocentric**(环境中心):世界坐标(海马)
- 导航需二者转换(retrosplenial cortex 关键)
- 路径整合(path integration)+ 地标校正

---

## 4. PyTorch — Place 由 Grid 组合

```python
import torch

def place_from_grids(pos, grid_scales, grid_phases):
    """Multiple grid modules (Fourier-like basis) -> unique place code."""
    code = []
    for scale, phase in zip(grid_scales, grid_phases):
        # Hexagonal grid firing as sum of 3 cosines
        g = sum(torch.cos(2*torch.pi/scale * (pos @ d) + phase)
                for d in torch.tensor([[1.,0.],[0.5,0.87],[-0.5,0.87]]))
        code.append(torch.relu(g))
    return torch.stack(code)   # combined -> place-cell-like uniqueness
```

---

## 5. 超越空间 — 概念地图

- 同一海马-内嗅机制编码**抽象关系空间**
- Constantinescu 2016:概念学习出现 grid-like 信号(fMRI)
- "Cognitive map of knowledge"(Behrens 2018):社会、价值、任务结构
- 统一:海马 = 关系记忆 + 推理 + 泛化引擎

---

## 6. 与记忆 / 推理

- 认知地图支持**推断未经历的关系**(transitive inference、抄近路)
- "Schema"+ 快速整合(Tse 2007)
- 想象 / 规划 = 在地图上"模拟轨迹"(见 [Mental_Imagery](Mental_Imagery.md))
- 与 [Memory_Systems](Memory_Systems.md) episodic 紧密

---

## 7. 临床

- **Alzheimer**:内嗅 / 海马早损 → 空间迷失早 symptom
- **海马损伤**(如 H.M.):空间 + 关系记忆缺
- **Topographical disorientation**:RSC / parahippocampal 损
- **Aging**:grid 漂移 → 导航退化

---

## 8. 与 AI

- RNN 训练导航 → emerge grid-like(Banino 2018,见 [Grid Cells](../05_Computational_Neuroscience/Grid_Cells.md))
- "World model" ↔ 认知地图(关系结构表征)
- Toluene Eichenbaum:relational memory ↔ graph / GNN
- Transformer 位置编码 ↔ grid-like basis(类比研究)

---

## 9. 计算视角

- 度量(grid)+ 拓扑(graph)双重
- Successor representation(Stachenfeld 2017):place 编码预测性后继 → RL 桥接
- 认知地图 = 可泛化的关系/预测结构(非纯几何)

---

## 10. Common Pitfalls

### 10.1 认知地图 = 字面地图

是结构化关系表征,含拓扑 + 预测,非比例尺地图。

### 10.2 仅编码物理空间

同机制泛化到概念 / 社会 / 价值空间。

### 10.3 Place cell = GPS 坐标

是上下文依赖、重映射(remapping)、预测性(SR)。

### 10.4 Tolman 已被行为主义否定

后被 place/grid cell 强力证实。

### 10.5 Grid = Place

不同区域 + 性质;grid 组合 → place(多对一 basis)。

---

## 11. Related Concepts

- **同节**:[Memory_Systems](Memory_Systems.md)、[Mental_Imagery](Mental_Imagery.md)、[Reasoning_Problem_Solving](Reasoning_Problem_Solving.md)
- **计算**:[Grid Cells](../05_Computational_Neuroscience/Grid_Cells.md)、[Reinforcement Learning Brain](../05_Computational_Neuroscience/Reinforcement_Learning_Brain.md)
- **系统**:[Hippocampus_Memory](../03_Systems_Neuroscience/Hippocampus_Memory.md)
- **AI**: world model、GNN、relational reasoning

---

## References

1. **Tolman, E. C.** "Cognitive maps in rats and men." *Psychol Rev*, 1948.
2. **O'Keefe, J. & Nadel, L.** *The Hippocampus as a Cognitive Map*. 1978.
3. **Behrens, T. E. J. et al.** "What is a cognitive map? Organizing knowledge for flexible behavior." *Neuron*, 2018.
4. **Stachenfeld, K. L. et al.** "The hippocampus as a predictive map." *Nat Neurosci*, 2017.
