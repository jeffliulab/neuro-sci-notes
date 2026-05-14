# 离子通道 (Ion Channels) — 神经膜的"晶体管"

> *离子通道是膜上的孔状蛋白,选择性让 ion 通过。脑内 ~ 400 种 channel,控制 spike / 突触 / 神经调制。Hille 1976 patch clamp 让单个 channel 可见;Nobel 1991 (Neher, Sakmann)。本篇覆盖主要 channel 类型 + 药物 / 疾病关联。*
>
> **难度**:Intermediate
> **前置知识**:[神经元](Neuron.md)、[动作电位](Action_Potential.md)
> **后续阅读**:[Hodgkin-Huxley](Hodgkin_Huxley.md)

---

## 1. Channel 分类

### 1.1 按门控

- **Voltage-gated**: V 变化触发开关 (Na+, K+, Ca²⁺ channels)
- **Ligand-gated**: NT 结合触发 (AMPA, NMDA, GABA-A)
- **Mechanically-gated**: 机械力 (听觉毛细胞, 触觉)
- **Thermal-gated**: TRP channels (温度 / 痛)
- **Light-gated**: opsins (视网膜 + optogenetics ChR2)
- **2nd messenger-gated**: cAMP (HCN), Ca²⁺-activated K

### 1.2 按选择性

- Na+, K+, Ca²⁺, Cl-, 非选择阳离子

---

## 2. Voltage-Gated Na+ Channel (Nav)

最关键 — 产生 AP rising phase。

### 2.1 结构

- α subunit (pore)
- β1-β4 subunit (调节)
- 4 个 voltage sensor

### 2.2 States

- Closed (rest): V < -55 mV
- Open: V > -55 mV → ~ 1 ms
- Inactivated: 1 ms 后,自动 inactivation gate 关 → 不能 fire (refractory)

### 2.3 亚型 (Nav 1.1-1.9)

- Nav 1.1: GABA interneuron — mutation → 癫痫 (Dravet syndrome)
- Nav 1.7: 痛觉神经 — mutation → 痛不敏 / 极痛
- TTX (河豚毒) 阻断 Nav

---

## 3. Voltage-Gated K+ Channel (Kv)

- Repolarization
- 多亚型 Kv1-Kv12
- 决定 spike width + frequency
- 4-AP 阻断 → 加宽 spike

---

## 4. Voltage-Gated Ca²⁺ Channel

- N-, P/Q-, L-, T-type
- 突触 NT 释放需 N/P 型
- 神经元 firing pattern (burst) 需 T 型
- L-type 在心肌、平滑肌

---

## 5. NMDA Receptor

特殊 — 同时 ligand- 和 voltage-gated:

- Glu 结合 + glycine co-agonist
- V depolarized 才让 Mg²⁺ 离开 pore
- Ca²⁺ inflow ↑ → CaMKII activation → LTP

NMDA antagonists:
- Ketamine (anesthetic, antidepressant)
- PCP (street drug)
- Memantine (Alzheimer 治疗)

---

## 6. GABA-A Receptor

主要抑制 — Cl- channel + 多 binding site:
- GABA site
- Benzodiazepine site (anxiolytic)
- Barbiturate site
- Alcohol 同 site
- 麻醉剂 propofol

---

## 7. HCN Channel ("Pacemaker")

- 在 hyperpolarization 开 (反 normal voltage 门)
- 慢去极化 → 自发 firing
- 在心脏 SA node + thalamic neuron
- Ivabradine (心律不齐药) 阻断

---

## 8. PyTorch — 简化 Channel Kinetics

```python
import torch

class VoltageGatedChannel:
    """Generic 2-state V-gated channel."""
    def __init__(self, g_max, E_rev, V_half=-20, k=10):
        self.g_max = g_max
        self.E_rev = E_rev
        self.V_half, self.k = V_half, k
        self.p_open = 0
    
    def p_open_steady(self, V):
        # Boltzmann distribution
        return 1 / (1 + torch.exp(-(V - self.V_half) / self.k))
    
    def step(self, V, tau=2, dt=0.1):
        p_inf = self.p_open_steady(V)
        self.p_open += dt * (p_inf - self.p_open) / tau
        return self.g_max * self.p_open * (V - self.E_rev)
```

---

## 9. 药物 / 毒素

| 物质 | Target | 用途 |
|---|---|---|
| Tetrodotoxin (TTX) | Nav | 河豚毒 (致命) |
| Saxitoxin (STX) | Nav | 贝类毒 |
| Bicuculline | GABA-A | 实验 |
| Picrotoxin | GABA-A | 实验 |
| Ketamine | NMDA | 麻醉 / 抗抑郁 |
| Carbamazepine | Nav | 抗癫痫 |
| Verapamil | Cav (L-type) | 心血管 |

---

## 10. Channelopathies (Channel 病)

- **Epilepsy**: Nav 1.1 / KCNQ mutations
- **Long QT syndrome**: Kv 1.5 mutation
- **Migraine**: CACNA1A (Ca²⁺) mutation
- **Cystic fibrosis**: CFTR Cl- channel mutation
- **Hyperkalemic periodic paralysis**: Nav 1.4

---

## 11. 现代研究技术

- **Patch clamp**: 单 channel 电流
- **Heterologous expression**: 表达在 HEK / oocyte
- **Cryo-EM**: 原子结构
- **Optogenetics**: ChR2, Halorhodopsin, etc.
- **Calcium imaging**: Ca²⁺ activity proxy

---

## 12. Common Pitfalls

### 12.1 一种亚型不够

> 100 KV subtypes, 9 Nav subtypes。不指定 subtype 没意义。

### 12.2 In vitro ≠ in vivo

Heterologous 表达 channel 与天然环境差。

### 12.3 Multi-state

Channel 通常多 state (不只 open/closed)。

### 12.4 Pharmacology specificity

许多 "specific" 药物有 off-target effects。

### 12.5 Stoichiometry

NMDA receptor 是 4 subunits;不同组合性质不同。

---

## 13. Related Concepts

- **同节**:[Hodgkin-Huxley](Hodgkin_Huxley.md)、[动作电位](Action_Potential.md)、[神经递质](Neurotransmitters.md)

---

## References

1. **Hille, B.** *Ion Channels of Excitable Membranes*. 3rd ed., Sinauer, 2001.
2. **Neher, E. & Sakmann, B.** "Single-channel currents recorded from membrane of denervated frog muscle fibres." *Nature*, 1976.
3. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
