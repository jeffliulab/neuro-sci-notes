# 突触囊泡循环 (Synaptic Vesicle Cycle)

> *化学突触传递的核心:囊泡装载 → 停靠(docking)→ 预备(priming)→ Ca²⁺ 触发融合(exocytosis)→ 内吞回收。SNARE 蛋白(syntaxin/SNAP-25/synaptobrevin)是融合机器,synaptotagmin 是 Ca²⁺ 传感器。2013 Nobel(Südhof/Rothman/Schekman)。量子释放(Katz)是基础。肉毒/破伤风毒素切 SNARE。*
>
> **难度**:Advanced
> **前置知识**:[Synapse](Synapse.md)、[Action_Potential](Action_Potential.md)

---

## 1. 循环步骤

```
1. 装载(transporter 充递质)
2. 转位 + 停靠(docking,活动区)
3. 预备(priming,SNARE 部分组装)
4. AP → Ca²⁺ 内流(P/Q/N 型通道)
5. Synaptotagmin 感 Ca²⁺ → SNARE 拉合 → 融合(< 1 ms)
6. 递质释放入裂隙
7. 内吞回收(clathrin / kiss-and-run)
8. 再装载
```

---

## 2. SNARE 融合机器

- **t-SNARE**:syntaxin-1 + SNAP-25(质膜)
- **v-SNARE**:synaptobrevin/VAMP(囊泡)
- 四螺旋束"拉链"式组装 → 拉近膜 → 融合
- NSF/SNAP 解旋复用
- 肉毒(BoNT)/破伤风毒素 = 蛋白酶切 SNARE → 阻释放

---

## 3. Synaptotagmin — Ca²⁺ 传感器

- C2 结构域结合 Ca²⁺ → 触发快同步释放
- 低亲和、快(匹配 ms 级 Ca²⁺ 微域)
- 敲除 → 失同步快释放(异步残留)
- 决定"为何 Ca²⁺ 进来就释放"

---

## 4. 量子释放(Katz)

- 递质以**量子**(单囊泡 ~ 数千分子)释放
- mEPP(微小终板电位)= 单囊泡自发
- EPP = 整数倍 mEPP(量子叠加)
- Pr(释放概率)× N(位点)× q(量子大小)= 突触强度

---

## 5. PyTorch — 量子释放(二项)

```python
import torch

def quantal_release(n_sites=10, p_release=0.3, q=1.0, trials=1000):
    """Binomial vesicle release; EPSP = k * q (Katz)."""
    k = torch.distributions.Binomial(n_sites, p_release).sample((trials,))
    epsp = k * q
    # Short-term plasticity: Pr changes (facilitation/depression)
    return epsp.mean(), epsp.var()   # mean & variance → quantal analysis
```

---

## 6. 释放概率 + 短期可塑

- **Facilitation**:残余 Ca²⁺ → 短暂 Pr↑(几十 ms)
- **Depression**:囊泡耗尽 → Pr↓
- 高 Pr 突触易抑制;低 Pr 易易化
- 短期可塑 = 动态滤波器(高/低通,见 [Synapse](Synapse.md))

---

## 7. 内吞回收

- **Clathrin 介导**:经典,慢(~ 10 s)
- **Kiss-and-run**:瞬开瞬闭,快回收
- **Bulk endocytosis**:高活动时
- 回收维持持续传递(囊泡池有限)
- 囊泡池:readily releasable / recycling / reserve

---

## 8. 临床

- **肉毒毒素(Botox)**:切 SNAP-25 → 肌松(美容/痉挛/偏头痛治疗)
- **破伤风毒素**:切 synaptobrevin(抑制性中间神经元)→ 强直
- **Lambert-Eaton**:抗 P/Q Ca²⁺ 通道 → 释放↓ → 肌无力
- **α-latrotoxin**(黑寡妇):强制释放
- 突触囊泡蛋白基因突变 → 癫痫/ID/帕金森(SYT、SV2A — 左乙拉西坦靶)

---

## 9. 与 AI

- 量子 + 随机释放 ↔ 突触噪声 / Dropout 类比(随机失活正则)
- 短期可塑 ↔ 动态权重 / 适应性增益(非静态 W)
- 有限囊泡池 ↔ 资源约束(能量/带宽,见 [Brain Energy Metabolism](../00_Foundations/Brain_Energy_Metabolism.md))

---

## 10. Common Pitfalls

### 10.1 释放是确定性

随机(量子 + 二项 Pr);单 AP 可能不释放。

### 10.2 SNARE 仅"对接"

是融合**力学机器**(拉链);非被动锚。

### 10.3 Pr 固定

短期可塑动态变(facilitation/depression)。

### 10.4 一囊泡池

至少 RRP/recycling/reserve 多池。

### 10.5 Botox 杀神经

是可逆阻 SNARE(数月恢复);非杀神经。

---

## 11. Related Concepts

- **同节**:[Synapse](Synapse.md)、[Neurotransmitters](Neurotransmitters.md)、[Action_Potential](Action_Potential.md)、[Ion_Channels](Ion_Channels.md)
- **基础**:[Brain Energy Metabolism](../00_Foundations/Brain_Energy_Metabolism.md)
- **计算**:[Synaptic Plasticity Models](../05_Computational_Neuroscience/Synaptic_Plasticity_Models.md)

---

## References

1. **Südhof, T. C.** "The molecular machinery of neurotransmitter release (Nobel Lecture)." *Angew Chem*, 2014.
2. **Katz, B.** *The Release of Neural Transmitter Substances*. 1969.
3. **Jahn, R. & Fasshauer, D.** "Molecular machines governing exocytosis of synaptic vesicles." *Nature*, 2012.
4. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
