# 精神分裂症 (Schizophrenia)

> *Schizophrenia 是 chronic mental disorder,~ 1% 患病率。特征:positive (幻觉 / 妄想) + negative (动力缺) + cognitive (执行) 症状。DA hypothesis 主流但不完整;现代视为 NMDA + DA + Glu 多系统失调。Antipsychotics 1950s 革命但仍主治症状不治本。*
>
> **难度**:Intermediate
> **前置知识**:[神经递质](../02_Cellular_Molecular/Neurotransmitters.md)、[Cortex](../01_Neuroanatomy/Cortex.md)

---

## 1. 临床

DSM-5:2+ 症状 ≥ 1 月:
- **Positive**: 幻觉 (听觉常见), 妄想, 思维 disorganized
- **Negative**: 情感 flat, 缺动力, 缩退
- **Cognitive**: 注意, 工作记忆, 执行 deficit

发病:典型青年期 (15-25)。

---

## 2. 神经基础假说

### 2.1 Dopamine Hypothesis (经典)

- DA 过量 (mesolimbic) → positive symptoms
- DA 不足 (mesocortical) → negative
- 证据:amphetamine (DA agonist) 引发 psychosis
- 抗精神病药 = D2 receptor 拮抗

### 2.2 NMDA Hypofunction (现代)

- NMDA 不足 → glutamate 失调
- 证据:Ketamine (NMDA antagonist) 引发类 schizophrenia 症状
- PCP 同
- 解释 cognitive + negative 症状好

### 2.3 GABA Interneurons

- PV-positive 中间 neuron 失功能
- 影响 cortical 同步 (gamma oscillation)
- 与 working memory 失调

### 2.4 Neurodevelopmental

- 青春期 abnormal pruning
- Early-life stressor + genetic susceptibility

---

## 3. 遗传

- Heritability ~ 80%
- Polygenic (千 SNPs)
- 22q11.2 deletion: 30× 风险
- COMT, DISC1, NRG1 等 candidate genes

---

## 4. 治疗

### 4.1 First-generation antipsychotics (FGA)

- **Haloperidol, Chlorpromazine**: D2 拮抗
- 强 positive 症状治疗
- Extrapyramidal side effects (parkinsonism, tardive dyskinesia)

### 4.2 Second-generation (atypical, SGA)

- **Risperidone, Olanzapine, Quetiapine, Clozapine**
- 加 5-HT2A 拮抗
- 较少 motor side effects
- 但 metabolic (weight gain, diabetes)

### 4.3 Clozapine

- 难治 case 唯一证明有效
- 但 agranulocytosis 风险 (需 blood monitoring)

### 4.4 实验

- NMDA modulator (D-serine, GLYX-13)
- Anti-inflammatory
- Psychotherapy + cognitive rehabilitation

---

## 5. 影像 / Biomarker

- MRI:Lateral ventricle 增大, gray matter 减
- DTI:white matter 异常
- fMRI:DMN + cognitive control network 失调
- DA imaging (PET): striatum DA 释放 ↑

---

## 6. 与 AI / Mech Interp

- Predictive coding 错误 → 幻觉
- LLM "hallucination" 是有趣 parallel
- Karl Friston 用 free energy framework 解释

---

## 7. PyTorch — 神经网络模拟 prediction error

```python
import torch

def simulate_schizo_prediction_error(noise_level=2.0):
    """Hyper-active prediction → 'hallucination' 在 silent context."""
    # Normal: low noise → accurate prediction
    # Schizo: high noise → false-positive predictions
    context = torch.zeros(100)
    sensory = torch.randn(100) * 0.1  # small actual sensory
    prediction = context * 0 + torch.randn(100) * noise_level
    # Surprise / error
    error = sensory - prediction
    return error.abs().mean()  # high in schizo
```

---

## 8. 数字

- 全球 ~ 2400 万患者 (2025)
- 自杀率 5-10% (高于一般 population)
- 寿命 < 15-20 年 (心血管, 自杀)
- 男 slight > 女

---

## 9. Common Pitfalls

### 9.1 DA hypothesis 不完整

不解 negative + cognitive。

### 9.2 D2 拮抗 副作用

Motor + metabolic 问题严重。

### 9.3 "Split personality" 误解

Schizophrenia ≠ multiple personality disorder。

### 9.4 早期诊断难

Prodromal phase 仅 cognitive 异常。

### 9.5 Stigma

社会偏见 → 治疗依从性差。

---

## 10. Related Concepts

- **同节**:[Depression](Depression.md)、[Alzheimer](Alzheimer.md)、[Parkinson](Parkinson.md)
- **神经递质**:[DA, Glu, GABA](../02_Cellular_Molecular/Neurotransmitters.md)
- **计算**:[Predictive Coding](../05_Computational_Neuroscience/Predictive_Coding.md)

---

## References

1. **Howes, O. D. & Kapur, S.** "The dopamine hypothesis of schizophrenia: version III." *Schizophr Bull*, 2009.
2. **Olney, J. W. & Farber, N. B.** "Glutamate receptor dysfunction and schizophrenia." *Arch Gen Psychiatry*, 1995.
3. **Lieberman, J. A. et al.** "Effectiveness of antipsychotic drugs in patients with chronic schizophrenia (CATIE)." *NEJM*, 2005.
4. **Kahn, R. S. et al.** "Schizophrenia." *Nat Rev Dis Primers*, 2015.
