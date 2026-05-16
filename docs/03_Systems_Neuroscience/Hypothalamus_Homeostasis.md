# 下丘脑与稳态 (Hypothalamus & Homeostasis)

> *下丘脑(< 1% 脑重)是稳态总调度:体温、摄食、饮水、生殖、应激、昼夜、睡眠。机制:set point + 负反馈 + 神经-内分泌(垂体)+ 自主输出。Allostasis(预测性调节)修正经典稳态观。瘦素/饥饿肽 → 肥胖;HPA 轴 → 应激。是生存的"恒温器"。*
>
> **难度**:Intermediate
> **前置知识**:[Autonomic_Nervous_System](Autonomic_Nervous_System.md)、[Brainstem](../01_Neuroanatomy/Brainstem.md)

---

## 1. 核团 → 功能(选）

| 核 | 功能 |
|---|---|
| 视上核/室旁核(SON/PVN) | ADH、催产素;CRH(应激) |
| 视交叉上核(SCN) | 昼夜主钟(见 [Circadian](Circadian_System.md)) |
| 弓状核(ARC) | 摄食(瘦素/饥饿肽感受) |
| 腹内侧(VMH) | "饱中枢" |
| 外侧下丘脑(LH) | "摄食/觉醒"(orexin) |
| 视前区(POA) | 体温、睡眠 |
| 乳头体 | 记忆(Papez) |

---

## 2. 三大输出通路

1. **自主**:→ 脑干/脊髓 → 交感/副交感(见 [ANS](Autonomic_Nervous_System.md))
2. **内分泌**:→ 垂体
   - 后叶:直接释 ADH/催产素
   - 前叶:释放/抑制激素(CRH→ACTH、GnRH、TRH、GHRH...)
3. **行为**:→ 边缘/皮层 → 动机行为(觅食/饮水)

---

## 3. 稳态 = 负反馈 + Set Point

```
扰动 → 感受器(渗透/温度/瘦素/血糖)
   → 下丘脑比较 set point
   → 误差 → 自主 + 内分泌 + 行为校正
   → 反馈
```

例:体温 ↑ → POA → 出汗 + 血管舒张 + 行为(脱衣)。

---

## 4. PyTorch — 稳态控制器

```python
import torch

def homeostatic_controller(state, set_point, gain=0.5, T=100):
    """Negative feedback toward set point (e.g., body temperature)."""
    traj = []
    for _ in range(T):
        error = set_point - state
        correction = gain * error            # autonomic + behavioral output
        state = state + correction + 0.05*torch.randn(1).item()  # + disturbance
        traj.append(state)
    return traj   # converges to set_point
```

---

## 5. 摄食调控

- **瘦素(leptin)**:脂肪 → ARC → 抑食(长期能量储备信号);肥胖 = 瘦素抵抗
- **饥饿肽(ghrelin)**:胃 → 促食(餐前↑)
- **胰岛素、PYY、GLP-1**:饱足
- AgRP/POMC 神经元(ARC)拮抗调节
- GLP-1 激动剂(司美格鲁肽)= 减重药突破

---

## 6. Allostasis(预测性调节)

- 经典稳态:被动回 set point
- **Allostasis**(Sterling):**预测性**调节,set point 可变 + 提前应对
- "Allostatic load":慢性预测调节损耗 → 疾病
- 与主动推断 / 内感受相通(见 [Interoception](Interoception.md))

---

## 7. HPA 轴(应激）

- 应激 → PVN CRH → 垂体 ACTH → 肾上腺皮质醇
- 皮质醇负反馈(海马/PFC → 抑 HPA)
- 慢性应激 → HPA 失调 → 抑郁/代谢/免疫(见 [Depression](../08_Neuro_Disorders/Depression.md))

---

## 8. 病理

- **肥胖 / 厌食**:摄食回路失衡
- **尿崩症**:ADH 缺(下丘脑/垂体)
- **下丘脑性闭经**:GnRH 抑(应激/低能量)
- **Prader-Willi**:贪食(下丘脑)
- **Kleine-Levin**、下丘脑肿瘤 → 睡眠/摄食/体温紊乱
- 发热 = set point 上调(前列腺素,非稳态失败)

---

## 9. 与 AI

- 稳态 ↔ 控制论(Wiener)、PID(见 eng-notes 控制)
- **Homeostatic RL**:内部需求作 reward 源(drive-reduction → 现代 Keramati)
- Allostasis ↔ model-predictive control
- 具身 agent 的"生存目标"=稳态(见 [Embodied_Cognition](../04_Cognitive_Neuroscience/Embodied_Cognition.md))

---

## 10. Common Pitfalls

### 10.1 下丘脑小 → 不重要

< 1% 脑重但调控全部生存功能。

### 10.2 稳态 = 固定 set point

Allostasis:set point 可变 + 预测性。

### 10.3 发热 = 体温调节失败

是 set point 主动上调(免疫策略),非失控。

### 10.4 瘦素多 → 不胖

肥胖常瘦素**抵抗**(高瘦素仍贪食)。

### 10.5 一核一功能

核团功能重叠 + 分布;非严格一一对应。

---

## 11. Related Concepts

- **同节**:[Autonomic_Nervous_System](Autonomic_Nervous_System.md)、[Interoception](Interoception.md)、[Circadian_System](Circadian_System.md)、[Sleep_Wake](Sleep_Wake.md)
- **解剖**:[Brainstem](../01_Neuroanatomy/Brainstem.md)
- **疾病**:[Depression](../08_Neuro_Disorders/Depression.md)(HPA)
- **计算**:[Reinforcement Learning Brain](../05_Computational_Neuroscience/Reinforcement_Learning_Brain.md)(homeostatic RL)

---

## References

1. **Saper, C. B. & Lowell, B. B.** "The hypothalamus." *Curr Biol*, 2014.
2. **Sterling, P.** "Allostasis: a model of predictive regulation." *Physiol Behav*, 2012.
3. **Friedman, J. M.** "Leptin and the endocrine control of energy balance." *Nat Metab*, 2019.
4. **McEwen, B. S.** "Stress, adaptation, and disease: allostasis and allostatic load." *Ann NY Acad Sci*, 1998.
