# 扣带回 (Cingulate Cortex)

> *扣带回沿胼胝体环绕,边缘系统核心。前扣带(ACC):冲突监测、错误、痛"难受度"、动机。后扣带(PCC):DMN 枢纽、自我参照。膝下 ACC(sgACC,Brodmann 25)= 难治抑郁 DBS 靶。Papez 环路成员。功能跨认知-情绪-自主。*
>
> **难度**:Intermediate
> **前置知识**:[Cortex](Cortex.md)、[Limbic_System](../03_Systems_Neuroscience/Limbic_System.md)

---

## 1. 分区

| 区 | 功能 |
|---|---|
| **dACC**(背侧前扣带) | 冲突监测、错误检测、认知控制、痛 affect |
| **sgACC/BA25**(膝下) | 情绪、抑郁(DBS 靶) |
| **PCC**(后扣带) | DMN、自我参照、记忆提取 |
| **RSC**(压后) | 空间导航、egocentric↔allocentric |

---

## 2. ACC — 冲突/错误

- **Error-Related Negativity (ERN)**:错误后 ~ 50-100 ms 头皮电位(ACC 源)
- 冲突监测理论(Botvinick):dACC 检测反应冲突 → 调用 dlPFC 控制
- 痛的"难受度"(affective)在 ACC(感觉强度在 S1)
- 动机/努力分配

---

## 3. PyTorch — 冲突监测 → 控制调用

```python
import torch

def acc_conflict_control(resp_a, resp_b, dlpfc_gain=1.0):
    """dACC detects co-active competing responses -> recruit dlPFC control."""
    conflict = resp_a * resp_b                       # both active = conflict
    control_signal = dlpfc_gain * torch.sigmoid(conflict - 0.3)
    # Next trial: heightened control reduces conflict (adaptation)
    return conflict, control_signal
```

---

## 4. PCC + 默认网络

- PCC/precuneus = DMN 核心枢纽(高代谢)
- 自我参照、自传记忆、心智漫游(见 [Self_and_Agency](../04_Cognitive_Neuroscience/Self_and_Agency.md))
- 任务时抑制;DMN-任务网络反相关
- 麻醉/意识下降 → PCC 活动/连接变(意识 marker)

---

## 5. sgACC(BA25)— 抑郁 DBS

- 难治抑郁:sgACC 过度活跃/连接异常
- Mayberg DBS 靶点(白质束)→ 部分缓解(试验结果不一)
- 抗抑郁/CBT 后 sgACC 正常化
- 见 [Depression](../08_Neuro_Disorders/Depression.md)、[DBS](../07_Neurotech_Frontiers/DBS.md)

---

## 6. 连接 + 整合

- ACC 连 PFC + 边缘 + 自主(下丘脑/脑干)+ 运动
- "认知-情绪-自主"接口
- Von Economo 神经元(大、快投射,人/猿/鲸;ACC + 前岛)— bvFTD 选择性易损(见 [Frontotemporal_Dementia](../08_Neuro_Disorders/Frontotemporal_Dementia.md))

---

## 7. 痛的双维度

- **感觉-辨别**(在哪/多强):S1/S2
- **情绪-动机**(多难受):ACC + 岛(见 [Pain_System](../03_Systems_Neuroscience/Pain_System.md))
- 扣带切开术(cingulotomy)→ 减痛"难受"而非感觉(历史 + 难治痛)

---

## 8. 病理

- **抑郁**:sgACC 异常
- **OCD**:ACC 过度(CSTC,见 [OCD](../08_Neuro_Disorders/OCD.md))
- **疼痛慢性化**:ACC 可塑
- **意识障碍**:PCC 标志
- **bvFTD**:ACC von Economo 损

---

## 9. 与 AI

- 冲突监测 → 控制调用 ↔ 不确定性触发额外计算 / "think more"(类 adaptive computation)
- 错误信号(ERN)↔ 内部 error monitor / 自评估
- DMN ↔ "offline" 模拟 / 默认生成(见 [Creativity](../04_Cognitive_Neuroscience/Creativity.md))

---

## 10. Common Pitfalls

### 10.1 扣带回单一功能

ACC vs PCC vs sgACC vs RSC 功能迥异(分区)。

### 10.2 ACC = 痛中枢

编码痛**情绪**维度,非感觉强度(S1)。

### 10.3 DMN = 无功能空闲

self/记忆/模拟活跃网络。

### 10.4 sgACC DBS 确定有效

试验结果不一(个体化靶点 + 白质束关键)。

### 10.5 ERN 仅"犯错"

也反映冲突 / 反馈 / 强化学习信号。

---

## 11. Related Concepts

- **同节**:[Cortex](Cortex.md)、[Prefrontal_Cortex](Prefrontal_Cortex.md)、[Amygdala](Amygdala.md)
- **系统**:[Limbic_System](../03_Systems_Neuroscience/Limbic_System.md)、[Pain_System](../03_Systems_Neuroscience/Pain_System.md)
- **认知**:[Decision_Making](../04_Cognitive_Neuroscience/Decision_Making.md)、[Self_and_Agency](../04_Cognitive_Neuroscience/Self_and_Agency.md)
- **疾病**:[Depression](../08_Neuro_Disorders/Depression.md)、[OCD](../08_Neuro_Disorders/OCD.md)

---

## References

1. **Botvinick, M. M. et al.** "Conflict monitoring and cognitive control." *Psychol Rev*, 2001.
2. **Shackman, A. J. et al.** "The integration of negative affect, pain and cognitive control in the cingulate cortex." *Nat Rev Neurosci*, 2011.
3. **Mayberg, H. S. et al.** "Deep brain stimulation for treatment-resistant depression." *Neuron*, 2005.
4. **Vogt, B. A.** "Pain and emotion interactions in subregions of the cingulate gyrus." *Nat Rev Neurosci*, 2005.
