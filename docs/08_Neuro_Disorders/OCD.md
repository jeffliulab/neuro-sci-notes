# 强迫症 (Obsessive-Compulsive Disorder)

> *OCD 是 obsessions(侵入性想法)+ compulsions(重复行为以减焦虑)的慢性病,~ 2-3% prevalence。核心环路:cortico-striato-thalamo-cortical (CSTC) loop 过度活跃 — OFC/ACC + caudate + thalamus。SSRI(高剂量)+ ERP(暴露反应预防)一线;难治者 DBS。*
>
> **难度**:Intermediate
> **前置知识**:[Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.md)、[Anxiety_Disorders](Anxiety_Disorders.md)

---

## 1. 临床

- **Obsessions**:侵入、不想要、引焦虑(污染、伤害、对称、禁忌想法)
- **Compulsions**:重复行为/心理动作减焦虑(洗、检查、计数、排列)
- 自知力(多数知道不合理但难抗)
- 耗时(> 1 h/天)+ 损功能

---

## 2. 流行病学

- Prevalence ~ 2-3%
- 起病:青少年-成年早期(双峰:童年 + 20s)
- 男童起病早;成人性别均衡
- 共病:抑郁、tic/Tourette、焦虑

---

## 3. CSTC 环路模型

```
OFC / ACC (过度"出错"信号)
   ↓
Caudate / striatum
   ↓ direct vs indirect 失衡
Thalamus (gating 不足)
   ↓
回 cortex (强化 loop)
```

- "皮层-纹状体-丘脑-皮层" loop hyperactivity
- OFC/ACC 过度 error/threat 信号
- 影像:OFC、caudate、ACC 代谢↑,治疗后下降

---

## 4. 神经递质

- **5-HT**:SSRI(高于抑郁剂量)有效 → 5-HT 假说
- **Glutamate**:CSTC 兴奋性 → 实验性 glutamate 调节(memantine、riluzole)
- **DA**:抗精神病增效(尤伴 tic)

---

## 5. 计算视角

- **Error/uncertainty 过度信号**:无法获得"足够"确定感 → 重复
- **目标导向 vs 习惯失衡**:过度依赖习惯系统(dorsal striatum)
- **Harm avoidance** 学习异常
- 与 RL/model-based 失衡相关(见 [Reinforcement Learning Brain](../05_Computational_Neuroscience/Reinforcement_Learning_Brain.md))

---

## 6. PyTorch — 无法终止的检查 loop

```python
import torch

def ocd_loop(uncertainty_gain=2.0, threshold=0.95, max_iter=50):
    """Compulsion: repeat until 'certainty' reached, but gain too high."""
    certainty = 0.3
    checks = 0
    while certainty < threshold and checks < max_iter:
        # Each check adds info but pathological discounting prevents closure
        gain = 0.1 / (1 + uncertainty_gain)   # high gain -> tiny increments
        certainty += gain
        checks += 1
    return checks   # high uncertainty_gain -> many repetitions

# OCD: uncertainty_gain high -> never feels 'sure' -> compulsive checking
```

---

## 7. 治疗

### 7.1 心理(一线)

- **ERP**(Exposure & Response Prevention):暴露触发 + 阻止 compulsion → extinction
- 最有效心理疗法

### 7.2 药物

- **SSRI 高剂量**(高于抗抑郁,起效慢 8-12 周):fluoxetine、fluvoxamine、sertraline
- **Clomipramine**(TCA,强 5-HT)
- 增效:atypical antipsychotic(aripiprazole/risperidone)

### 7.3 难治性

- **DBS**:VC/VS、STN 等靶点(见 [DBS](../07_Neurotech_Frontiers/DBS.md))
- 既往:cingulotomy/capsulotomy(损毁,慎)
- TMS

---

## 8. 相关谱系

- Tourette / tic(共病高,CSTC 共享)
- Hoarding disorder(DSM-5 独立)
- Body dysmorphic disorder
- Trichotillomania、skin picking
- PANDAS(链球菌后儿童急性 OCD,自身免疫假说,争议)

---

## 9. 与焦虑区分

- DSM-5 已将 OCD 移出焦虑障碍单列
- 共性:焦虑驱动;差异:CSTC loop + compulsion 结构
- 自知力 + 仪式化区分于妄想 / GAD

---

## 10. Common Pitfalls

### 10.1 = 爱干净 / 完美主义

是损功能的临床障碍,非性格特质。

### 10.2 SSRI 普通剂量够

OCD 需更高剂量 + 更久起效(8-12 周)。

### 10.3 Compulsion 带来快乐

是减焦虑(负强化),非愉悦。

### 10.4 = 焦虑障碍

DSM-5 已单列;CSTC 机制特异。

### 10.5 患者不知不合理

多数有自知力(与妄想区分)。

---

## 11. Related Concepts

- **同节**:[Anxiety_Disorders](Anxiety_Disorders.md)、[Depression](Depression.md)
- **解剖**:[Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.md)
- **计算**:[Reinforcement Learning Brain](../05_Computational_Neuroscience/Reinforcement_Learning_Brain.md)
- **前沿**:[DBS](../07_Neurotech_Frontiers/DBS.md)

---

## References

1. **Stein, D. J. et al.** "Obsessive-compulsive disorder." *Nat Rev Dis Primers*, 2019.
2. **Graybiel, A. M. & Rauch, S. L.** "Toward a neurobiology of obsessive-compulsive disorder." *Neuron*, 2000.
3. **Pauls, D. L. et al.** "Obsessive-compulsive disorder: an integrative genetic and neurobiological perspective." *Nat Rev Neurosci*, 2014.
4. **Gillan, C. M. & Robbins, T. W.** "Goal-directed learning and obsessive-compulsive disorder." *Phil Trans R Soc B*, 2014.
