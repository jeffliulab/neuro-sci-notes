# 创伤性脑损伤 (Traumatic Brain Injury)

> *TBI 是外力致脑功能损伤,从 concussion(mild)到 severe。机制:primary injury(撞击瞬间)+ secondary injury(数小时-天的级联:兴奋毒性、炎症、缺血、水肿)。CTE(慢性创伤性脑病)与反复 TBI 关联。Glasgow Coma Scale 分级。"金时窗"防 secondary injury 是救治核心。*
>
> **难度**:Intermediate
> **前置知识**:[Stroke](Stroke.md)、[Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.md)

---

## 1. 分类

| 严重度 | GCS | 特征 |
|---|---|---|
| Mild (concussion) | 13-15 | 短暂意识/认知改变,影像常正常 |
| Moderate | 9-12 | 较长昏迷,影像异常 |
| Severe | 3-8 | 长昏迷,高死亡/残疾 |

类型:局灶(挫伤、血肿)vs 弥漫(DAI — 弥漫性轴索损伤)。

---

## 2. Primary vs Secondary Injury

```
Primary (撞击瞬间, 不可逆):
  挫伤、轴索剪切、血管撕裂
       ↓
Secondary (数小时-天, 可干预!):
  谷氨酸兴奋毒性 → Ca²⁺ 内流
  线粒体衰竭、氧化应激
  神经炎症 (小胶质活化)
  脑水肿 → ICP↑ → 缺血
  BBB 破坏
```

→ 救治窗 = 阻断 secondary cascade。

---

## 3. 兴奋毒性级联

- 机械损伤 → 大量 glutamate 释放
- NMDA 过度激活 → Ca²⁺ 内流
- → 蛋白酶 / 脂酶 / 线粒体损伤 → 神经元死亡
- 与 stroke 机制重叠(见 [Stroke](Stroke.md))

---

## 4. 颅内压 (ICP) 与 Monro-Kellie

- 颅腔固定容积 = 脑 + 血 + CSF
- 水肿 / 血肿 → ICP ↑ → 脑灌注压 ↓ → 缺血 → 恶性循环
- 严重 → 脑疝(致命)
- 管理:渗透治疗(甘露醇/高渗盐)、引流、去骨瓣减压

---

## 5. PyTorch — Monro-Kellie ICP

```python
import numpy as np

def icp_dynamics(edema_volume, baseline_icp=10):
    """Exponential pressure-volume curve (compliance exhausts)."""
    E = 0.15  # elastance coefficient
    icp = baseline_icp * np.exp(E * edema_volume)
    cpp = 80 - icp           # cerebral perfusion pressure (MAP~80)
    ischemia = cpp < 50      # critical threshold
    return icp, cpp, ischemia
```

---

## 6. Concussion (mTBI)

- 无结构损伤但功能紊乱(代谢、离子、轴索微损)
- "Neurometabolic cascade"(K⁺ 外流 → 能量危机)
- 多数 7-10 天恢复;少数 post-concussion syndrome 持续
- Second-impact syndrome:未愈再撞 → 灾难性脑肿胀

---

## 7. CTE (慢性创伤性脑病)

- 反复 TBI/亚震荡(拳击、橄榄球、军人)
- Tau 蛋白病变(血管周围 p-tau)
- 行为/认知/情绪退化(数年-数十年后)
- 仅尸检确诊(生前 biomarker 研究中)
- 体育界重大议题

---

## 8. 治疗

- **急性**:ABC + 防 secondary(控 ICP、氧合、血压、避免低血糖/高热)
- **手术**:血肿清除、去骨瓣减压
- **神经保护药**:大量试验**失败**(谷氨酸拮抗等)→ 临床仍支持性为主
- **康复**:认知 / 物理 / 职业(慢性期核心)

---

## 9. 长期后果

- 认知(注意、记忆、执行)、情绪(抑郁、易怒)、PTSD 共病
- 癫痫风险↑(post-traumatic epilepsy)
- 神经退行风险↑(AD、PD、CTE)
- 慢性 disability(尤年轻人)

---

## 10. Common Pitfalls

### 10.1 影像正常 = 没事

mTBI 常影像正常但功能紊乱;不可轻视。

### 10.2 Primary injury 可治

Primary 不可逆;治疗目标是 secondary cascade。

### 10.3 震荡后立即返赛安全

Second-impact syndrome 可致命;须分级返回。

### 10.4 神经保护药已成熟

数十项 III 期失败;目前无特效神经保护药。

### 10.5 CTE 可生前确诊

目前仅尸检;生前诊断仍研究阶段。

---

## 11. Related Concepts

- **同节**:[Stroke](Stroke.md)、[Epilepsy](Epilepsy.md)、[Alzheimer](Alzheimer.md)(tau)
- **细胞**:[Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.md)(glutamate 兴奋毒性)
- **基础**:[Blood Brain Barrier](../00_Foundations/Blood_Brain_Barrier.md)

---

## References

1. **Maas, A. I. R. et al.** "Traumatic brain injury: integrated approaches to improve prevention, clinical care, and research." *Lancet Neurol*, 2017.
2. **Giza, C. C. & Hovda, D. A.** "The new neurometabolic cascade of concussion." *Neurosurgery*, 2014.
3. **McKee, A. C. et al.** "The spectrum of disease in chronic traumatic encephalopathy." *Brain*, 2013.
4. **Jennett, B. & Teasdale, G.** "Assessment of coma and impaired consciousness (GCS)." *Lancet*, 1974.
