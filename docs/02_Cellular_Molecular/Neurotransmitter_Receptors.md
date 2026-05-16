# 神经递质受体 (Neurotransmitter Receptors)

> *受体决定递质效应,而非递质本身。两大类:ionotropic(配体门控离子通道,快 ms)vs metabotropic(GPCR,慢 + 持久 + 放大)。同一递质经不同受体可兴奋或抑制(ACh:nAChR 兴奋 vs mAChR 可抑)。受体是药理学 + 精神药物的主战场。*
>
> **难度**:Intermediate
> **前置知识**:[Neurotransmitters](Neurotransmitters.md)、[Synapse](Synapse.md)

---

## 1. 两大类

| | Ionotropic | Metabotropic |
|---|---|---|
| 结构 | 配体门控离子通道 | GPCR(7 跨膜) |
| 速度 | 快(ms) | 慢(100 ms-s) |
| 持续 | 短 | 长 |
| 机制 | 直接开孔 | G 蛋白 → 第二信使级联 |
| 放大 | 无 | 有(级联放大) |
| 例 | nAChR、AMPA、NMDA、GABA-A | mAChR、mGluR、GABA-B、DA/5-HT 多数 |

---

## 2. "效应由受体定"

- 同递质 + 不同受体 → 相反效应
  - ACh:nAChR(阳离子,兴奋)vs mAChR M2(开 K⁺,抑制心)
  - Glutamate:AMPA/NMDA(兴奋)vs mGluR(调制)
  - GABA:GABA-A(Cl⁻,快抑)vs GABA-B(K⁺/Ca²⁺,慢抑)
- → 不能问"递质兴奋还是抑制",要问"哪个受体"

---

## 3. 关键 ionotropic

| 受体 | 配体 | 离子 |
|---|---|---|
| AMPA | Glu | Na⁺(快 EPSP) |
| NMDA | Glu | Ca²⁺(Mg²⁺ 电压门控,巧合检测器,LTP) |
| GABA-A | GABA | Cl⁻(快 IPSP;苯二氮䓬位点) |
| Glycine-R | Gly | Cl⁻(脊髓抑制) |
| nAChR | ACh | Na⁺/Ca²⁺ |
| 5-HT3 | 5-HT | 阳离子(止吐靶) |

---

## 4. NMDA — 巧合检测器

- 需 **glutamate 结合 + 突触后去极化**(逐 Mg²⁺ 阻塞)双重条件
- → Ca²⁺ 内流 → 触发 LTP(见 [LTP_LTD](LTP_LTD.md))
- 分子层面的 Hebbian "fire together"
- 解释关联学习 + 是 ketamine/PCP/记忆药靶

---

## 5. PyTorch — NMDA 巧合检测

```python
import torch

def nmda_current(glutamate_bound, post_voltage, mg_block_v=-40.0):
    """NMDA: needs BOTH ligand AND depolarization (Mg2+ relief)."""
    mg_relief = torch.sigmoid((post_voltage - mg_block_v) / 10.0)
    ca_influx = glutamate_bound * mg_relief        # AND gate
    return ca_influx   # high only if pre-active AND post-depolarized
```

---

## 6. Metabotropic 信号

```
配体 → GPCR → G 蛋白(Gs/Gi/Gq)
  Gs → ↑cAMP → PKA
  Gi → ↓cAMP / 开 K⁺(GIRK)
  Gq → PLC → IP3+DAG → Ca²⁺ / PKC
→ 磷酸化通道/受体/转录因子(CREB)→ 短期 + 长期效应
```

慢、放大、可塑性 + 基因表达(见 [Second_Messengers](Second_Messengers.md))。

---

## 7. 受体调控

- **脱敏**(desensitization):持续配体 → 失活
- **上/下调**:慢性药物 → 受体数变(耐受 / 戒断基础)
- **变构调节**:苯二氮䓬(GABA-A 正变构)、巴比妥
- 受体亚基组合 → 多样药理(GABA-A α 亚基 → 镇静 vs 抗焦虑)

---

## 8. 药理 — 主战场

| 药 | 受体作用 |
|---|---|
| 苯二氮䓬 | GABA-A 正变构(抗焦虑/镇静) |
| 氯胺酮 | NMDA 拮抗(麻醉/速效抗抑郁) |
| 阿片 | μ-opioid GPCR(镇痛/成瘾) |
| 抗精神病 | D2 拮抗 |
| SSRI | 间接 ↑5-HT(下游受体适应) |
| 烟碱 | nAChR 激动(成瘾) |

---

## 9. 与 AI

- Ionotropic(快)≈ ANN 即时加权;Metabotropic(慢/放大)≈ 调质/超参 + 慢权重
- NMDA 巧合检测 ↔ 乘性门控 / Hebbian + 三因子(见 [Synaptic Plasticity Models](../05_Computational_Neuroscience/Synaptic_Plasticity_Models.md))
- 受体多样性 ≫ ANN 单一激活 → 生物计算更丰富

---

## 10. Common Pitfalls

### 10.1 递质决定兴奋/抑制

由**受体**定;同递质可兴奋或抑制(ACh/Glu/GABA)。

### 10.2 Ionotropic = 全部快

NMDA 较慢 + Ca²⁺ 信号触发慢级联。

### 10.3 Metabotropic 无关紧要

调制 + 可塑 + 基因表达核心;药物主靶。

### 10.4 一受体一亚型

亚基组合多 → 药理多样(GABA-A 数十亚型)。

### 10.5 SSRI 立即起效(受体)

5-HT 升即时,但受体适应 + 下游可塑需周(临床滞后)。

---

## 11. Related Concepts

- **同节**:[Neurotransmitters](Neurotransmitters.md)、[Synapse](Synapse.md)、[Ion_Channels](Ion_Channels.md)、[LTP_LTD](LTP_LTD.md)、[Second_Messengers](Second_Messengers.md)
- **系统**:[Reward_System](../03_Systems_Neuroscience/Reward_System.md)
- **计算**:[Synaptic Plasticity Models](../05_Computational_Neuroscience/Synaptic_Plasticity_Models.md)

---

## References

1. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
2. **Traynelis, S. F. et al.** "Glutamate receptor ion channels: structure, regulation, and function." *Pharmacol Rev*, 2010.
3. **Nicoll, R. A.** "A brief history of long-term potentiation." *Neuron*, 2017.
4. **Hille, B.** *Ion Channels of Excitable Membranes*. 3rd ed., 2001.
