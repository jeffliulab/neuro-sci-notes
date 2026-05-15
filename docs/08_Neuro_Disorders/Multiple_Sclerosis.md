# 多发性硬化 (Multiple Sclerosis)

> *MS 是 autoimmune demyelinating disease of CNS。免疫细胞攻击 myelin → 多 lesion → 神经传导减慢 / fail。Prevalence ~ 1/1000(高纬度多)。Female 2-3× more。Relapsing-remitting (RRMS) → secondary progressive (SPMS)。Beta-interferon 1993 首治疗。Ocrelizumab、Natalizumab 等 mAb 当代。MRI gadolinium 是 marker。*
>
> **难度**:Intermediate
> **前置知识**:[Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.md)、[Glia](../02_Cellular_Molecular/Glia.md)

---

## 1. 病理

- T cell + B cell + macrophage 攻击 CNS myelin (oligodendrocytes)
- 多 lesion(plaques)散布:
  - Periventricular
  - Juxtacortical
  - Infratentorial
  - Spinal cord
- 失髓鞘 → 信号 saltatory conduction 失 → 慢、fail
- 后期 axonal degeneration → 永久 disability

---

## 2. 临床类型

| 型 | 描述 |
|---|---|
| **CIS** (Clinically Isolated Syndrome) | 单次 episode,可能演 RRMS |
| **RRMS** (Relapsing-Remitting) | 复发 + 恢复(85% 起初) |
| **SPMS** (Secondary Progressive) | RRMS 后慢 progressive |
| **PPMS** (Primary Progressive) | 一开始 progressive(15%) |

---

## 3. 症状(多变)

- **Visual**: optic neuritis(单眼急失明,RRMS 起 50%)
- **Motor**: spasticity、weakness
- **Sensory**: numbness、paresthesia、Lhermitte sign
- **Cerebellar**: ataxia、dysmetria
- **Bladder**: urgency、retention
- **Fatigue**: 普遍 + 严重
- **Cognitive**: 速度、工作记忆

---

## 4. 诊断

- **MRI**: T2 / FLAIR hyperintense lesions,Gadolinium-enhancing(活动)
- **CSF**: oligoclonal bands(B cell 局部抗体)
- **VEP** (Visual Evoked Potential): optic 速度
- **McDonald criteria**: lesion 时空 dissemination

---

## 5. 危险因素

- **Geography**: 高纬度 (Vit D 缺少?)
- **Ethnicity**: 北欧裔
- **Genetics**: HLA-DRB1*15:01
- **Female**
- **EBV** (Epstein-Barr virus): 2022 Bjornevik 大数据强 link
- **Smoking**, **obesity adolescence**

---

## 6. 治疗

### 6.1 急 relapse

- 高剂 corticosteroids (methylprednisolone 1g × 3-5 day)
- Plasmapheresis(顽固病例)

### 6.2 Disease-modifying (DMT)

| Drug | 类 | 机制 |
|---|---|---|
| **Beta-interferon** | Injectable | 减 T-cell migration |
| **Glatiramer acetate** | Injectable | Myelin mimic peptide |
| **Fingolimod** | Oral | S1P modulator → 留 lymphocyte 淋巴 |
| **Dimethyl fumarate** | Oral | NRF2 pathway |
| **Teriflunomide** | Oral | 抗增殖 |
| **Natalizumab** | mAb | α4-integrin blocker (但 PML 风险) |
| **Ocrelizumab** | mAb | Anti-CD20 (B cell depletion) |
| **Ofatumumab** | mAb | Anti-CD20 (sub-Q) |
| **Cladribine** | Oral | Lymphocyte depletion |
| **AHSCT** (autologous stem cell) | Aggressive | Reset immune |

### 6.3 PPMS 特

- Ocrelizumab 是唯一 FDA 批 PPMS
- 大部分 DMT 对 PPMS 效差

### 6.4 Symptomatic

- Spasticity: baclofen、tizanidine
- Fatigue: amantadine、modafinil
- Bladder: anticholinergics
- Pain: gabapentin
- Depression: SSRI

---

## 7. PyTorch — 失髓鞘 sim

```python
import torch

def simulate_demyelination(N_segments=50, myelin_loss=0.3, T=100):
    """AP propagation with myelinated vs demyelinated axon."""
    conduction_speed = torch.ones(N_segments)
    # 30% segments are demyelinated → 10x slower
    demyelinated_mask = torch.rand(N_segments) < myelin_loss
    conduction_speed[demyelinated_mask] *= 0.1
    
    total_time = (1 / conduction_speed).sum().item()
    return total_time
```

---

## 8. EBV Link (2022 Big Discovery)

- Bjornevik et al. *Science* 2022:DoD blood bank 10M 军人
- Pre-MS 接触 EBV → 32× MS 风险
- 几乎所有 MS 患者前有 EBV
- → EBV likely necessary (not sufficient) cause
- → 疫苗 + 抗病毒治 future

---

## 9. AHSCT (Autologous Hematopoietic Stem Cell)

- 化疗 ablate 免疫系统 → 自体干细胞 reset
- Aggressive but 1-time
- 多 trial 显示比 DMT 更 effective in RRMS
- 但 mortality risk ~ 1-2%

---

## 10. Common Pitfalls

### 10.1 MS = ALS

完全不同;MS 是 demyelination autoimmune,ALS 是 motor neuron 退化。

### 10.2 Tx 必 stop progression

DMT 减 relapse、slow progression,但 not stop。

### 10.3 EBV 决定

EBV 必要 not 充分;其他 factor cofactor。

### 10.4 PPMS 与 RRMS 同源

可能不同 biology;PPMS treatment 难。

### 10.5 Heat 让 MS 恶

Uhthoff phenomenon — 高温 → 暂时 worsening,but reversible。

---

## 11. Related Concepts

- **同节**:[Alzheimer](Alzheimer.md)、[Parkinson](Parkinson.md)、[ALS](ALS.md)
- **细胞**:[Glia](../02_Cellular_Molecular/Glia.md)、[Action_Potential](../02_Cellular_Molecular/Action_Potential.md)

---

## References

1. **Reich, D. S., Lucchinetti, C. F., Calabresi, P. A.** "Multiple sclerosis." *NEJM*, 2018.
2. **Bjornevik, K. et al.** "Longitudinal analysis reveals high prevalence of Epstein-Barr virus associated with multiple sclerosis." *Science*, 2022.
3. **Thompson, A. J. et al.** "Diagnosis of multiple sclerosis: 2017 revisions of the McDonald criteria." *Lancet Neurol*, 2018.
4. **Hauser, S. L. & Cree, B. A. C.** "Treatment of Multiple Sclerosis: A Review." *Am J Med*, 2020.
