# 下丘脑解剖 (Hypothalamus Anatomy)

> *下丘脑 < 4 g(脑 ~0.3%),位于丘脑下方、三脑室壁。~ 一打核团分前/中/后区 + 内/外侧。控垂体(神经-内分泌)、自主、稳态。本文聚焦**解剖结构**;功能见 [Hypothalamus_Homeostasis](../03_Systems_Neuroscience/Hypothalamus_Homeostasis.md)。是边缘-内分泌-自主交汇枢纽。*
>
> **难度**:Intermediate
> **前置知识**:[Brainstem](Brainstem.md)、[Thalamus](Thalamus.md)

---

## 1. 位置 + 边界

- 丘脑下方、三脑室两侧壁 + 底
- 前界:终板/视交叉;后界:乳头体/中脑;下:垂体柄/正中隆起
- 视交叉、漏斗、乳头体 = 表面标志
- 极小但连接极广

---

## 2. 分区(前后)

| 区 | 主要核 |
|---|---|
| **视前/前区** | 视上(SON)、室旁(PVN)、视交叉上(SCN)、视前(POA) |
| **结节(中)区** | 弓状(ARC)、腹内侧(VMH)、背内侧(DMH)、外侧下丘脑(LH) |
| **乳头体(后)区** | 乳头体、后核 |

内外侧:外侧下丘脑(LH,内侧前脑束通过)vs 内侧核团。

---

## 3. 与垂体连接

| 通路 | 机制 |
|---|---|
| **下丘脑-垂体后叶束** | SON/PVN 大细胞 → 轴突直达后叶,释 ADH/催产素 |
| **下丘脑-垂体门脉** | 小细胞 → 正中隆起释放/抑制激素 → 门脉血 → 前叶 |

→ 神经分泌(Scharrer)经典发现。

---

## 4. PyTorch — 核团-功能映射(查表)

```python
hypothalamic_nuclei = {
    "SCN":  "circadian master clock",
    "SON":  "ADH / oxytocin (magnocellular)",
    "PVN":  "CRH (stress), ADH/OXT, autonomic",
    "ARC":  "feeding (leptin/ghrelin sensing)",
    "VMH":  "satiety, defensive behavior",
    "LH":   "feeding/arousal (orexin)",
    "POA":  "thermoregulation, sleep, sexual",
    "Mammillary": "memory (Papez)",
}
def lookup(nucleus): return hypothalamic_nuclei.get(nucleus, "unknown")
```

---

## 5. 内侧前脑束 (MFB)

- 纵贯外侧下丘脑的大纤维束
- 双向:边缘 ↔ 脑干调质核(DA/NE/5-HT)
- 自我刺激(Olds & Milner)经典 reward 通路(见 [Reward_System](../03_Systems_Neuroscience/Reward_System.md))
- DBS 抑郁靶之一

---

## 6. 血供 + 特殊性

- Willis 环分支(穿支动脉)
- **正中隆起 + 部分核**无 BBB(circumventricular)→ 感血液激素(见 [Blood Brain Barrier](../00_Foundations/Blood_Brain_Barrier.md))
- 高度血管化(内分泌器官特性)

---

## 7. 临床(解剖定位)

- **垂体瘤**压视交叉 → 双颞侧偏盲
- **颅咽管瘤**(儿童)→ 内分泌 + 视觉 + 下丘脑综合征
- **下丘脑错构瘤**→ 痴笑性癫痫 + 性早熟
- **Wernicke-Korsakoff**:乳头体出血/萎缩(硫胺素缺,见 [Limbic_System](../03_Systems_Neuroscience/Limbic_System.md))
- 下丘脑损 → 体温/摄食/睡眠/内分泌紊乱

---

## 8. 与 AI / 工程

- 微小但连接枢纽 ↔ 高 fan-in/out 的控制节点
- 神经-内分泌 = 慢全局广播(激素)↔ 全局标量调质
- circumventricular "采样口" ↔ 系统的传感接口
- 见 [Hypothalamus_Homeostasis](../03_Systems_Neuroscience/Hypothalamus_Homeostasis.md)(功能/计算)

---

## 9. 演化保守

- 下丘脑核团高度保守(鱼到人)— 生存核心
- 神经分泌细胞 = 古老(无脊椎已有类似)
- 见 [Evolution of Nervous Systems](../00_Foundations/Evolution_of_Nervous_Systems.md)

---

## 10. Common Pitfalls

### 10.1 小 = 不重要

~0.3% 脑重却控生存全部(垂体/自主/稳态)。

### 10.2 = 丘脑

不同结构:丘脑=中继;下丘脑=内分泌/自主/稳态。

### 10.3 一核一功能

核团功能重叠 + 分布;映射是简化。

### 10.4 全有 BBB

正中隆起等 circumventricular 无 BBB(故意)。

### 10.5 仅"本能"

也参记忆(乳头体)、情绪、节律。

---

## 11. Related Concepts

- **同节**:[Brainstem](Brainstem.md)、[Thalamus](Thalamus.md)、[Amygdala](Amygdala.md)
- **系统**:[Hypothalamus_Homeostasis](../03_Systems_Neuroscience/Hypothalamus_Homeostasis.md)、[Autonomic_Nervous_System](../03_Systems_Neuroscience/Autonomic_Nervous_System.md)、[Circadian_System](../03_Systems_Neuroscience/Circadian_System.md)
- **基础**:[Blood Brain Barrier](../00_Foundations/Blood_Brain_Barrier.md)

---

## References

1. **Saper, C. B. & Lowell, B. B.** "The hypothalamus." *Curr Biol*, 2014.
2. **Swaab, D. F.** *The Human Hypothalamus*. Handbook of Clinical Neurology, 2003.
3. **Scharrer, E. & Scharrer, B.** "Neurosecretion." *Physiol Rev*, 1945.
4. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
