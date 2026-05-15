# 抑郁症 (Depression) — Major Depressive Disorder

> *Depression 是全球最常见 mental disorder (~ 5% 患病率)。神经基础涉 monoamine (5-HT, NE, DA) + HPA axis + 海马 + DMN 多系统。SSRI 60+ 年 mainstream;近年 esketamine + psilocybin 革命性 (快速 + 单剂)。*
>
> **难度**:Intermediate
> **前置知识**:[神经递质](../02_Cellular_Molecular/Neurotransmitters.md)、[Hippocampus](../01_Neuroanatomy/Hippocampus_Anatomy.md)

---

## 1. 临床

DSM-5 criteria:9 项中 ≥ 5 项 ≥ 2 周:
- Depressed mood
- Anhedonia (失乐感)
- Sleep / appetite 变化
- Energy 减
- Concentration 差
- Worthlessness / guilt
- Suicidal thoughts
- 等

---

## 2. 神经基础假说

### 2.1 Monoamine Hypothesis (1965)

- 5-HT / NE / DA 减少
- SSRI / SNRI 增 these → 抗抑郁
- 但:延迟起效 (2-4 周) → 不能完全解释

### 2.2 Neurotrophic Hypothesis

- BDNF (Brain-Derived Neurotrophic Factor) 减少
- 海马 neurogenesis 降
- LTP 受损
- 抗抑郁药 + 运动 增 BDNF

### 2.3 Inflammation Hypothesis

- Cytokines (IL-6, TNF-α) 增
- Microglia 激活
- 与 monoamine 互动

### 2.4 HPA Axis

- 慢性 stress → cortisol 高
- 损海马 (volume 减少)
- 形成 vicious cycle

### 2.5 Network Dysfunction

- DMN (Default Mode Network) hyperactivity → rumination
- Salience network 失调
- PFC ↓ amygdala ↑

---

## 3. 治疗

### 3.1 第一线

- **SSRI** (Prozac, Zoloft, Lexapro): 5-HT reuptake 阻
- **SNRI** (Venlafaxine, Duloxetine): 加 NE
- **CBT** (Cognitive Behavioral Therapy): 心理治疗

### 3.2 二线

- **Bupropion**: DA + NE
- **Mirtazapine**: 5-HT receptor 拮抗 (sedating)
- **Tricyclic antidepressants** (TCA): 老,副作用多
- **MAOI**: 老,有 dietary interaction

### 3.3 革命性 (2019+)

- **Esketamine (Spravato)**: NMDA antagonist, 鼻喷, 数小时起效, FDA 2019
- **Psilocybin** (实验): 1-2 dose 长期效;FDA breakthrough 2018

### 3.4 物理治疗

- **ECT (Electroconvulsive Therapy)**: 严重难治 case
- **TMS** (rTMS on dlPFC): FDA 2008
- **DBS** (实验): SCC25 / vmPFC

---

## 4. Treatment-Resistant Depression (TRD)

- 2+ adequate trials 仍 fail
- ~ 30% MDD 患者
- 候选:Esketamine, ECT, psilocybin, DBS

---

## 5. 数字

- 全球 ~3.8 亿患者 (2025)
- WHO 全球 disability 主因
- 自杀 ~70万/年 (50% 有 depression)
- 男 vs 女:1:2

---

## 6. 与遗传

- Heritability ~ 35-40%
- 没有单 gene;polygenic (1000+ SNPs)
- 5-HTTLPR polymorphism × life stress 经典 GxE

---

## 7. PyTorch — Depression risk model (toy)

```python
import torch
import torch.nn as nn

class DepressionRiskModel(nn.Module):
    """Predict depression from multi-modal."""
    def __init__(self):
        super().__init__()
        # Genetic SNPs
        self.snp_encoder = nn.Linear(1000, 64)
        # Self-report questionnaire (PHQ-9)
        self.phq_encoder = nn.Linear(9, 16)
        # Wearable (sleep, activity)
        self.wear_encoder = nn.LSTM(5, 32, batch_first=True)
        # Combine
        self.classifier = nn.Linear(64 + 16 + 32, 1)
    
    def forward(self, snp, phq, wear_ts):
        s = self.snp_encoder(snp)
        p = self.phq_encoder(phq)
        _, (h, _) = self.wear_encoder(wear_ts)
        feat = torch.cat([s, p, h.squeeze(0)], dim=-1)
        return torch.sigmoid(self.classifier(feat))
```

---

## 8. Common Pitfalls

### 8.1 "Chemical imbalance" 简化

5-HT 低 ≠ 唯一原因;modern view multi-system。

### 8.2 SSRI 不是 magic bullet

~ 40-60% remission only。

### 8.3 Placebo response 强

抗抑郁试验 placebo response 30-40%。

### 8.4 Heterogeneity

Depression 是 syndrome;多 subtypes。

### 8.5 SSRI 长期效应

依赖性 ⊕ withdrawal 难;tapering 重要。

---

## 9. AI / Tech 应用

- 移动 app monitor 心情
- 自然语言处理识别 depressive language
- DL fMRI 分类
- Chatbot therapy (Woebot, Replika 等)

---

## 10. Related Concepts

- **同节**:[Alzheimer](Alzheimer.md)、[Parkinson](Parkinson.md)
- **神经递质**:[5-HT, DA](../02_Cellular_Molecular/Neurotransmitters.md)
- **Reward system**:[Reward](../03_Systems_Neuroscience/Reward_System.md)

---

## References

1. **Schildkraut, J. J.** "The catecholamine hypothesis of affective disorders." *Am J Psychiatry*, 1965.
2. **Duman, R. S. & Aghajanian, G. K.** "Synaptic dysfunction in depression: potential therapeutic targets." *Science*, 2012.
3. **Daly, E. J. et al.** "Efficacy of Esketamine Nasal Spray Plus Oral Antidepressant Treatment for Relapse Prevention in Patients with TRD." *JAMA Psychiatry*, 2019.
4. **Carhart-Harris, R. L. et al.** "Trial of psilocybin versus escitalopram for depression." *NEJM*, 2021.
