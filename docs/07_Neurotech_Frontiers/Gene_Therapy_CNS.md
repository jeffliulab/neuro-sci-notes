# 中枢神经基因治疗 (CNS Gene Therapy)

> *基因治疗为单基因神经病带来根治希望。AAV(腺相关病毒)递送基因、ASO(反义寡核苷酸)调控 RNA、CRISPR 编辑。已批准:Zolgensma(SMA,AAV9,~ $2M)、Spinraza(SMA ASO)、Luxturna(遗传失明)、Tofersen(SOD1-ALS)。BBB + 递送 + 成本是核心挑战(见 [Blood Brain Barrier](../00_Foundations/Blood_Brain_Barrier.md))。*
>
> **难度**:Advanced
> **前置知识**:[Blood Brain Barrier](../00_Foundations/Blood_Brain_Barrier.md)、分子生物基础

---

## 1. 三大模式

| 模式 | 机制 | 例 |
|---|---|---|
| **基因替代**(AAV) | 递送功能基因拷贝 | Zolgensma(SMN1) |
| **RNA 调控**(ASO/siRNA) | 反义寡核苷酸改剪接/降解 | Spinraza、Tofersen |
| **基因编辑**(CRISPR) | 直接改 DNA | 临床早期(HD、亨廷顿) |

---

## 2. AAV 载体

- 小、低免疫原、不整合(多为 episomal)、长表达
- 血清型决定嗜性:**AAV9**、**AAVrh10**(可过 BBB,尤新生儿)
- 工程衣壳(PHP.eB 等)增强 CNS 嗜性(动物)
- 限:包装容量 ~ 4.7 kb(大基因难)、预存抗体、剂量肝毒

---

## 3. 递送路径

| 路径 | 覆盖 | 侵入 |
|---|---|---|
| IV(静脉) | 广(需过 BBB,AAV9) | 低 |
| 鞘内 / ICV | CSF → 广 CNS | 中 |
| 实质内注射 | 局部精确 | 高(立体定向) |
| 鼻内 | 绕 BBB(有限) | 低 |

---

## 4. 已批准里程碑

- **Zolgensma**(2019):AAV9-SMN1,SMA 一次性,~ $2.1M(最贵药之一)
- **Spinraza**(2016):ASO,SMN2 剪接修正,鞘内反复
- **Luxturna**(2017):AAV-RPE65,遗传性视网膜病(首个 FDA 基因治疗)
- **Tofersen / Qalsody**(2023):SOD1-ALS ASO(降 SOD1)

---

## 5. PyTorch — ASO 剪接修正(概念)

```python
import torch

def aso_splice_correction(pre_mrna_exons, aso_target_exon, efficiency=0.8):
    """ASO blocks a splice silencer -> include skipped exon (e.g., SMN2)."""
    included = pre_mrna_exons.clone().float()
    # Without ASO: target exon skipped (=0). ASO restores inclusion.
    included[aso_target_exon] = efficiency      # fraction corrected
    functional_protein = included.prod()         # all exons needed
    return functional_protein
```

---

## 6. 候选疾病

- **单基因**最适:SMA、ALS(SOD1/C9orf72)、HD(HTT ASO — Tominersen 试败,见 [Huntington](../08_Neuro_Disorders/Huntington.md))、Rett(MECP2,剂量敏感难)、Friedreich、AADC 缺乏(已批 Upstaza)
- **多基因/散发**(AD、PD、SCZ)难得多(非单靶)
- 溶酶体贮积病(神经型)

---

## 7. ASO vs AAV

| | ASO | AAV |
|---|---|---|
| 持久 | 需反复(鞘内) | 一次性(长表达) |
| 可逆 | ✓(停药) | ✗(难撤) |
| 容量 | 短序列 | ≤ 4.7 kb |
| 适合 | knockdown / 剪接 | 替代缺失基因 |
| 例 | Spinraza/Tofersen | Zolgensma/Luxturna |

---

## 8. 挑战

- **BBB**:全身递送难入脑(AAV9 部分,新生儿更好)
- **成本**:$1-3M(可及性 + 医保)
- **免疫**:预存 AAV 抗体排除部分患者;衣壳免疫
- **剂量毒性**:高剂 AAV 肝 / DRG 毒(致死个案)
- **不可逆**(AAV)+ 脱靶(CRISPR)
- **时间窗**:神经退行常需早干预

---

## 9. 前沿

- 工程衣壳跨 BBB(人源化验证中)
- + Focused Ultrasound 开 BBB 递送(见 [Focused Ultrasound](Focused_Ultrasound.md))
- In vivo base/prime editing(更精准少脱靶)
- 可调控表达(诱导启动子)
- 个性化 ASO(Milasen — 单患者定制先例)

---

## 10. Common Pitfalls

### 10.1 治愈所有神经病

最适**单基因**;多基因/散发(AD/PD)远未可及。

### 10.2 AAV 一劳永逸无风险

不可逆 + 免疫 + 高剂肝毒;非无忧。

### 10.3 IV 注射即入脑

BBB 强阻;仅特定血清型 + 新生儿较好。

### 10.4 ASO = 基因治疗(永久)

ASO 可逆需反复;与 AAV 替代不同。

### 10.5 CRISPR 已临床常规

CNS CRISPR 仍早期;脱靶 + 递送未解。

---

## 11. Related Concepts

- **同节**:[Focused Ultrasound](Focused_Ultrasound.md)、[Optogenetics_Advanced](Optogenetics_Advanced.md)
- **基础**:[Blood Brain Barrier](../00_Foundations/Blood_Brain_Barrier.md)
- **疾病**:[ALS](../08_Neuro_Disorders/ALS.md)、[Huntington](../08_Neuro_Disorders/Huntington.md)、[Multiple Sclerosis](../08_Neuro_Disorders/Multiple_Sclerosis.md)

---

## References

1. **Mendell, J. R. et al.** "Single-dose gene-replacement therapy for spinal muscular atrophy (Zolgensma)." *NEJM*, 2017.
2. **Finkel, R. S. et al.** "Nusinersen versus sham control in infantile-onset SMA (Spinraza)." *NEJM*, 2017.
3. **Miller, T. et al.** "Trial of antisense oligonucleotide tofersen for SOD1 ALS." *NEJM*, 2022.
4. **Deverman, B. E. et al.** "Gene therapy for neurological disorders: progress and prospects." *Nat Rev Drug Discov*, 2018.
