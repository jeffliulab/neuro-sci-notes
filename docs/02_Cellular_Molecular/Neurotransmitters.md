# 神经递质 — Glutamate, GABA, DA, 5-HT, ACh 等

> *神经递质 (NT) 是突触上传递信号的化学分子。脑内 100+ 种 NT,主要 7-10 种主导大部分功能。本篇覆盖**主要 NT 类型**、receptor 机制、与药物 / 疾病的关系。*
>
> **难度**:Intermediate
> **前置知识**:[突触](Synapse.md)、[神经元](Neuron.md)
> **后续阅读**:[LTP/LTD](LTP_LTD.md)

---

## 1. 主要 NT 分类

### 1.1 氨基酸类

- **Glutamate (Glu)**: 主要**兴奋性** NT,~80% cortex synapses
- **GABA**: 主要**抑制性** NT
- **Glycine**: 抑制,主在脊髓 / 脑干

### 1.2 单胺类 (Monoamines)

- **Dopamine (DA)**: 奖赏、运动、注意
- **Norepinephrine (NE)**: 唤醒、注意、应激
- **Serotonin (5-HT)**: 情绪、睡眠、食欲
- **Histamine**: 唤醒、过敏

### 1.3 乙酰胆碱 (ACh)

- 神经肌肉接头
- 注意、学习

### 1.4 神经肽

- Endorphin, Enkephalin (内源性阿片)
- Substance P (痛感)
- Oxytocin (社交)
- ~50+ 种

### 1.5 气体

- NO (Nitric Oxide), CO — retrograde messenger

---

## 2. Receptor 类型

### 2.1 Ionotropic (离子型)

NT 结合 → channel 直接开 → 离子流。**快** (ms 级)。

- **AMPA receptor** (Glu): 快兴奋
- **NMDA receptor** (Glu): 慢, voltage + ligand 双门
- **GABA-A** (GABA): Cl- inflow, 抑制
- **Nicotinic ACh** (nAChR): cation

### 2.2 Metabotropic (代谢型)

NT 结合 → GPCR → 二级信使 → 慢调节 (秒-分钟级)。

- **GABA-B**: K+ outflow
- **mGluR** (Glu)
- **DA receptors D1-D5**
- **5-HT receptors 5-HT1-5-HT7**
- **Muscarinic ACh** (mAChR)

---

## 3. Glutamate 详细

最重要 NT:
- 80% cortex EPSPs 由 Glu 介导
- 受 AMPA + NMDA + mGluR
- **NMDA**: $V$ depolarized 且 Glu present 才开 (Mg²⁺ block 移除) → coincidence detector for LTP
- **过量 Glu = excitotoxicity** (stroke / ALS)

---

## 4. GABA 详细

主抑制:
- 30% 海马 + cortex synapse 抑制
- GABA-A receptor:Cl- channel,目标 anxiolytics, anesthetic
- 药物:
  - **Benzodiazepines** (Valium): GABA-A allosteric 增强
  - **Barbiturates**, **Alcohol**: 同
  - **Bicuculline**: GABA-A 阻断 (实验)

---

## 5. 多巴胺系统

```
VTA → NAcc / PFC (mesolimbic / mesocortical, 奖赏)
SNc → 纹状体 (nigrostriatal, 运动)
Hypothalamus → 垂体 (tuberoinfundibular)
```

### 5.1 功能

- Reward prediction error (Schultz 1997)
- 运动 (Parkinson DA 退化)
- 注意 (ADHD: DA 假说)

### 5.2 药物

- **Cocaine, 安非他命**: 阻 DA reuptake
- **Antipsychotics** (Haloperidol): D2 拮抗
- **L-DOPA**: DA 前体 (Parkinson 治疗)

---

## 6. 5-HT 系统

```
脑干中缝核 → 全脑
```

### 6.1 功能

- 情绪 (depression 假说)
- 睡眠
- 食欲、性

### 6.2 药物

- **SSRI** (Prozac, Zoloft): 5-HT reuptake 抑制 → 抗抑郁
- **LSD, psilocybin**: 5-HT2A 激动 → 幻觉

---

## 7. ACh 系统

### 7.1 神经肌肉接头

每个运动神经元控制肌肉的 ACh → nAChR → 肌肉收缩。
箭毒 (curare) 阻 nAChR → 麻痹。

### 7.2 中枢

- Basal forebrain → cortex (注意, 学习)
- Brainstem → thalamus (唤醒)
- Alzheimer: ACh neuron 退化 → cholinesterase inhibitor 治疗

---

## 8. NT 生命周期

```
合成 (神经元 cytoplasm)
  ↓
装载 vesicle
  ↓
AP 到 → Ca²⁺ → 融合 → 释放
  ↓
扩散过 cleft
  ↓
结合 post receptor → 信号
  ↓
解离
  ↓
Reuptake (transporter) / 酶降解
```

例 ACh: 由 ChAT 合成 → vesicle → 释放 → AChE 降解为 choline + acetate → choline reuptake → 重合成。

---

## 9. 实验测量

- **HPLC**: NT 浓度
- **Microdialysis**: in vivo NT 测量
- **Optogenetics**: 控制 NT 释放
- **Fluorescent sensors** (GRAB-DA, iGluSnFR): 实时荧光 imaging

---

## 10. 药物作用机制总结

| 类别 | NT | 机制 |
|---|---|---|
| Antidepressant (SSRI) | 5-HT | reuptake 阻断 |
| Antipsychotic | DA | D2 拮抗 |
| Anxiolytic (Benzo) | GABA | GABA-A 增强 |
| Stimulant (Adderall) | DA, NE | reuptake 阻 / 释放促进 |
| Opioid | Endorphin | μ receptor 激动 |
| Anesthetic | GABA, NMDA | 多目标 |
| Hallucinogen | 5-HT | 5-HT2A 激动 |
| Cholinesterase inhibitor | ACh | AChE 抑制 (Alzheimer) |

---

## 11. Common Pitfalls

### 11.1 单 NT 错觉

许多神经元 release > 1 NT (co-transmission)。

### 11.2 Receptor specificity

Subtypes 性质迥异。"GABA receptor" 含多种,只说 receptor 不够具体。

### 11.3 Acute vs chronic

短期 vs 长期 NT 变化机制完全不同 (LTP / down-regulation / etc.)。

### 11.4 BBB

许多药物不过 blood-brain barrier。

### 11.5 Net effect ≠ NT-level

NT 总量 ≠ 功能。Receptor / circuit / 状态 都重要。

---

## 12. Related Concepts

- **同节**:[突触](Synapse.md)、[LTP/LTD](LTP_LTD.md)、[离子 channel](Ion_Channels.md)

---

## References

1. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
2. **Iversen, L. L. et al.** *Introduction to Neuropsychopharmacology*. Oxford, 2009.
3. **Schultz, W.** "A neural substrate of prediction and reward." *Science*, 1997.
4. **Squire, L. R. et al.** *Fundamental Neuroscience*. 4th ed., 2012.
