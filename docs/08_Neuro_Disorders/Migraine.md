# 偏头痛 (Migraine)

> *偏头痛是常见 neurovascular disorder(~ 15% 人群),不只是"头疼"。机制:cortical spreading depression (CSD) + trigeminovascular 系统激活 + CGRP。四期:prodrome → aura → headache → postdrome。CGRP 单抗(2018+)是数十年来首个机制性预防突破。*
>
> **难度**:Intermediate
> **前置知识**:[Brainstem](../01_Neuroanatomy/Brainstem.md)、[Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.md)

---

## 1. 临床四期

```
Prodrome (数小时-天前): 哈欠、食欲变、情绪
   ↓
Aura (~ 20-60 min, ~ 1/3 患者): 视觉锯齿/暗点、感觉异常
   ↓
Headache (4-72 h): 单侧搏动性、畏光畏声、恶心
   ↓
Postdrome ("migraine hangover"): 疲惫、认知迟钝
```

---

## 2. 流行病学

- ~ 15% 全球(女 ~ 3× 男,激素相关)
- 致残:全球 disability 主因之一(青壮年)
- 分型:有先兆 / 无先兆 / 慢性偏头痛(≥ 15 天/月)
- 强遗传成分(家族性偏瘫型 — CACNA1A 等离子通道基因)

---

## 3. Cortical Spreading Depression (CSD)

- Leão 1944 发现
- 一波去极化以 ~ 3 mm/min 在皮层扩散,随后长时抑制
- 对应 **aura**(视觉锯齿移动速度匹配!)
- 触发 trigeminovascular 激活 → 痛

---

## 4. Trigeminovascular 系统

- 三叉神经支配脑膜血管
- 激活 → 释放 **CGRP**、substance P → 神经源性炎症 + 血管扩张
- 信号 → trigeminal nucleus → thalamus → cortex
- 中枢敏化 → allodynia(头皮触痛)

---

## 5. CGRP — 关键分子

- Calcitonin gene-related peptide:强血管扩张 + 痛信号
- 偏头痛发作时 CGRP ↑
- 注射 CGRP 可诱发偏头痛(患者)
- → 靶向 CGRP = 现代治疗革命

---

## 6. PyTorch — CSD 扩散波

```python
import numpy as np

def cortical_spreading_depression(N=100, T=200, D=0.1, speed=3.0):
    """1D reaction-diffusion wave (~3 mm/min) like aura."""
    u = np.zeros(N); u[0] = 1.0       # depolarization start
    history = []
    for t in range(T):
        lap = np.gradient(np.gradient(u))
        react = u * (1 - u) * (u - 0.2)   # bistable excitable
        u = np.clip(u + D * lap + 0.05 * react, 0, 1)
        history.append(u.copy())
    return np.array(history)   # traveling wave = aura percept
```

---

## 7. 治疗

### 7.1 急性

- **Triptans**(5-HT1B/1D 激动):sumatriptan 等 — 收缩血管 + 抑 CGRP 释放
- **Gepants**(CGRP 受体拮抗,口服):ubrogepant、rimegepant — 无血管收缩(心血管安全)
- **Ditans**(5-HT1F):lasmiditan
- NSAID、止吐药辅助

### 7.2 预防

- **CGRP mAb**(2018+):erenumab、fremanezumab、galcanezumab、eptinezumab — 突破性
- 传统:β-blocker、topiramate、amitriptyline、valproate
- **OnabotulinumtoxinA**:慢性偏头痛
- **Neuromodulation**:外周 / 经颅刺激设备

---

## 8. 触发因素

- 激素(月经期)、睡眠紊乱、压力(及压力后"放松性")
- 特定食物/酒、脱水、强光强味
- 天气变化
- 但"触发"个体差异大,部分实为 prodrome 误判

---

## 9. 与其他头痛区分

| | Migraine | Tension | Cluster |
|---|---|---|---|
| 部位 | 单侧搏动 | 双侧紧箍 | 单侧眶周剧痛 |
| 时长 | 4-72 h | 30 min-7 d | 15-180 min(成群) |
| 伴随 | 畏光声、恶心 | 少 | 流泪、鼻塞(自主) |
| 性别 | 女多 | — | 男多 |

---

## 10. Common Pitfalls

### 10.1 只是普通头疼

是 neurovascular 脑病,可严重致残 + 有先兆神经症状。

### 10.2 Aura = 血管缺血

现代:CSD(神经元波)为主,非单纯血管学说。

### 10.3 血管扩张是根因

血管学说过时;神经源性 + CGRP 中心。

### 10.4 Triptan 人人适用

血管收缩 → 心血管病慎用;gepant 更安全替代。

### 10.5 止痛药越多越好

过用 → "药物过用性头痛"(MOH)反恶化。

---

## 11. Related Concepts

- **同节**:[Epilepsy](Epilepsy.md)(CSD vs seizure)、[Stroke](Stroke.md)
- **解剖**:[Brainstem](../01_Neuroanatomy/Brainstem.md)
- **细胞**:[Ion Channels](../02_Cellular_Molecular/Ion_Channels.md)(CACNA1A)、[Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.md)

---

## References

1. **Goadsby, P. J. et al.** "Pathophysiology of migraine: a disorder of sensory processing." *Physiol Rev*, 2017.
2. **Leão, A. A. P.** "Spreading depression of activity in the cerebral cortex." *J Neurophysiol*, 1944.
3. **Edvinsson, L. et al.** "CGRP as the target of new migraine therapies." *Nat Rev Neurol*, 2018.
4. **Charles, A.** "The pathophysiology of migraine: implications for clinical management." *Lancet Neurol*, 2018.
