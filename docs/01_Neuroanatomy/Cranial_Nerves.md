# 颅神经 (Cranial Nerves)

> *12 对颅神经直接从脑/脑干发出(vs 脊神经)。混合感觉/运动/副交感。记忆口诀经典。临床定位价值极高(每对有特征体征)。CN II(视)+ I(嗅)是 CNS 延伸;迷走(X)是副交感主力。是神经系统检查基石。*
>
> **难度**:Intermediate
> **前置知识**:[Brainstem](Brainstem.md)、[Nervous System Overview](../00_Foundations/Nervous_System_Overview.md)

---

## 1. 十二对一览

| # | 名 | 类型 | 主功能 |
|---|---|---|---|
| I | 嗅 | 感 | 嗅觉(见 [Olfactory_System](../03_Systems_Neuroscience/Olfactory_System.md)) |
| II | 视 | 感 | 视觉(CNS 束) |
| III | 动眼 | 运+副交感 | 多数眼肌、瞳孔缩、提睑 |
| IV | 滑车 | 运 | 上斜肌(眼下内) |
| V | 三叉 | 混 | 面感觉 + 咀嚼肌 |
| VI | 外展 | 运 | 外直肌(眼外展) |
| VII | 面 | 混 | 面表情肌 + 味(前2/3)+ 泪/涎 |
| VIII | 前庭蜗 | 感 | 听觉 + 平衡(见 [Vestibular_System](../03_Systems_Neuroscience/Vestibular_System.md)) |
| IX | 舌咽 | 混 | 咽感 + 味(后1/3)+ 腮腺 |
| X | 迷走 | 混 | 内脏副交感主力 + 咽喉 |
| XI | 副 | 运 | 胸锁乳突/斜方肌 |
| XII | 舌下 | 运 | 舌肌 |

---

## 2. 类型分类

- 纯感觉:I、II、VIII
- 纯运动:III(主)、IV、VI、XI、XII
- 混合:V、VII、IX、X
- 含副交感:III、VII、IX、X("1973" 规则)

---

## 3. 脑发出部位

- I、II:前脑(端/间脑)— 实为 CNS 束
- III、IV:中脑
- V、VI、VII、VIII:脑桥
- IX、X、XI、XII:延髓
- → 颅神经核定位 = 脑干病变定位关键(见 [Brainstem](Brainstem.md))

---

## 4. PyTorch — 颅神经查表(临床定位)

```python
cranial_nerves = {
 1:("Olfactory","sensory","smell"), 2:("Optic","sensory","vision"),
 3:("Oculomotor","motor+PSNS","most eye muscles, pupil"),
 4:("Trochlear","motor","superior oblique"),
 5:("Trigeminal","mixed","face sensation, mastication"),
 6:("Abducens","motor","lateral rectus"),
 7:("Facial","mixed","facial expression, taste ant2/3"),
 8:("Vestibulocochlear","sensory","hearing+balance"),
 9:("Glossopharyngeal","mixed","pharynx, taste post1/3"),
 10:("Vagus","mixed","parasympathetic viscera"),
 11:("Accessory","motor","SCM/trapezius"),
 12:("Hypoglossal","motor","tongue"),
}
def localize(cn): return cranial_nerves.get(cn)
```

---

## 5. 经典临床体征

- **III 麻痹**:眼"down and out"、上睑下垂、瞳孔散大(钩回疝急症!)
- **VII 麻痹**:Bell 麻痹(周围:全侧面;中枢:仅下面 — 额肌双侧支配)
- **V**:三叉神经痛(剧烈面痛)
- **X**:声嘶、吞咽困难、悬雍垂偏
- **瞳孔对光反射**:II(传入)+ III(传出)
- **角膜反射**:V(传入)+ VII(传出)

---

## 6. 反射弧(颅神经)

- 对光:CN II → 中脑 → CN III(双侧)
- 角膜:CN V1 → CN VII
- 呕吐:CN IX → CN X
- 颈动脉窦(压力):CN IX → 延髓 → CN X
- → 反射检查精确定位脑干水平

---

## 7. 迷走神经(X)— 副交感主力

- 支配心/肺/胃肠(降率、促消化)
- 80% 传入(内脏感觉 → NTS,见 [Interoception](../03_Systems_Neuroscience/Interoception.md))
- VNS(迷走刺激)治癫痫/抑郁(见 [Closed_Loop_Neuromodulation](../07_Neurotech_Frontiers/Closed_Loop_Neuromodulation.md))

---

## 8. 与 AI / 工程

- 颅神经 = 脑的专用 I/O 端口(传感器 + 执行器接口)
- 定位诊断 = 故障节点反推(类硬件 fault localization)
- 反射弧 = 低延迟硬连线控制回路

---

## 9. 发育 + 演化

- 鳃弓衍生(V/VII/IX/X 与古鳃弓对应)
- 颅神经核排列保留分节(rhombomere)印记
- 见 [Evolution of Nervous Systems](../00_Foundations/Evolution_of_Nervous_Systems.md)

---

## 10. Common Pitfalls

### 10.1 全是"神经"

I、II 实为 CNS 束(髓鞘少突胶质,非施万)。

### 10.2 中枢 = 周围 VII 麻痹

中枢仅下面(额肌双侧皮层支配);周围全侧。

### 10.3 迷走主运动

80% 是**传入**(内脏感觉)。

### 10.4 颅神经只在头

迷走达腹腔(广泛内脏)。

### 10.5 瞳孔反射单侧

正常双侧(consensual);单侧异常定位 II vs III。

---

## 11. Related Concepts

- **同节**:[Brainstem](Brainstem.md)、[Thalamus](Thalamus.md)
- **基础**:[Nervous System Overview](../00_Foundations/Nervous_System_Overview.md)
- **系统**:[Vestibular_System](../03_Systems_Neuroscience/Vestibular_System.md)、[Olfactory_System](../03_Systems_Neuroscience/Olfactory_System.md)、[Autonomic_Nervous_System](../03_Systems_Neuroscience/Autonomic_Nervous_System.md)

---

## References

1. **Wilson-Pauwels, L. et al.** *Cranial Nerves: Function and Dysfunction*. 3rd ed., 2010.
2. **Blumenfeld, H.** *Neuroanatomy through Clinical Cases*. 2nd ed., 2010.
3. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
4. **Standring, S.** *Gray's Anatomy*. 42nd ed., 2020.
