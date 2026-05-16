# 脑膜与脑室系统 (Meninges & Ventricular System)

> *脑膜三层(硬膜 dura、蛛网膜 arachnoid、软膜 pia)保护 CNS + 形成腔隙。脑室系统(2 侧脑室 + 3 + 4 脑室)产生/容纳 CSF。出血按层分(硬膜外/下、蛛网膜下)。脑疝综合征是神经急症。功能见 [CSF_Glymphatic](../00_Foundations/CSF_Glymphatic.md)。*
>
> **难度**:Intermediate
> **前置知识**:[Nervous System Overview](../00_Foundations/Nervous_System_Overview.md)

---

## 1. 脑膜三层

| 层 | 特征 |
|---|---|
| **Dura mater**(硬膜) | 厚、双层(骨膜+脑膜层)、形成镰/幕 + 静脉窦 |
| **Arachnoid**(蛛网膜) | 蛛网膜下腔(含 CSF)、蛛网膜颗粒(CSF 回吸) |
| **Pia mater**(软膜) | 紧贴脑表、随沟回、血管伴行 |

潜在腔:硬膜外(脑:中脑膜动脉)、硬膜下(桥静脉)、蛛网膜下(Willis 环)。

---

## 2. 脑室系统

```
侧脑室 (×2, 含脉络丛)
   ↓ Foramen of Monro
第三脑室 (丘脑间)
   ↓ Cerebral aqueduct (中脑, 最窄)
第四脑室 (脑桥/延髓背)
   ↓ Foramina (Magendie 中, Luschka ×2 侧)
蛛网膜下腔 → 蛛网膜颗粒 → 静脉窦
```

CSF 由脉络丛产(~ 500 mL/d),循环见 [CSF_Glymphatic](../00_Foundations/CSF_Glymphatic.md)。

---

## 3. PyTorch — CSF 流路图(图遍历)

```python
csf_path = {
 "lateral_ventricles": "foramen_Monro",
 "foramen_Monro": "third_ventricle",
 "third_ventricle": "cerebral_aqueduct",
 "cerebral_aqueduct": "fourth_ventricle",
 "fourth_ventricle": "subarachnoid_space",
 "subarachnoid_space": "arachnoid_granulations(venous)"
}
def trace(node):
    seq=[node]
    while node in csf_path: node=csf_path[node]; seq.append(node)
    return seq   # blockage anywhere -> hydrocephalus upstream
```

---

## 4. 出血分层(临床关键)

| 类型 | 来源 | 影像/特征 |
|---|---|---|
| **硬膜外**(epidural) | 中脑膜动脉(颞骨折) | 双凸透镜、lucid interval |
| **硬膜下**(subdural) | 桥静脉(老人/酗酒/外伤) | 新月形、亚急/慢性 |
| **蛛网膜下**(SAH) | 动脉瘤破(Willis 环) | "雷击样"头痛、CT 沟内高密度 |
| **脑实质内** | 高血压/淀粉样 | — |

---

## 5. 硬膜反折 + 静脉窦

- 大脑镰(纵裂)、小脑幕(分隔大/小脑)
- 反折内含**硬膜静脉窦**(上矢状窦等)→ 脑静脉回流 + CSF 经蛛网膜颗粒回吸
- 幕切迹 = 钩回疝通道(CN III 受压)

---

## 6. 脑积水 (Hydrocephalus)

- **梗阻性**(non-communicating):导水管/孔阻 → 上游脑室扩
- **交通性**:吸收障碍(SAH 后、脑膜炎)
- **NPH(正常压力)**:步态 + 认知 + 尿失禁三联(可逆性痴呆!)
- 治:分流术(VP shunt)、第三脑室造瘘

---

## 7. 脑疝综合征(神经急症)

- **钩回疝**(uncal):颞叶内移压 CN III(瞳孔散大)+ 中脑
- **中心疝**:间脑下移
- **扁桃体疝**:小脑扁桃体入枕骨大孔 → 压延髓(致死,呼吸停)
- Monro-Kellie:ICP↑ → 疝(见 [Traumatic_Brain_Injury](../08_Neuro_Disorders/Traumatic_Brain_Injury.md))

---

## 8. 临床操作

- **腰穿(LP)**:L3-L5 蛛网膜下取 CSF(脊髓终于 L1-2 → 安全);测压 + 化验(见 [CSF_Glymphatic](../00_Foundations/CSF_Glymphatic.md))
- **脑膜炎**:脑膜炎症(颈强、Kernig/Brudzinski)
- **脑室外引流(EVD)**:急性脑积水/ICP 监测

---

## 9. 与 AI / 工程

- 脑室-CSF = 流体缓冲 + 清除"管路系统"↔ 散热/流体循环工程类比
- 出血分层 = 故障定位(按解剖层)
- 与 [CSF_Glymphatic](../00_Foundations/CSF_Glymphatic.md) 清除功能

---

## 10. Common Pitfalls

### 10.1 脑膜仅"包膜"

形成腔隙 + 静脉窦 + 镰幕(疝通道)+ CSF 回吸,功能丰富。

### 10.2 硬膜外/下 = 一回事

来源(动脉 vs 桥静脉)、形态(双凸 vs 新月)、时程不同。

### 10.3 脑室越大病越重

NPH 脑室大但可逆;需结合临床(萎缩 vs 积水)。

### 10.4 腰穿伤脊髓

脊髓终 L1-2;L3-5 穿仅马尾(可避)。

### 10.5 脑疝非急症

扁桃体疝压延髓致死 — 最高级急症。

---

## 11. Related Concepts

- **同节**:[Cortex](Cortex.md)、[Brainstem](Brainstem.md)、[Cerebral_Vasculature](Cerebral_Vasculature.md)
- **基础**:[CSF_Glymphatic](../00_Foundations/CSF_Glymphatic.md)、[Blood Brain Barrier](../00_Foundations/Blood_Brain_Barrier.md)
- **疾病**:[Stroke](../08_Neuro_Disorders/Stroke.md)、[Traumatic_Brain_Injury](../08_Neuro_Disorders/Traumatic_Brain_Injury.md)

---

## References

1. **Standring, S.** *Gray's Anatomy*. 42nd ed., 2020.
2. **Blumenfeld, H.** *Neuroanatomy through Clinical Cases*. 2nd ed., 2010.
3. **Sakka, L. et al.** "Anatomy and physiology of cerebrospinal fluid." *Eur Ann Otorhinolaryngol*, 2011.
4. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
