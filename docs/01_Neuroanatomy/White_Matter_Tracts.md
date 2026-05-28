# 白质纤维束 (White Matter Tracts)

> *白质 = 髓鞘化轴突束(髓鞘脂质 → 白),~ 半脑体积。三类:联络纤维(同半球区间,如弓状束)、连合纤维(两半球,如胼胝体)、投射纤维(皮层↔皮层下,如内囊/皮质脊髓束)。"Disconnection syndrome"(Geschwind)= 束损断连。DTI 可活体重建(见 [DTI Tractography](../07_Neurotech_Frontiers/DTI_Tractography.md))。*
>
> **难度**:Intermediate
> **前置知识**:[Cortex](Cortex.md)、[Myelination](../02_Cellular_Molecular/Myelination.md)

---

## 1. 三类纤维

| 类 | 连接 | 例 |
|---|---|---|
| **联络**(association) | 同半球皮层区间 | 弓状束、上/下纵束、钩束、扣带束 |
| **连合**(commissural) | 两半球 | 胼胝体(最大)、前连合 |
| **投射**(projection) | 皮层 ↔ 皮层下/脊髓 | 内囊、皮质脊髓束、丘脑辐射 |

---

## 2. 关键束

- **弓状束**(arcuate):Broca↔Wernicke(语言;损 → 传导性失语)
- **胼胝体**(corpus callosum):~ 2 亿轴突;裂脑(split-brain)研究(Sperry Nobel)
- **内囊**(internal capsule):投射纤维"瓶颈"(小卒中 → 大缺损)
- **皮质脊髓束**:随意运动(锥体束)
- **钩束**(uncinate):眶额↔前颞(情绪-记忆)

---

## 3. PyTorch — 断连综合征(图)

```python
tracts = {
 "Broca": ["arcuate->Wernicke"],
 "Wernicke": ["arcuate->Broca"],
}
def disconnect(tract_cut):
    # Cutting arcuate: comprehension OK, production OK, but repetition fails
    if tract_cut == "arcuate":
        return "conduction aphasia: poor repetition, intact comprehension"
    return "depends on tract"
```

---

## 4. 投射纤维 — 内囊

- 前肢/膝/后肢:不同纤维(额桥、皮质核、皮质脊髓、丘脑辐射)
- 高度聚集 → 小腔隙梗死致"纯运动卒中"(见 [Cerebral_Vasculature](Cerebral_Vasculature.md))
- 解释为何小损伤大缺损

---

## 5. 胼胝体 + 裂脑

- 切断治顽固癫痫 → "裂脑人"
- Sperry/Gazzaniga:两半球可独立处理(左语言、右空间)
- "Alien hand"、左视野命名障碍(信息不能跨胼胝体)
- 见 [Cortex](Cortex.md)(偏侧化)

---

## 6. Disconnection Syndromes(Geschwind)

- 症状源于**束损**(连接断)而非皮层区本身损
- 传导性失语(弓状束)、失读不伴失写(胼胝体压部)、失用
- 1965 Geschwind 复兴 → 现代连接组学先声(见 [Connectomics](../00_Foundations/Connectomics.md))

---

## 7. 活体成像

- **DTI tractography**:水扩散各向异性 → 重建束(mm 级,见 [DTI Tractography](../07_Neurotech_Frontiers/DTI_Tractography.md))
- FA↓ = 白质完整性下降(非特异)
- 术前规划(避开弓状束/皮质脊髓束)

---

## 8. 病理

- **MS**:多灶脱髓鞘(见 [Multiple_Sclerosis](../08_Neuro_Disorders/Multiple_Sclerosis.md))
- **白质病(leukoaraiosis)**:血管性(老化/高血压)
- **TBI 弥漫性轴索损伤(DAI)**:剪切力断束(见 [Traumatic_Brain_Injury](../08_Neuro_Disorders/Traumatic_Brain_Injury.md))
- **遗传性脑白质营养不良**
- 精神病作为"connectopathy"(束异常)

---

## 9. 与 AI

- 白质 = 长程"布线" ↔ 网络连接拓扑 / 通信带宽
- Disconnection ↔ 模块间通信切断的功能后果(ablation 研究类比)
- 弓状束 ↔ encoder-decoder 间通路
- 见 [Connectomics](../00_Foundations/Connectomics.md)(macro vs micro)

---

## 10. Common Pitfalls

### 10.1 白质"不计算"

布线决定信息路由 + 时序;损伤致 disconnection 症状。

### 10.2 症状 = 灰质损

可能纯束损(传导性失语 = 弓状束,皮层完好)。

### 10.3 DTI 束 = 真轴突束

mm 级统计推断,假阳性(见 [DTI Tractography](../07_Neurotech_Frontiers/DTI_Tractography.md))。

### 10.4 小白质损 = 小缺损

内囊小腔隙 → 大运动缺损(纤维聚集)。

### 10.5 裂脑人两个意识

通常统一(右半球有限语言 + 行为整合);过度解读慎。

---

## 11. Related Concepts

- **同节**:[Cortex](Cortex.md)、[Cerebral_Vasculature](Cerebral_Vasculature.md)、[Spinal_Cord_Anatomy](Spinal_Cord_Anatomy.md)
- **细胞**:[Myelination](../02_Cellular_Molecular/Myelination.md)
- **基础**:[Connectomics](../00_Foundations/Connectomics.md)
- **前沿**:[DTI Tractography](../07_Neurotech_Frontiers/DTI_Tractography.md)
- **认知**:[Language](../04_Cognitive_Neuroscience/Language.md)

---

## References

1. **Geschwind, N.** "Disconnexion syndromes in animals and man." *Brain*, 1965.
2. **Catani, M. & Thiebaut de Schotten, M.** *Atlas of Human Brain Connections*. Oxford, 2012.
3. **Gazzaniga, M. S.** "Cerebral specialization and interhemispheric communication." *Brain*, 2000.
4. **Standring, S.** *Gray's Anatomy*. 42nd ed., 2020.
