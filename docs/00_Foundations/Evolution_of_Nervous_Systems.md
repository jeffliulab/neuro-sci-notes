# 神经系统演化 (Evolution of Nervous Systems)

> *从 sponge (无神经) → cnidarian nerve net → bilateral CNS → vertebrate brain → human cortex。演化无"目标",是约束下的修补(Jacob "tinkering")。理解演化解释了为何 brain 设计常非最优(blind spot、绕行的喉返神经)。比较神经科学揭示保守的 building blocks。*
>
> **难度**:Intermediate
> **前置知识**:[Nervous System Overview](Nervous_System_Overview.md)、演化生物基础

---

## 1. 演化序列

| 阶段 | 代表 | 神经组织 |
|---|---|---|
| 无神经 | 海绵 (Porifera) | 无 neuron |
| Nerve net | 水母 (Cnidaria) | 弥散网,无中枢 |
| Cephalization | 扁虫 (Platyhelminthes) | 头神经节 + 神经索 |
| Ganglia | 节肢/软体 | 分节神经节 |
| Tubular CNS | 脊索动物 | dorsal nerve cord |
| Vertebrate brain | 鱼→哺乳 | 5 vesicle 脑 |
| Neocortex 扩张 | 灵长/人 | 6 层皮层巨增 |

---

## 2. Nerve Net (Cnidaria)

- 最早 neuron(~ 6 亿年前)
- 无中枢,弥散
- 但已有 synapse、neurotransmitter
- 说明 neuron building blocks 极古老 + 保守

---

## 3. Cephalization

- 双侧对称 → 头端集中(感官 + 神经节)
- 运动方向性驱动(前进时前端先遇环境)
- Bilateria 共同祖先 "urbilaterian"

---

## 4. 脊椎动物脑

- 3 vesicle → 5 vesicle(见 [Neurodevelopment](Neurodevelopment.md))
- 鱼 → 两栖 → 爬行 → 鸟/哺乳
- Telencephalon(端脑)逐步扩张
- 哺乳:neocortex 出现(6 层)

---

## 5. Neocortex 扩张

- 啮齿 → 灵长 → 人:neocortex 比例剧增
- 人 ~ 16 billion cortical neuron
- Folding(gyrification)增表面积
- 但与体型 allometry:人 brain 比预期大 ~ 7×(encephalization quotient)

---

## 6. 保守 vs 创新

- **保守**:ion channel、NT、突触机制(线虫到人高度同源)
- **创新**:新区域(neocortex)、新 cell type、scaling
- 演化 = 在旧 toolkit 上 tinkering(Jacob 1977)

---

## 7. 演化"缺陷"(非最优证据)

- **盲点**:脊椎动物视网膜倒装(光感受器在后)
- **喉返神经**:绕主动脉(长颈鹿尤甚)
- **背痛**:四足→直立未充分重设计
- → 演化无前瞻,只能改良现有

---

## 8. PyTorch — 演化 vs 设计 类比

```python
import torch

# "Evolution": mutate + select on existing structure (no redesign)
def evolutionary_step(weights, fitness_fn, mutation=0.05):
    candidate = weights + torch.randn_like(weights) * mutation
    if fitness_fn(candidate) > fitness_fn(weights):
        return candidate  # kept (incremental, path-dependent)
    return weights

# vs "Design": global optimization (gradient) — brain CANNOT do this
```

→ 演化像 local hill-climbing,非全局重设计 → 解释 suboptimal traits。

---

## 9. 模式生物(比较)

| 生物 | neuron 数 | 用途 |
|---|---|---|
| C. elegans | 302 | 完整 connectome |
| Drosophila | ~ 140,000 | 遗传 + connectome |
| Zebrafish | ~ 10⁷ | 透明,全脑 imaging |
| Mouse | ~ 7×10⁷ | 哺乳模型主力 |
| Macaque | ~ 6×10⁹ | 灵长,接近人 |
| Human | ~ 8.6×10¹⁰ | 目标 |

---

## 10. AI 类比

- 神经演化 ↔ neural architecture search / evolutionary algorithms
- "Tinkering" ↔ transfer learning(在 pretrained 上改)
- Brain 非最优 ↔ NN 也常 local minima
- 但 AI 可全局重设计,演化不可

---

## 11. Common Pitfalls

### 11.1 演化有方向/目标

无;只是 differential survival,无"进步"内涵。

### 11.2 复杂 = 高等

水母 nerve net 极成功(5 亿年);"高低等"是误导。

### 11.3 人脑是演化巅峰

只是一支;鸟类用不同架构(无 6 层皮层)实现高智能。

### 11.4 大脑越大越聪明

EQ(相对体型)+ 神经密度更相关;鲸脑大于人。

### 11.5 演化产物最优

大量 suboptimal(盲点、喉返神经)— 路径依赖。

---

## 12. Related Concepts

- **同节**:[Nervous System Overview](Nervous_System_Overview.md)、[Neurodevelopment](Neurodevelopment.md)、[Research Methods](Research_Methods.md)
- **细胞**:[Neuron](../02_Cellular_Molecular/Neuron.md)、[Ion Channels](../02_Cellular_Molecular/Ion_Channels.md)

---

## References

1. **Jacob, F.** "Evolution and tinkering." *Science*, 1977.
2. **Striedter, G. F.** *Principles of Brain Evolution*. Sinauer, 2005.
3. **Bullock, T. H. & Horridge, G. A.** *Structure and Function in the Nervous Systems of Invertebrates*. 1965.
4. **Herculano-Houzel, S.** *The Human Advantage*. MIT Press, 2016.
