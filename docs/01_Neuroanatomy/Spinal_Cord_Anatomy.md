# 脊髓解剖 (Spinal Cord Anatomy)

> *脊髓:延髓延续至 L1-2(成人),31 节段。横断面:中央蝴蝶状灰质(背/侧/腹角)+ 外周白质(背/侧/腹索)。上行(感觉)+ 下行(运动)束有精确空间排布 → 损伤定位。本文聚焦**结构**;回路/CPG 见 [Spinal_Cord_Systems](../03_Systems_Neuroscience/Spinal_Cord_Systems.md)。*
>
> **难度**:Intermediate
> **前置知识**:[Brainstem](Brainstem.md)、[Nervous System Overview](../00_Foundations/Nervous_System_Overview.md)

---

## 1. 大体

- 上接延髓,下终 **圆锥**(conus,成人 L1-2)→ 终丝 + 马尾
- 31 节段:8C+12T+5L+5S+1Co
- 两膨大:颈膨大(上肢)、腰骶膨大(下肢)
- 包膜:脊膜(同脑膜三层)+ 硬膜外腔(脂肪/静脉丛)

---

## 2. 横断面

```
        背角(感觉传入)
       /  Rexed laminae I-X
背索 ─ 灰质(蝴蝶/H)─ 侧索
       \ 腹角(运动神经元 → 肌)
        中央管(室管膜, CSF)
外周:白质(髓鞘束)
```

灰质:背角(感觉)、中间外侧柱(T1-L2 交感)、腹角(α/γ 运动神经元,躯体定位排列)。

---

## 3. 主要束(空间排布)

| 束 | 位置 | 方向/功能 |
|---|---|---|
| 后柱(薄/楔束) | 背索 | 上行:精触/本体(同侧上行,延髓交叉) |
| 脊髓丘脑束 | 前外侧 | 上行:痛温(脊髓即交叉) |
| 皮质脊髓束(外侧) | 侧索 | 下行:随意运动(延髓已交叉) |
| 脊髓小脑束 | 侧索周 | 上行:本体 → 小脑 |

→ 排布精确 → 损伤模式定位(见 [Spinal_Cord_Systems](../03_Systems_Neuroscience/Spinal_Cord_Systems.md))。

---

## 4. PyTorch — 损伤平面定位

```python
def localize_lesion(deficit):
    rules = {
     "ipsilateral motor+proprioception, contralateral pain/temp": "Brown-Sequard (hemisection)",
     "bilateral pain/temp loss, motor spared (cape)": "central cord (syrinx)",
     "bilateral motor + pain/temp below, proprioception spared": "anterior cord",
     "all modalities below a level": "complete transection",
    }
    return rules.get(deficit, "see tract layout")
```

---

## 5. 脊神经 + 节段

- 每节段:背根(感觉,有背根节)+ 腹根(运动)→ 合成脊神经
- **皮节(dermatome)**:每节感觉皮区(临床定位带状疱疹/损伤)
- **肌节(myotome)**:每节支配肌群
- 神经丛:颈/臂/腰骶丛(重组)

---

## 6. 与椎骨错位

- 脊髓比椎管短(发育速度差)→ 节段 ≠ 同序椎骨(下段尤甚)
- 马尾:L2 以下仅神经根 → 腰穿安全区(见 [Meninges_Ventricles](Meninges_Ventricles.md))
- 椎间盘突出 → 压神经根(根性痛/无力)

---

## 7. 反射弧解剖

- 单突触(牵张):Ia → 腹角 α 运动元
- 多突触(屈肌回撤)
- 节段内 + 节段间(联络束)
- 见 [Spinal_Cord_Systems](../03_Systems_Neuroscience/Spinal_Cord_Systems.md)(功能)

---

## 8. 临床损伤综合征

| 损伤 | 解剖 | 表现 |
|---|---|---|
| 完全横断 | 全断 | 平面下全失 + 自主 |
| Brown-Séquard | 半切 | 同侧运动/本体,对侧痛温 |
| 前索 | 前 2/3 | 运动+痛温失,本体保 |
| 中央索(脊髓空洞) | 中央管周 | 节段性痛温("披肩") |
| 后索 | 背索 | 本体觉/振动失(感觉性共济) |
| ALS | 前角+皮质脊髓 | 上下运动神经元(见 [ALS](../08_Neuro_Disorders/ALS.md)) |

---

## 9. 与 AI / 工程

- 精确束排布 = 模块化布线 + 拓扑诊断(损伤模式 → 定位)
- 灰白质分层 = 计算(灰)vs 通信(白)分离
- 脊髓 = 脑的"外设总线 + 边缘控制器"

---

## 10. Common Pitfalls

### 10.1 脊髓延至骶骨

成人终于 L1-2;以下为马尾(腰穿安全基础)。

### 10.2 节段 = 同序椎骨

脊髓短于椎管;下段错位明显。

### 10.3 痛温与本体同侧上行

痛温脊髓即交叉;本体延髓才交叉(损伤模式关键)。

### 10.4 横断面束随机

精确空间排布 → 综合征高度定型。

### 10.5 脊髓仅传导(解剖上)

灰质含运动元/中间元/自主柱(计算 + 反射)。

---

## 11. Related Concepts

- **同节**:[Brainstem](Brainstem.md)、[White_Matter_Tracts](White_Matter_Tracts.md)、[Cerebral_Vasculature](Cerebral_Vasculature.md)
- **系统**:[Spinal_Cord_Systems](../03_Systems_Neuroscience/Spinal_Cord_Systems.md)、[Somatosensory](../03_Systems_Neuroscience/Somatosensory.md)、[Motor_System](../03_Systems_Neuroscience/Motor_System.md)
- **疾病**:[ALS](../08_Neuro_Disorders/ALS.md)

---

## References

1. **Standring, S.** *Gray's Anatomy*. 42nd ed., 2020.
2. **Blumenfeld, H.** *Neuroanatomy through Clinical Cases*. 2nd ed., 2010.
3. **Watson, C. et al.** *The Spinal Cord: A Christopher and Dana Reeve Foundation Text and Atlas*. 2009.
4. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
