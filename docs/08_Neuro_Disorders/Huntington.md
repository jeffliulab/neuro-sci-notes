# 亨廷顿病 (Huntington's Disease)

> *Huntington's Disease (HD) 是 autosomal dominant 神经退行 disease,~ 1/10,000 prevalence。HTT 基因 CAG 重复 expansion → mutant huntingtin protein → striatum (尤 GABA medium spiny neuron) 退化 → chorea + cognitive decline + 精神症状。George Huntington 1872 描述。无治愈,Tetrabenazine 控 chorea。Tominersen ASO 临床试败,基因治新方向。*
>
> **难度**:Intermediate
> **前置知识**:[Basal_Ganglia](../01_Neuroanatomy/Basal_Ganglia.md)、Parkinson

---

## 1. 临床

### 1.1 Motor

- **Chorea**: 不自主、流畅、舞蹈样动作
- Dystonia、bradykinesia(晚期)
- Saccade abnormality(早 marker)
- 跌倒、吞咽

### 1.2 Cognitive

- Executive 缺陷(早)
- Working memory 下降
- Procedural learning 损
- 最终 dementia

### 1.3 Psychiatric

- 抑郁、焦虑
- Apathy
- 精神病、行为障碍
- 自杀风险高

---

## 2. 遗传

- **HTT 基因** 4 号染色体,exon 1 CAG repeat
- Normal: 9-35 repeats
- Pre-mutation: 27-35
- **HD: ≥ 40 repeats**
- 36-39: incomplete penetrance
- 重复越多 → onset 越早(anticipation 主要 paternal)

---

## 3. 病理生理

- **Mutant huntingtin (mHTT)**: polyglutamine tract → 误折叠 + 聚集
- 主要影响:
  - Striatum GABA medium spiny neurons (MSN)
  - Cortex (later)
- 直接 vs 间接通路:间接 path 先损 → chorea
- 后期 dopamine 系统衰退 → parkinsonism

---

## 4. 与 Parkinson 反差

| | Parkinson | Huntington |
|---|---|---|
| 突触损失 | SNc DA | Striatal GABA |
| 通路影响 | Direct ↓ | Indirect ↓ (early) |
| Motor | Hypokinetic (僵) | Hyperkinetic (chorea) |
| Tx | L-DOPA(+) | Tetrabenazine (DA depleter,-) |

---

## 5. 影像

- **MRI**: caudate atrophy(早 marker,先比 putamen)
- **PET**: 葡萄糖代谢 ↓ striatum
- **MRS**: glutamate / NAA changes

---

## 6. Onset 时间

- Typical adult onset 30-50 岁
- Juvenile HD (< 20 岁): 重复 60+,父系遗传多
- Late-onset (> 60 岁): 40-50 repeats,milder

---

## 7. 治疗

### 7.1 Symptomatic

- **Tetrabenazine / deutetrabenazine** (Austedo): VMAT2 抑 → DA depleter → 减 chorea
- **Antipsychotics**: 兼治 chorea + psychiatric
- **SSRI**: 抑郁
- **Physical/occupational therapy**

### 7.2 Disease-modifying (实验)

- **Tominersen (Roche)**: ASO 减 mHTT — 2021 Phase III 失败
- **Branaplam (Novartis)**: 类似失败
- **Gene therapy (AAV)**: uniQure trial 进行
- **Stem cell**: 早期
- **CRISPR**: 临前

---

## 8. 预测 + 遗传咨询

- 基因测可预测(若家族 + 测 HTT CAG 长度)
- 但 ethical:无治疗 → 知道有何用?
- 多数 at-risk 选 不测
- 产前 / IVF 选 unaffected embryo (PGD)

---

## 9. PyTorch — 简化 BG Hyperkinetic Sim

```python
import torch

def hd_basal_ganglia_sim(direct_strength=1.0, indirect_strength=0.3, T=200):
    """HD: indirect 通路 weaken → hyperkinetic."""
    activity = []
    for t in range(T):
        direct = direct_strength * torch.randn(1)
        indirect = indirect_strength * torch.randn(1)  # 弱
        movement = direct - indirect  # excess
        activity.append(movement.item())
    return activity
```

---

## 10. Common Pitfalls

### 10.1 Chorea = HD only

错;Sydenham chorea(post-strep)、wilson、tardive dyskinesia 也 chorea。

### 10.2 Familial 必发病

> 99% with CAG ≥ 40,但 timing 差。

### 10.3 Tominersen 必有效

未 prove。复杂 ASO challenges。

### 10.4 HD 仅 motor

Cognitive + psychiatric 同样 disabling,常 missed。

### 10.5 Caudate 唯一影响

后期 cortex、cerebellum、其他 BG 都 affected。

---

## 11. Related Concepts

- **同节**:[Parkinson](Parkinson.md)、[Alzheimer](Alzheimer.md)、[ALS](ALS.md)
- **解剖**:[Basal_Ganglia](../01_Neuroanatomy/Basal_Ganglia.md)
- **奖赏**:[Reward_System](../03_Systems_Neuroscience/Reward_System.md)

---

## References

1. **Huntington, G.** "On chorea." *Med Surg Reporter*, 1872.
2. **HD Collaborative Research Group** "A novel gene containing a trinucleotide repeat that is expanded and unstable on Huntington's disease chromosomes." *Cell*, 1993.
3. **Bates, G. P. et al.** "Huntington disease." *Nat Rev Dis Primers*, 2015.
4. **Tabrizi, S. J. et al.** "Targeting Huntingtin Expression in Patients with Huntington's Disease." *NEJM*, 2019.
