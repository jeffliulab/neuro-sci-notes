# 化学遗传学 (Chemogenetics / DREADD)

> *DREADD(Designer Receptors Exclusively Activated by Designer Drugs)用工程化 GPCR(对内源配体不敏感,仅被惰性合成药 CNO/DCZ 激活)实现细胞特异、可逆、**无需植入**的神经调控。优势:无光纤侵入、可全脑覆盖;代价:时间分辨低(分钟-小时)。是 optogenetics 的互补工具。*
>
> **难度**:Advanced
> **前置知识**:[Optogenetics](Optogenetics.md)、GPCR 信号基础

---

## 1. 核心思想

- 工程化 muscarinic GPCR:对乙酰胆碱失敏,仅对**惰性合成配体**响应
- "Designer receptor + designer drug"锁钥
- AAV + Cre → 细胞型特异表达
- 全身给药(IP/口服)→ 调控表达 DREADD 的特定 neuron

---

## 2. 主要 DREADD

| DREADD | G 蛋白 | 效应 |
|---|---|---|
| **hM3Dq** | Gq | 兴奋(去极化 / 增 firing) |
| **hM4Di** | Gi | 抑制(超极化 / 减释放) |
| **rM3Ds** | Gs | cAMP↑ |
| **KORD** | Gi(κ-opioid 基) | 抑制(配体 SalB)→ 与 hM3Dq 双调控 |

---

## 3. 配体演变

- **CNO**(clozapine-N-oxide):经典,但体内代谢回 clozapine(脱靶!)
- **DCZ**(deschloroclozapine):新一代,高亲和 + 低脱靶 + 低剂量
- **Compound 21**、**JHU37160** 等
- 配体选择是实验严谨性关键

---

## 4. 与 Optogenetics 对比

| | DREADD | Optogenetics |
|---|---|---|
| 触发 | 药物(全身) | 光(光纤) |
| 植入 | 无需 | 需光纤(侵入) |
| 时间分辨 | 分-小时 | 毫秒 |
| 覆盖 | 全表达区(广) | 光锥(局部) |
| 自由行为 | 易(无系绳) | 需无线 / 系绳 |
| 用 | 慢过程、长时、广域 | 快速、时序精确 |

---

## 5. PyTorch — DREADD 调控模拟

```python
import torch

def dreadd_modulation(baseline_firing, dreadd_type, ligand_conc, t):
    """Slow onset (minutes) drug-induced gain change."""
    # Pharmacokinetics: gradual rise then decay over ~hour
    drug_effect = ligand_conc * torch.exp(-((t - 30) ** 2) / 400)
    if dreadd_type == 'hM3Dq':       # excitatory
        return baseline_firing * (1 + 1.5 * drug_effect)
    elif dreadd_type == 'hM4Di':     # inhibitory
        return baseline_firing * torch.exp(-1.5 * drug_effect)
    return baseline_firing
```

---

## 6. 应用

- 慢行为过程(摄食、动机、睡眠、社交)长时调控
- 全脑/广域 cell-type 通路功能(无光纤限制)
- 神经环路在**长程行为**中的因果(数小时实验)
- 临床转化潜力(无植入 → 比 optogenetics 易转化)

---

## 7. 关键对照

- **必做**:DREADD(-) + 配体 对照(排除配体本身 / 代谢物效应)
- CNO 脱靶争议后,DCZ + 严格对照成标准
- 表达验证(免疫 / reporter)

---

## 8. 临床方向

- 工程化人源 GPCR + 安全配体 → 可调神经治疗(癫痫、疼痛、运动障碍)
- "化学遗传 DBS"概念(无电极)
- 仍需基因递送 + 长期安全(同 optogenetics 临床障碍)

---

## 9. 局限

- 时间分辨低(不能研究毫秒过程)
- 配体药代动力学 → onset/offset 不精确
- CNO 脱靶历史教训
- 仍需 AAV 递送 + 表达调控
- 效应量 / 表达水平依赖

---

## 10. Common Pitfalls

### 10.1 CNO 完全惰性

CNO 代谢回 clozapine → 脱靶;改用 DCZ + 对照。

### 10.2 时间精确

分钟-小时尺度;非毫秒(用 optogenetics 做快过程)。

### 10.3 无需对照

必须 DREADD(-)+ 配体对照(配体效应)。

### 10.4 = optogenetics 替代

互补:DREADD 慢广无植入,opto 快精局部。

### 10.5 即临床可用

需基因递送 + 长期安全;仍研究阶段。

---

## 11. Related Concepts

- **同节**:[Optogenetics](Optogenetics.md)、[Optogenetics_Advanced](Optogenetics_Advanced.md)、[Focused Ultrasound](Focused_Ultrasound.md)
- **细胞**:[Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.md)(GPCR)
- **基础**:[Neural Circuits](../00_Foundations/Neural_Circuits.md)

---

## References

1. **Armbruster, B. N. et al.** "Evolving the lock to fit the key to create a family of GPCRs (DREADD)." *PNAS*, 2007.
2. **Roth, B. L.** "DREADDs for neuroscientists." *Neuron*, 2016.
3. **Gomez, J. L. et al.** "Chemogenetics revealed: DREADD occupancy and activation via converted clozapine." *Science*, 2017.
4. **Nagai, Y. et al.** "Deschloroclozapine, a potent and selective chemogenetic actuator." *Nat Neurosci*, 2020.
