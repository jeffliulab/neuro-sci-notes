# 第二信使 (Second Messengers & Signal Transduction)

> *细胞外信号(第一信使:递质/激素)→ 受体 → **第二信使**(cAMP、Ca²⁺、IP3、DAG、cGMP、NO)→ 级联放大 → 通道/酶/基因。是 metabotropic 信号、可塑性、长期记忆(CREB)、神经调质的分子基础。Sutherland(cAMP)+ Kandel(海兔学习)Nobel 工作核心。*
>
> **难度**:Advanced
> **前置知识**:[Neurotransmitter_Receptors](Neurotransmitter_Receptors.md)、[LTP_LTD](LTP_LTD.md)

---

## 1. 信号级联

```
第一信使(递质/激素,胞外)
   ↓ 受体(GPCR / RTK)
G 蛋白 / 酶
   ↓
第二信使(cAMP/Ca²⁺/IP3/DAG/cGMP)
   ↓ 激酶(PKA/PKC/CaMKII)
磷酸化:通道、受体、酶、转录因子(CREB)
   ↓
短期(分钟)+ 长期(基因表达,小时-天)
```

每步**放大**(1 受体 → 多 G 蛋白 → 多 cAMP → ...)。

---

## 2. 主要第二信使

| 信使 | 来源 | 下游 |
|---|---|---|
| **cAMP** | AC(腺苷酸环化酶) | PKA |
| **Ca²⁺** | 通道/IP3-R 释放 | CaMKII、calmodulin、calcineurin |
| **IP3** | PLC 切 PIP2 | 内质网 Ca²⁺ 释放 |
| **DAG** | PLC 切 PIP2 | PKC |
| **cGMP** | GC | PKG;视觉转导 |
| **NO** | nNOS(Ca²⁺ 依赖)| 可扩散逆信使 |

---

## 3. Gs/Gi/Gq 通路

- **Gs** → AC↑ → cAMP↑ → PKA(如 β 肾上腺素、D1)
- **Gi** → AC↓ → cAMP↓;或开 GIRK(K⁺)(如 D2、μ-opioid、GABA-B)
- **Gq** → PLC → IP3 + DAG → Ca²⁺ + PKC(如 mGluR1/5、α1)

→ 同信使不同通路 → 多样效应(见 [Neurotransmitter_Receptors](Neurotransmitter_Receptors.md))。

---

## 4. PyTorch — 级联放大

```python
import torch

def signaling_cascade(ligand, gain_per_step=8.0, n_steps=4):
    """Each tier amplifies: receptor→G→cAMP→PKA→targets."""
    signal = ligand
    trace = [signal]
    for _ in range(n_steps):
        signal = torch.tanh(gain_per_step * signal)   # saturating amplification
        trace.append(signal)
    return trace   # tiny ligand -> large downstream (sensitivity + saturation)
```

---

## 5. Ca²⁺ — 通用第二信使

- 静息胞内 ~100 nM,信号时 ↑10-100×(陡梯度 → 高 SNR)
- 来源:电压/配体门控通道 + 内质网(IP3-R/RyR)释放
- 解码器:CaMKII(LTP)、calcineurin(LTD)— 幅度/时程依赖双向
- 缓冲/泵快速复位 → 时空微域(microdomain)

---

## 6. 与学习记忆 — CREB

- 持续/强信号 → PKA/CaMKII 入核 → 磷酸化 **CREB**
- CREB → 转录"晚期"基因 → 突触结构改变 → **长期记忆**
- 短期记忆(磷酸化,不需转录)vs 长期(需转录/翻译)
- Kandel 海兔(Aplysia)经典(Nobel 2000;见 [LTP_LTD](LTP_LTD.md))

---

## 7. 逆信使 + 扩散

- **NO / 内源大麻素**:突触后产生 → 逆向作用突触前(调释放)
- 体积传递(volume transmission):非突触局部扩散
- 解释 LTP 的突触前成分 + 异突触可塑

---

## 8. 药理 / 临床

- **咖啡因**:磷酸二酯酶(PDE)抑 → cAMP↑(部分机制)
- **西地那非**:PDE5 抑 → cGMP↑
- **锂(双相)**:抑 IMPase / GSK-3β(肌醇/cAMP 通路假说)
- 霍乱毒素(Gs 锁定)、百日咳毒素(Gi 失活)= 经典工具
- 信号通路异常 → 多种疾病

---

## 9. 与 AI

- 级联放大 ↔ 非线性增益 + 灵敏度调
- cAMP/Ca²⁺ "慢变量" ↔ 慢权重 / 资格迹(eligibility trace)
- 三因子可塑性的"第三因子"= 神经调质 → 第二信使(见 [Synaptic Plasticity Models](../05_Computational_Neuroscience/Synaptic_Plasticity_Models.md))
- 短期(磷酸化)vs 长期(转录)↔ fast/slow weights

---

## 10. Common Pitfalls

### 10.1 第二信使 = 慢且次要

是 metabotropic + 可塑 + 长期记忆核心(放大 + 持久)。

### 10.2 Ca²⁺ 单一效应

幅度/时程/位置依赖双向(CaMKII↑LTP vs calcineurin↑LTD)。

### 10.3 cAMP 只兴奋

依下游;Gi 降 cAMP → 抑制。

### 10.4 短期 = 长期同机制

短期=磷酸化(无需转录);长期需基因表达(CREB)。

### 10.5 信号线性

每级放大 + 饱和 + 交叉对话(crosstalk),高度非线性。

---

## 11. Related Concepts

- **同节**:[Neurotransmitter_Receptors](Neurotransmitter_Receptors.md)、[LTP_LTD](LTP_LTD.md)、[Synapse](Synapse.md)、[Ion_Channels](Ion_Channels.md)
- **计算**:[Synaptic Plasticity Models](../05_Computational_Neuroscience/Synaptic_Plasticity_Models.md)
- **疾病**:[Bipolar_Disorder](../08_Neuro_Disorders/Bipolar_Disorder.md)(锂)

---

## References

1. **Sutherland, E. W.** "Studies on the mechanism of hormone action (cAMP)." *Science*, 1972 (Nobel).
2. **Kandel, E. R.** "The molecular biology of memory storage." *Science*, 2001.
3. **Greengard, P.** "The neurobiology of slow synaptic transmission." *Science*, 2001.
4. **Berridge, M. J.** "Inositol trisphosphate and calcium signalling." *Nature*, 1993.
