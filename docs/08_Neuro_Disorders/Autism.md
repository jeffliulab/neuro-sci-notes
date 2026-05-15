# 自闭症谱系障碍 (Autism Spectrum Disorder)

> *ASD 是 neurodevelopmental disorder,prevalence ~1/36 (CDC 2023)。三核特征:social communication 缺陷 + restricted/repetitive behavior + sensory atypicality。Heterogeneous spectrum:从严重残疾到 high-functioning (前 Asperger 1994 取消)。Heritability ~ 80%,多基因 + 环境 multi-factor。无 cure,行为干预 (ABA) 主流。*
>
> **难度**:Intermediate
> **前置知识**:[Social_Cognition](../04_Cognitive_Neuroscience/Social_Cognition.md)、[Cortex](../01_Neuroanatomy/Cortex.md)

---

## 1. 临床 (DSM-5)

A. Social communication 缺陷(持续):
   1. Social-emotional reciprocity
   2. Nonverbal communication
   3. 维持关系
   
B. Restricted, repetitive behaviors / interests / activities(≥ 2):
   1. Stereotyped movements
   2. Insistence on sameness
   3. 强烈兴趣
   4. Sensory hyper/hypo-reactivity

发病:< 3 岁(早期 marker 9-18 个月可测)。

---

## 2. Spectrum 取代 subtypes

- 2013 DSM-5 合并 Asperger、PDD-NOS、Autism 为 ASD
- 新增 levels 1-3 (mild → severe)
- 强调 spectrum,反对"低/高 functioning"二分

---

## 3. 流行病学

- US prevalence: 2023 ~ 1/36 (儿童)
- Male : Female = 4 : 1(但 female 诊断不足)
- 高 IQ + ASD: high-functioning autism
- Co-morbidity: ADHD (50%)、anxiety、epilepsy、GI 问题

---

## 4. 神经基础(争议)

### 4.1 Connectivity 异常

- Local hyperconnectivity + long-range hypoconnectivity
- Default Mode Network atypical
- Mirror neuron system 假说(有争议)

### 4.2 Brain Overgrowth

- 早期 brain volume 增 (尤其 amygdala)
- 后期"normalization"

### 4.3 GABA/Glu 失衡

- E/I balance shift toward excitation
- 高 epilepsy 风险关联

### 4.4 Theory of Mind 缺陷 (Baron-Cohen)

- False belief task 表现差
- 但近年质疑

---

## 5. 遗传

- Heritability ~ 80%
- 100+ candidate genes (SHANK3, NRXN1, MECP2, CHD8, FMR1 等)
- 单基因 ASD (e.g., Fragile X, Rett) 占 ~ 10%
- 多基因 + CNV (copy number variants)

---

## 6. 干预

### 6.1 Behavioral

- **Applied Behavior Analysis (ABA)**: 主流,但有伦理争议
- **Early Start Denver Model (ESDM)**: ABA + 发展心理学
- **PECS**: 图片交流
- **Social skills training**

### 6.2 Pharmacological

- 无药物治疗核心症状
- Risperidone / Aripiprazole 治 irritability
- SSRI 治 anxiety
- 实验:Oxytocin、bumetanide(GABA shift)

### 6.3 Sensory

- OT (occupational therapy)
- 感官 accommodation(降噪耳机等)

---

## 7. PyTorch — E/I Imbalance Sim

```python
import torch

def simulate_E_I_imbalance(N=100, T=200, E_strength=1.5, I_strength=1.0):
    """Higher E/I ratio → more sustained firing (ASD model)."""
    h = torch.zeros(N)
    W_E = torch.randn(N, N).abs() * E_strength
    W_I = -torch.randn(N, N).abs() * I_strength
    W = W_E + W_I
    W.fill_diagonal_(0)
    
    activity = []
    for _ in range(T):
        h = torch.relu(W @ h + torch.randn(N) * 0.1)
        activity.append(h.mean().item())
    return activity
```

---

## 8. Strengths / Neurodiversity

- 不仅 "缺陷":强烈 pattern recognition、attention to detail
- ASD scientists、engineers 多(Temple Grandin、Greta Thunberg)
- Neurodiversity movement:autism 是 variant 非 disease

---

## 9. AI Connection

- LLM autistic-coded?(争议,反对 stigma)
- Robot for autism therapy(Kaspar、Nao)
- Eye-tracking + ML 用于 early detection

---

## 10. Common Pitfalls

### 10.1 疫苗导致 ASD

完全错误(Wakefield 1998 fraud retracted)。

### 10.2 高 functioning = 没问题

仍有 sensory、anxiety、social 挑战。

### 10.3 Autism = ToM 缺陷

近年 double empathy problem 理论:autistic 之间沟通良好,与 NT 才有 mismatch。

### 10.4 ABA 万能

ABA 有伦理批评(强制 masking、压抑特征)。

### 10.5 男孩多

实际 female 多被忽略 / mis-diagnosed (camouflaging)。

---

## 11. Related Concepts

- **同节**:[Schizophrenia](Schizophrenia.md)、[Depression](Depression.md)
- **认知**:[Social_Cognition](../04_Cognitive_Neuroscience/Social_Cognition.md)、[Working_Memory](../04_Cognitive_Neuroscience/Working_Memory.md)
- **解剖**:[Amygdala](../01_Neuroanatomy/Amygdala.md)

---

## References

1. **APA** *DSM-5*. 2013.
2. **Baron-Cohen, S.** *Mindblindness*. MIT Press, 1995.
3. **Lord, C. et al.** "Autism spectrum disorder." *Nat Rev Dis Primers*, 2020.
4. **Maenner, M. J. et al.** "Prevalence and characteristics of autism spectrum disorder." *MMWR*, 2023.
