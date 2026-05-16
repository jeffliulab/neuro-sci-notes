# 神经营养因子 (Neurotrophins)

> *Neurotrophins(NGF、BDNF、NT-3、NT-4)是调控神经元存活、生长、分化、突触可塑的分泌蛋白。Levi-Montalcini 发现 NGF(1986 Nobel)。"Neurotrophic 假说":靶源性、限量竞争 → 发育期程序性死亡。BDNF/TrkB 是 LTP、学习、抗抑郁的核心分子。*
>
> **难度**:Intermediate
> **前置知识**:[Neurodevelopment](../00_Foundations/Neurodevelopment.md)、[LTP_LTD](LTP_LTD.md)

---

## 1. 家族 + 受体

| 神经营养因子 | 高亲和受体(Trk) | 低亲和 |
|---|---|---|
| NGF | TrkA | p75NTR |
| **BDNF** | TrkB | p75NTR |
| NT-3 | TrkC(也 TrkA/B) | p75NTR |
| NT-4 | TrkB | p75NTR |

- Trk:受体酪氨酸激酶 → 存活/生长(PI3K-Akt、MAPK、PLCγ)
- **p75NTR**:可促凋亡(尤 proNT 形式)→ "yin-yang"

---

## 2. Neurotrophic 假说

- 靶组织分泌**限量** NGF → 神经元竞争
- 获足量 → 存活;不足 → **程序性凋亡**
- 解释发育期 ~50% 神经元死亡(匹配 input-target)
- Hamburger & Levi-Montalcini 经典(见 [Neurodevelopment](../00_Foundations/Neurodevelopment.md))

---

## 3. 逆向运输信号

- 轴突末端摄取 NGF → **逆向**运输(dynein)→ 胞体核
- "Signaling endosome":携活化 Trk 的内体
- 远距存活信号(末端 → 核,数 cm)
- 见 [Axonal_Transport](Axonal_Transport.md)

---

## 4. BDNF — 可塑性核心

- 活动依赖表达(CREB 调控,见 [Second_Messengers](Second_Messengers.md))
- TrkB → 促 LTP、树突生长、新突触
- "Synaptic + cellular consolidation" 关键(长期记忆)
- Val66Met 多态 → 分泌↓ → 记忆/情绪表型(人群常见)

---

## 5. PyTorch — 限量竞争存活

```python
import torch

def neurotrophic_competition(n_neurons=100, ngf_supply=30.0):
    """Neurons compete for limited target NGF; insufficient -> apoptosis."""
    uptake = torch.rand(n_neurons)
    uptake = uptake / uptake.sum() * ngf_supply       # share fixed supply
    survival_threshold = 0.2
    survives = uptake > survival_threshold
    return survives.sum().item()   # only well-supplied neurons survive
```

---

## 6. proNT vs 成熟型("yin-yang")

- **proBDNF**(前体)→ p75NTR → 促凋亡 / LTD
- **mBDNF**(成熟,蛋白酶切)→ TrkB → 存活 / LTP
- 切割平衡(plasmin/MMP)= 双向开关
- 解释同分子相反效应

---

## 7. 临床

- **抑郁**:BDNF↓(应激/海马);抗抑郁/运动/ECT → BDNF↑("神经营养假说",见 [Depression](../08_Neuro_Disorders/Depression.md))
- **神经退行**:NGF/BDNF↓(AD、HD、PD)→ 营养因子治疗试验(递送难,见 [Gene_Therapy_CNS](../07_Neurotech_Frontiers/Gene_Therapy_CNS.md))
- **疼痛**:NGF → 痛敏 → 抗 NGF 抗体(tanezumab)镇痛
- 周围神经再生(施万细胞分泌)

---

## 8. 与运动 / 环境

- 有氧运动 → BDNF↑(海马)→ 认知/情绪获益(见 [Cognitive_Aging](../04_Cognitive_Neuroscience/Cognitive_Aging.md))
- 环境富化 → 营养因子 + 神经发生
- "Exercise as medicine"的分子中介

---

## 9. 与 AI

- 限量竞争存活 ↔ 神经元/连接的"使用驱动"保留(类 pruning + 资源竞争,见 [Neurodevelopment](../00_Foundations/Neurodevelopment.md))
- 活动依赖 BDNF ↔ 活动门控可塑(三因子,见 [Synaptic Plasticity Models](../05_Computational_Neuroscience/Synaptic_Plasticity_Models.md))
- 营养因子 = 全局/局部调质标量

---

## 10. Common Pitfalls

### 10.1 神经营养因子只管发育

终生调可塑/存活(BDNF 学习/抗抑郁核心)。

### 10.2 都促生长

p75NTR + proNT 可促**凋亡**(yin-yang)。

### 10.3 BDNF↑ 必好

过量/异位可致痛敏/癫痫;需精确调控。

### 10.4 营养因子易做药

蛋白难过 BBB + 半衰期短 + 副作用 → 递送大挑战。

### 10.5 NGF 仅交感/感觉

广泛(基底前脑胆碱能等);BDNF 更普遍。

---

## 11. Related Concepts

- **同节**:[LTP_LTD](LTP_LTD.md)、[Second_Messengers](Second_Messengers.md)、[Synapse](Synapse.md)、[Axonal_Transport](Axonal_Transport.md)
- **基础**:[Neurodevelopment](../00_Foundations/Neurodevelopment.md)、[Neuroplasticity](../00_Foundations/Neuroplasticity.md)
- **疾病**:[Depression](../08_Neuro_Disorders/Depression.md)

---

## References

1. **Levi-Montalcini, R.** "The nerve growth factor 35 years later." *Science*, 1987 (Nobel).
2. **Huang, E. J. & Reichardt, L. F.** "Neurotrophins: roles in neuronal development and function." *Annu Rev Neurosci*, 2001.
3. **Lu, B. et al.** "BDNF-based synaptic repair as a disease-modifying strategy." *Nat Rev Neurosci*, 2013.
4. **Park, H. & Poo, M.-M.** "Neurotrophin regulation of neural circuit development and function." *Nat Rev Neurosci*, 2013.
