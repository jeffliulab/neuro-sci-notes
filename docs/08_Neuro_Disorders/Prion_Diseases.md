# 朊病毒病 (Prion Diseases)

> *Prion disease(CJD、kuru、FFI、GSS、动物 BSE/scrapie)是唯一由**蛋白质构象**传播的传染性神经退行病。Prusiner "protein-only" 假说(1997 Nobel):错折叠 PrPˢᶜ 模板诱导正常 PrPᶜ 错折叠 → 链式扩增。100% 致死,无治疗。是理解 AD/PD/ALS 蛋白病"prion 样"扩散的范式。*
>
> **难度**:Advanced
> **前置知识**:[Alzheimer](Alzheimer.md)、蛋白折叠基础

---

## 1. Protein-Only 假说

- 无核酸的传染因子(违反中心法则直觉)
- **PrPᶜ**(正常,α-helix 富)→ **PrPˢᶜ**(致病,β-sheet 富)
- PrPˢᶜ 作模板诱导 PrPᶜ 转构象 → 指数扩增
- Prusiner 1982 提出,1997 Nobel(曾极受争议)

---

## 2. 人类朊病毒病

| 病 | 来源 |
|---|---|
| **sCJD**(散发型) | 自发错折叠(~ 85%) |
| **vCJD**(变异型) | 食用 BSE 牛肉 |
| **iCJD**(医源性) | 角膜/硬膜移植、生长激素、器械 |
| **fCJD/GSS/FFI**(家族性) | PRNP 基因突变 |
| **Kuru** | 食人习俗(Fore 族,已绝) |

---

## 3. 临床特征

- 快速进展性痴呆(数月,远快于 AD)
- 肌阵挛(myoclonus)
- 共济失调、视觉障碍
- **FFI**:进行性失眠 → 死(丘脑前部退行 — 见 [Thalamus](../01_Neuroanatomy/Thalamus.md))
- 病程:数月-2 年,100% 致死

---

## 4. 病理

- **Spongiform**(海绵状空泡)— 标志性
- PrPˢᶜ 沉积、神经元丢失、星形胶质增生
- 无典型炎症(与感染不同)
- 脑组织感染性极强(标准灭菌**无效**!)

---

## 5. PyTorch — Prion 自催化扩增

```python
import numpy as np

def prion_propagation(prp_c=1000, prp_sc=1, rate=0.05, T=200):
    """Autocatalytic templated conversion: PrP^Sc converts PrP^C."""
    c, sc = float(prp_c), float(prp_sc)
    traj = []
    for t in range(T):
        converted = rate * sc * c / (prp_c)   # template-dependent
        c -= converted
        sc += converted
        c += 5                                # synthesis of normal PrP
        traj.append(sc)
    return traj   # exponential then saturating PrP^Sc growth

# Same kinetic motif underlies 'prion-like' Aβ/tau/α-syn spread
```

---

## 6. 诊断

- **RT-QuIC**(实时震荡诱导转化):高敏特异(检测种子活性)
- MRI:DWI 皮层/基底节高信号(cortical ribboning)
- EEG:周期性尖波(sCJD)
- CSF:14-3-3、tau ↑
- 确诊:脑活检/尸检(spongiform + PrPˢᶜ)

---

## 7. "Prion 样"扩散范式

- AD(Aβ、tau)、PD(α-synuclein)、ALS(TDP-43、SOD1)、Huntington(polyQ)
- 均显示**模板化错折叠 + 细胞间扩散**(Braak staging 解释)
- 但**非传染性**(无 person-to-person)→ 称"prion-like"非"prion"
- Prion 是这些蛋白病的机制原型(见 [Alzheimer](Alzheimer.md)、[Frontotemporal_Dementia](Frontotemporal_Dementia.md))

---

## 8. 物种屏障 + 株系

- 物种屏障:PrP 序列差异限跨种传播(BSE→人破例)
- **Prion strains**:同一 PrP 序列不同构象 → 不同表型(挑战"protein-only"早期质疑,现支持)
- 构象 = 信息载体(类似"蛋白质遗传")

---

## 9. 治疗 + 预防

- **无有效治疗**(100% 致死)
- 试验:doxycycline、quinacrine、PrP ASO(动物有效,人试验中)
- 预防:消除 BSE(饲料禁令)、医疗器械严格(朊病毒抗常规灭菌 → 134°C 高压 / NaOH)
- 监测:CJD 国家监测网

---

## 10. Common Pitfalls

### 10.1 朊病毒是病毒

无核酸;是错折叠蛋白(名字误导)。

### 10.2 常规灭菌可杀

抗标准高压/化学;需特殊程序(134°C 延长 / 强碱)。

### 10.3 AD/PD 是传染性朊病毒病

是"prion-like"机制,但**不**人际传染。

### 10.4 都是吃出来的

多数 sCJD 是自发;vCJD/kuru 才与摄入相关。

### 10.5 Protein-only 已无争议

广泛接受,但 strain 现象等细节仍研究。

---

## 11. Related Concepts

- **同节**:[Alzheimer](Alzheimer.md)、[Frontotemporal_Dementia](Frontotemporal_Dementia.md)、[ALS](ALS.md)、[Parkinson](Parkinson.md)
- **解剖**:[Thalamus](../01_Neuroanatomy/Thalamus.md)(FFI)
- **系统**:[Sleep_Wake](../03_Systems_Neuroscience/Sleep_Wake.md)(FFI)

---

## References

1. **Prusiner, S. B.** "Prions." *PNAS*, 1998 (Nobel Lecture).
2. **Collinge, J.** "Prion diseases of humans and animals: their causes and molecular basis." *Annu Rev Neurosci*, 2001.
3. **Jucker, M. & Walker, L. C.** "Self-propagation of pathogenic protein aggregates in neurodegenerative diseases." *Nature*, 2013.
4. **Atarashi, R. et al.** "Real-time quaking-induced conversion (RT-QuIC)." *Nat Med*, 2011.
