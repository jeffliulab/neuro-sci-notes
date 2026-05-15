# 创伤后应激障碍 (PTSD)

> *PTSD 是创伤事件后持续的 re-experiencing + avoidance + hyperarousal + 负性认知。机制:过强 fear memory consolidation + extinction 失败 + amygdala 过度 + hippocampus/vmPFC 调控不足。Trauma-focused CBT / EMDR 一线;MDMA-assisted therapy 突破性试验。*
>
> **难度**:Intermediate
> **前置知识**:[Amygdala](../01_Neuroanatomy/Amygdala.md)、[Hippocampus_Memory](../03_Systems_Neuroscience/Hippocampus_Memory.md)

---

## 1. 临床 (DSM-5)

创伤暴露 + 4 症状簇(> 1 月):
1. **Intrusion**:闪回、噩梦、侵入记忆
2. **Avoidance**:回避相关线索
3. **Negative cognition/mood**:负性信念、麻木、解离
4. **Arousal/reactivity**:过度警觉、惊跳、易怒、睡眠差

---

## 2. 流行病学

- 终生 prevalence ~ 6-8%(经历创伤者 ~ 10-20% 发展 PTSD)
- 女 > 男
- 风险:创伤严重度、既往创伤、缺社会支持、遗传
- 多数创伤经历者**不**发 PTSD(韧性常态)

---

## 3. 神经基础

```
Amygdala 过度 (fear)
  ↑ vmPFC 调控不足 (extinction 失败)
Hippocampus ↓ (情境化差 → 泛化)
```

- **Amygdala** hyperreactivity
- **vmPFC** 抑制 / extinction 不足
- **Hippocampus** 体积↓ + 功能↓ → context discrimination 差 → 过度泛化
- HPA axis 异常(cortisol 模式特殊 — 常偏低 + 高敏)

---

## 4. 记忆视角

- 创伤记忆 = 过强 consolidation(NE/cortisol 增强 amygdala 编码)
- Extinction 学习失败(vmPFC-amygdala)
- **Reconsolidation**:回忆使记忆短暂可塑 → 治疗窗口
- 闪回 = involuntary、感官、无时间标签(hippocampus 弱)

---

## 5. PyTorch — 过强 fear consolidation

```python
import torch

def trauma_consolidation(stress_hormone=1.0, baseline=0.3):
    """Cortisol/NE amplify amygdala memory encoding strength."""
    encoding_strength = baseline * (1 + 2.0 * stress_hormone)
    extinction_rate = 0.5 / (1 + stress_hormone)   # high stress -> poor extinction
    return encoding_strength, extinction_rate

# PTSD: high stress_hormone -> over-strong memory + weak extinction
```

---

## 6. 治疗

### 6.1 心理(一线)

- **Trauma-focused CBT**:prolonged exposure、cognitive processing therapy
- **EMDR**(eye movement desensitization)— 有效,机制争议
- 核心:extinction + 记忆重整

### 6.2 药物

- **SSRI / SNRI**(sertraline、paroxetine FDA 批)
- **Prazosin**(α1 拮抗)→ 改善噩梦
- Benzo **禁忌**(恶化 + 阻 extinction)

### 6.3 突破性

- **MDMA-assisted therapy**(Phase 3 阳性,降低 amygdala fear + 增治疗联盟)
- **Propranolol** + 回忆 → 阻 reconsolidation(结果不一)
- Ketamine、cannabinoid 研究中

---

## 7. 韧性 (Resilience)

- 多数人经历创伤不发 PTSD
- 保护:社会支持、认知灵活、NPY、安全学习
- 是常态非例外 → 研究 resilience 与研究病理同重要

---

## 8. 共病 + 后果

- 抑郁、物质滥用、慢性痛
- 自杀风险↑
- 躯体健康(心血管、炎症)
- Complex PTSD(长期反复创伤 → 加情绪调节 + 自我 + 关系障碍)

---

## 9. 争议

- EMDR 眼动是否必要(vs 仅 exposure)
- 创伤记忆"压抑/恢复"争议(false memory 风险)
- 诊断扩张
- 心理 debriefing(事后即时)可能有害

---

## 10. Common Pitfalls

### 10.1 创伤必致 PTSD

多数有韧性;PTSD 是少数。

### 10.2 Benzo 有助

恶化 + 阻 extinction → 禁忌。

### 10.3 闪回 = 普通回忆

是 involuntary、感官化、缺时间标签(hippocampus 弱)。

### 10.4 必须详述创伤才好

强制 debriefing 可有害;治疗需结构 + 时机。

### 10.5 MDMA 是娱乐用法

治疗用是受控 + 配心理治疗,机制不同于滥用。

---

## 11. Related Concepts

- **同节**:[Anxiety_Disorders](Anxiety_Disorders.md)、[Depression](Depression.md)
- **解剖**:[Amygdala](../01_Neuroanatomy/Amygdala.md)
- **系统**:[Hippocampus_Memory](../03_Systems_Neuroscience/Hippocampus_Memory.md)、[Sleep_Wake](../03_Systems_Neuroscience/Sleep_Wake.md)

---

## References

1. **Yehuda, R. et al.** "Post-traumatic stress disorder." *Nat Rev Dis Primers*, 2015.
2. **Milad, M. R. & Quirk, G. J.** "Fear extinction as a model for translational neuroscience." *Annu Rev Psychol*, 2012.
3. **Mitchell, J. M. et al.** "MDMA-assisted therapy for severe PTSD (Phase 3)." *Nat Med*, 2021.
4. **Pitman, R. K. et al.** "Biological studies of post-traumatic stress disorder." *Nat Rev Neurosci*, 2012.
