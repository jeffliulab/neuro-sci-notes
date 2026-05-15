# 比较神经科学 (Comparative Neuroscience)

> *比较神经科学跨物种研究 nervous system,揭示保守原理 + 物种特化。模式生物各有优势:C. elegans(完整 connectome)、Drosophila(遗传)、zebrafish(透明)、mouse(哺乳遗传)、macaque(灵长)、octopus(独立演化的智能)。鸟类无 6 层皮层却高智能 → 挑战"皮层中心"观。*
>
> **难度**:Intermediate
> **前置知识**:[Evolution of Nervous Systems](Evolution_of_Nervous_Systems.md)

---

## 1. 为何比较

- 找**保守原理**(跨物种通用 → 基本机制)
- 找**特化**(物种独特 → 适应)
- 模式生物**可操作性**(基因、connectome)
- 演化视角理解人脑

---

## 2. 模式生物对照

| 生物 | neuron | 优势 | 局限 |
|---|---|---|---|
| C. elegans | 302 | 完整 connectome、透明、基因 | 太简单(无行为复杂性) |
| Drosophila | ~ 140k | 遗传工具箱、connectome | 无哺乳同源结构 |
| Zebrafish | ~ 10⁷ | 透明 → 全脑 imaging、发育 | 非哺乳 |
| Mouse | ~ 7×10⁷ | 哺乳、转基因、行为 | ≠ 人(PFC 小) |
| Macaque | ~ 6×10⁹ | 接近人、电生理 | 伦理、成本 |
| Octopus | ~ 5×10⁸ | 独立演化智能 | 无脊椎、难基因操作 |
| Songbird | varies | 发声学习(类语言) | |

---

## 3. 保守的 building blocks

- Ion channel(Nav、Kv)线虫到人高度同源
- Neurotransmitter(glutamate、GABA、DA)普遍
- Synaptic 机制(SNARE、receptor)保守
- 基本 circuit motif(feedforward、recurrent、lateral inhibition)

---

## 4. 物种特化

- **回声定位**:蝙蝠 / 海豚 auditory 特化
- **电感知**:电鱼 electroreception
- **磁感**:候鸟 magnetoreception
- **Star-nosed mole**:触觉极致
- **Octopus**:分布式智能(腕足半自主)

---

## 5. 鸟类智能(挑战皮层中心)

- 乌鸦、鹦鹉:工具使用、计数、规划
- 鸟无 6 层 neocortex,用 **DVR / nidopallium**
- 但实现类似 high cognition
- → 智能不必需哺乳式皮层架构(convergent evolution)

---

## 6. Octopus — 独立演化智能

- 软体动物,与脊椎动物 ~ 6 亿年前分支
- 2/3 neuron 在腕足(分布式)
- 解决问题、伪装、玩耍
- 智能的**另一种实现方案**

---

## 7. PyTorch — 跨物种 motif 比较

```python
import torch

# Conserved motif: lateral inhibition (worm → human)
def lateral_inhibition(x, inhib_strength=0.5):
    """Winner-take-all-ish — found across species."""
    total = x.sum()
    return torch.relu(x - inhib_strength * (total - x))

# Same computational motif, different substrate (C.elegans vs cortex)
```

---

## 8. 同源 vs 同功

- **Homology**: 共同祖先(哺乳 cortex)
- **Analogy / convergence**: 独立演化相似功能(鸟 vs 哺乳认知;章鱼 vs 脊椎)
- 区分对理解"智能的必要条件"关键

---

## 9. 转化价值

- Mouse model → 人类疾病(但 translation gap)
- C. elegans → 衰老、神经退行基础机制
- Zebrafish → 高通量药筛
- Aplysia(海兔)→ Kandel 学习记忆分子(Nobel 2000)

---

## 10. Common Pitfalls

### 10.1 Mouse = 小人

PFC、认知差异大;translation failure 常见。

### 10.2 智能需哺乳皮层

鸟、章鱼反例 — convergent evolution。

### 10.3 简单生物无价值

C. elegans / Aplysia 贡献基础机制(Nobel 级)。

### 10.4 同源 = 同功

需区分 homology vs convergence。

### 10.5 神经元越多越聪明

架构 + 连接 + 密度更重要;非纯数量。

---

## 11. Related Concepts

- **同节**:[Evolution of Nervous Systems](Evolution_of_Nervous_Systems.md)、[Connectomics](Connectomics.md)、[Research Methods](Research_Methods.md)
- **细胞**:[Ion Channels](../02_Cellular_Molecular/Ion_Channels.md)、[Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.md)

---

## References

1. **Striedter, G. F.** *Principles of Brain Evolution*. Sinauer, 2005.
2. **Güntürkün, O. & Bugnyar, T.** "Cognition without cortex." *Trends Cogn Sci*, 2016.
3. **Godfrey-Smith, P.** *Other Minds: The Octopus and the Evolution of Intelligent Life*. 2016.
4. **Kandel, E. R.** "The molecular biology of memory storage." *Science*, 2001.
