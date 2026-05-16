# 脑血管 (Cerebral Vasculature)

> *脑 ~2% 体重耗 ~15-20% 心输出(无能储备 → 缺血即损,见 [Brain Energy Metabolism](../00_Foundations/Brain_Energy_Metabolism.md))。前(颈内动脉)+ 后(椎-基底)循环经 Willis 环吻合。卒中按血管区(ACA/MCA/PCA)有特征综合征。静脉经硬膜窦回流。脑血管解剖 = 卒中定位基石。*
>
> **难度**:Intermediate
> **前置知识**:[Cortex](Cortex.md)、[Stroke](../08_Neuro_Disorders/Stroke.md)

---

## 1. 双供血系统

| 系统 | 来源 | 供区 |
|---|---|---|
| **前循环** | 颈内动脉(ICA) | 大脑半球前 2/3、眼 |
| **后循环** | 椎动脉 → 基底动脉 | 脑干、小脑、枕叶、丘脑后 |

→ Willis 环(基底)吻合连接,提供侧支冗余。

---

## 2. Willis 环

```
   前交通 (ACoA)
  /            \
ACA           ACA
 |             |
ICA ─ MCA   MCA ─ ICA
 |             |
后交通(PCoA) 后交通
  \            /
   PCA  ─基底─ PCA
        椎动脉 ×2
```

完整环 < 50% 人群(变异多)→ 侧支代偿差异 → 卒中后果不同。

---

## 3. 三大脑动脉供区

| 动脉 | 供区 | 闭塞综合征 |
|---|---|---|
| **ACA** | 内侧额顶(下肢运动/感觉) | 对侧下肢无力 > 上肢 |
| **MCA** | 外侧面大部(上肢/面、语言区) | 对侧上肢/面瘫、失语(左)、忽视(右) |
| **PCA** | 枕叶 + 丘脑后 | 对侧偏盲、视觉失认 |

腔隙(穿支)→ 内囊/基底节(纯运动/感觉卒中)。

---

## 4. PyTorch — 血管区 → 缺失定位

```python
vascular_syndromes = {
 "ACA": "contralateral leg weakness > arm",
 "MCA": "contralateral arm/face palsy, aphasia(L)/neglect(R)",
 "PCA": "contralateral hemianopia, visual agnosia",
 "lenticulostriate": "pure motor (internal capsule lacune)",
 "basilar": "locked-in / coma (brainstem)",
 "PICA": "Wallenberg (lateral medullary)",
}
def localize(occluded): return vascular_syndromes.get(occluded, "?")
```

---

## 5. 自动调节 (Autoregulation)

- 脑血流(CBF)在 MAP ~ 60-150 mmHg 恒定(肌源 + 代谢 + 神经)
- 超范围 → 灌注压依赖(低 → 缺血;高 → 水肿/出血)
- 高血压/卒中后曲线右移
- 神经血管耦合 = 局部活动 ↑CBF(fMRI 基础,见 [fMRI BOLD](../07_Neurotech_Frontiers/fMRI_BOLD.md))

---

## 6. 静脉回流

- 皮层静脉 → 硬膜窦(上矢状窦等)→ 横窦 → 乙状窦 → 颈内静脉
- 深部 → 大脑大静脉(Galen)→ 直窦
- **桥静脉**断 → 硬膜下血肿(见 [Meninges_Ventricles](Meninges_Ventricles.md))
- **脑静脉窦血栓(CVST)**:头痛 + 癫痫 + 局灶(易漏诊)

---

## 7. 分水岭区 (Watershed)

- 相邻动脉供区交界(ACA/MCA、MCA/PCA)
- 全脑低灌注(心脏停搏/严重低血压)→ 分水岭梗死("man in a barrel")
- 终末血管 → 缺血脆弱

---

## 8. 临床

- **缺血卒中**(~85%):血栓/栓塞 → tPA/取栓(时间窗,见 [Stroke](../08_Neuro_Disorders/Stroke.md))
- **出血卒中**(~15%):高血压(基底节)、动脉瘤(SAH)、AVM、淀粉样
- **TIA**:短暂可逆(卒中预警)
- **Moyamoya**、血管炎、夹层(青年卒中)
- 影像:CTA/MRA/DSA(血管),CT/DWI(实质)

---

## 9. 与 AI / 工程

- 双供血 + Willis 环 = 冗余 + 侧支(容错网络设计类比)
- 自动调节 = 闭环稳压控制(见 eng-notes 控制)
- 血管区定位 = 故障节点反推(类拓扑诊断)
- 神经血管耦合 → fMRI 信号物理基础

---

## 10. Common Pitfalls

### 10.1 Willis 环人人完整

< 50% 完整;变异决定侧支代偿 + 卒中后果。

### 10.2 脑有能储备

无;血流停数分钟即不可逆损(缺血核心)。

### 10.3 卒中症状随机

按血管区高度定型(ACA/MCA/PCA 综合征)。

### 10.4 静脉问题罕见/不重要

CVST 易漏诊但可致命/致残。

### 10.5 自动调节无限

仅 ~60-150 mmHg;超范围灌注压依赖(高血压右移)。

---

## 11. Related Concepts

- **同节**:[Cortex](Cortex.md)、[Brainstem](Brainstem.md)、[Meninges_Ventricles](Meninges_Ventricles.md)
- **基础**:[Brain Energy Metabolism](../00_Foundations/Brain_Energy_Metabolism.md)、[Blood Brain Barrier](../00_Foundations/Blood_Brain_Barrier.md)
- **疾病**:[Stroke](../08_Neuro_Disorders/Stroke.md)
- **前沿**:[fMRI BOLD](../07_Neurotech_Frontiers/fMRI_BOLD.md)

---

## References

1. **Blumenfeld, H.** *Neuroanatomy through Clinical Cases*. 2nd ed., 2010.
2. **Caplan, L. R.** *Caplan's Stroke: A Clinical Approach*. 5th ed., 2016.
3. **Cipolla, M. J.** *The Cerebral Circulation*. Morgan & Claypool, 2009.
4. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
