# 焦虑障碍 (Anxiety Disorders)

> *焦虑障碍是最常见精神障碍(终生 ~ 30%)。含 GAD、panic disorder、social anxiety、specific phobia、agoraphobia。核心环路:amygdala 过度反应 + PFC 调控不足。SSRI/SNRI + CBT 一线。是适应性恐惧系统的失调,非单纯"想太多"。*
>
> **难度**:Intermediate
> **前置知识**:[Amygdala](../01_Neuroanatomy/Amygdala.md)、[Emotion](../04_Cognitive_Neuroscience/Emotion.md)

---

## 1. 分类 (DSM-5)

| 类型 | 特征 |
|---|---|
| GAD | 持续广泛担忧(≥ 6 月) |
| Panic disorder | 反复 panic attack + 预期焦虑 |
| Social anxiety | 怕被评判 |
| Specific phobia | 特定对象(蛇、高、针) |
| Agoraphobia | 怕难逃离场所 |
| Separation anxiety | 分离恐惧 |

(OCD、PTSD 在 DSM-5 已独立分类)

---

## 2. 流行病学

- 终生 prevalence ~ 30%(最高一类)
- 女 : 男 ~ 2 : 1
- 起病早(儿童-青年)
- 高共病:抑郁、物质滥用

---

## 3. 神经环路

```
威胁 → Amygdala (过度激活)
          ↑ 调控不足
        vmPFC / dlPFC
          
Amygdala → 下丘脑 (HPA, cortisol)
         → 脑干 (心率↑, 呼吸↑)
         → BNST (持续焦虑 vs 急性恐惧)
```

- Amygdala hyperreactivity + PFC top-down 调控弱
- BNST:持续 / 不确定威胁(与 phobia 急性 fear 区分)
- Insula:内感受 → panic

---

## 4. 神经递质

- **GABA**:抑制不足 → benzodiazepine(GABA-A 正变构)起效
- **5-HT**:SSRI 长期调节
- **NE**:locus coeruleus 过度 → panic
- **Glutamate**、CRF、neuropeptide Y

---

## 5. Fear vs Anxiety

| | Fear | Anxiety |
|---|---|---|
| 触发 | 明确 / 即时威胁 | 不确定 / 未来 |
| 环路 | central amygdala | BNST(extended amygdala) |
| 时程 | 相位性(快) | 持续性 |
| 行为 | 战 / 逃 | 警觉 / 回避 |

---

## 6. PyTorch — Amygdala-PFC 调控失衡

```python
import torch

def anxiety_circuit(threat, pfc_control=1.0, T=100):
    """High amygdala gain + weak PFC regulation -> sustained arousal."""
    amy = 0.0
    arousal = []
    for t in range(T):
        drive = threat - pfc_control * amy        # PFC inhibits amygdala
        amy = torch.relu(torch.tensor(amy + 0.1 * drive))
        arousal.append(amy.item())
    return arousal   # weak pfc_control -> runaway anxiety

# anxiety disorder: pfc_control low -> amygdala unchecked
```

---

## 7. 治疗

### 7.1 心理(一线)

- **CBT**:认知重构 + exposure(extinction learning — 见 [Amygdala](../01_Neuroanatomy/Amygdala.md))
- Exposure 是 phobia/panic 最有效

### 7.2 药物

- **SSRI / SNRI**:一线(起效 2-6 周)
- **Benzodiazepines**:急用,依赖风险 → 不长期
- **Buspirone**(5-HT1A)、**β-blocker**(表演焦虑躯体症状)
- **Pregabalin**(GAD)

### 7.3 实验

- D-cycloserine 增强 exposure(NMDA 部分激动)
- 经颅刺激、digital CBT

---

## 8. 学习视角

- Phobia/panic = 异常 fear conditioning + extinction 失败
- Exposure = 新 inhibitory learning(不抹除旧记忆)
- 回避维持焦虑(阻止 extinction)
- 与 RL / extinction 计算模型相关

---

## 9. 适应 vs 病理

- 焦虑本是适应性(预警 + 准备)
- 病理 = 过度 / 失调 / 与威胁不成比例 / 损功能
- "Smoke detector principle"(Nesse):假阳性代价 < 假阴性

---

## 10. Common Pitfalls

### 10.1 = 想太多 / 性格弱

是恐惧系统失调的医学状况,非性格缺陷。

### 10.2 Benzo 是好长期方案

耐受 + 依赖 + 认知损;仅短期 / 急用。

### 10.3 回避能缓解

短期缓解,长期维持 + 恶化(阻 extinction)。

### 10.4 Fear = Anxiety

环路 + 时程不同(amygdala vs BNST)。

### 10.5 SSRI 立即起效

抗焦虑需 2-6 周;早期可短暂加重。

---

## 11. Related Concepts

- **同节**:[Depression](Depression.md)、[Bipolar_Disorder](Bipolar_Disorder.md)
- **解剖**:[Amygdala](../01_Neuroanatomy/Amygdala.md)
- **认知**:[Emotion](../04_Cognitive_Neuroscience/Emotion.md)
- **细胞**:[Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.md)(GABA、5-HT)

---

## References

1. **Craske, M. G. et al.** "Anxiety disorders." *Nat Rev Dis Primers*, 2017.
2. **Davis, M. et al.** "Phasic vs sustained fear in rats and humans: role of the extended amygdala in fear vs anxiety." *Neuropsychopharmacology*, 2010.
3. **Nesse, R. M.** "The smoke detector principle." *Ann NY Acad Sci*, 2001.
4. **Bystritsky, A. et al.** "Current diagnosis and treatment of anxiety disorders." *P&T*, 2013.
