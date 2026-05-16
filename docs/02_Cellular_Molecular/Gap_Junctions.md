# 缝隙连接与电突触 (Gap Junctions & Electrical Synapses)

> *电突触 = connexin(脊椎)/innexin(无脊椎)构成的缝隙连接,胞质直接电耦合。特点:极快(无突触延迟)、双向、同步化。是 reticular theory "部分正确"之处(见 [Neuron_Doctrine](../00_Foundations/Neuron_Doctrine.md))。功能:快反射、振荡同步、发育、星胶网络。与化学突触互补。*
>
> **难度**:Intermediate
> **前置知识**:[Synapse](Synapse.md)、[Membrane_Potential](Membrane_Potential.md)

---

## 1. 结构

- **Connexon**(半通道)= 6 个 connexin → 两细胞对接成 gap junction 通道
- 孔径 ~ 1-2 nm:过离子 + 小分子(< 1 kDa,如 cAMP、IP3)
- 多通道成 plaque
- 无脊椎:innexin(同功能不同源)

---

## 2. 电 vs 化学突触

| | 电突触 | 化学突触 |
|---|---|---|
| 延迟 | 几乎 0 | ~ 0.5-2 ms |
| 方向 | 多双向 | 单向 |
| 增益 | 衰减(无放大) | 可放大 |
| 可塑 | 较少(有但弱) | 丰富 |
| 同步 | 强 | 弱 |
| 整流 | 部分(rectifying) | — |

---

## 3. PyTorch — 电耦合同步

```python
import torch

def electrical_coupling(v1, v2, g_gap=0.3, steps=50):
    """Gap junction current drives two cells toward synchrony."""
    traj = []
    for _ in range(steps):
        i_gap = g_gap * (v2 - v1)        # bidirectional, ohmic
        v1 = v1 + i_gap + 0.05*torch.randn(1).item()
        v2 = v2 - i_gap + 0.05*torch.randn(1).item()
        traj.append((float(v1), float(v2)))
    return traj   # voltages converge → synchronization
```

---

## 4. 功能

- **快反射 / 逃逸**:鱼 Mauthner 细胞(毫秒级逃避)
- **振荡同步**:中间神经元网络(PV 互连 → gamma 节律,见 [Brain Rhythms](../00_Foundations/Brain_Rhythms.md))
- **发育**:早期广泛电耦合(后被化学突触替代)
- **星形胶质合胞体**:astrocyte 网络经 gap junction 传 Ca²⁺ 波(见 [Astrocyte_Function](Astrocyte_Function.md))

---

## 5. 整流与调控

- 部分 gap junction **整流**(单向偏好)
- 调控:电压、pH、Ca²⁺、磷酸化、connexin 类型
- "电突触可塑性"存在但弱于化学
- 缝隙连接也存在非神经组织(心肌同步)

---

## 6. 与 Neuron Doctrine

- 电突触 = 胞质连续 → reticular theory 在此意义"部分对"
- 但大多数传递为化学突触 → Doctrine 仍主导(见 [Neuron_Doctrine](../00_Foundations/Neuron_Doctrine.md))
- 历史教训:二元对立常是连续谱

---

## 7. 临床

- **Connexin 突变**:CMTX(GJB1/Cx32 周围神经)、耳聋(GJB2/Cx26 最常见遗传性耳聋)
- 心律失常(心肌 connexin)
- 缝隙连接在缺血"旁观者效应"(bystander)— 损伤扩散
- 抗惊厥:缝隙连接阻断剂研究(同步病理)

---

## 8. 与 AI

- 双向电耦合 ↔ 对称权重 + 扩散/平滑(类 mean-field 耦合)
- 同步化 ↔ 振荡网络 / coupled oscillators
- 与化学突触互补 ↔ fast linear pathway + slow nonlinear pathway
- 多为同步而非信息变换(角色不同)

---

## 9. 发育角色

- 胚胎/早期:广泛电耦合 → 协调活动波(retinal waves 等)→ 回路精化
- 成熟:多数退化,保留于特定网络(下橄榄、TRN、中间神经元)
- 见 [Neurodevelopment](../00_Foundations/Neurodevelopment.md)

---

## 10. Common Pitfalls

### 10.1 突触 = 化学突触

电突触(gap junction)存在且功能重要(同步/快反射)。

### 10.2 电突触无方向/无调控

部分整流(单向偏好)+ 受 pH/Ca²⁺/磷酸化调。

### 10.3 电突触无可塑

有但弱于化学(非"完全固定")。

### 10.4 仅快反射用

也主振荡同步、发育、星胶网络。

### 10.5 Neuron Doctrine 全错(因电突触)

Doctrine 主导(化学突触为主);电突触是补充非推翻。

---

## 11. Related Concepts

- **同节**:[Synapse](Synapse.md)、[Membrane_Potential](Membrane_Potential.md)、[Astrocyte_Function](Astrocyte_Function.md)
- **基础**:[Neuron_Doctrine](../00_Foundations/Neuron_Doctrine.md)、[Brain Rhythms](../00_Foundations/Brain_Rhythms.md)、[Neural Circuits](../00_Foundations/Neural_Circuits.md)
- **系统**:[Thalamocortical_System](../03_Systems_Neuroscience/Thalamocortical_System.md)(TRN)

---

## References

1. **Bennett, M. V. L. & Zukin, R. S.** "Electrical coupling and neuronal synchronization in the mammalian brain." *Neuron*, 2004.
2. **Connors, B. W. & Long, M. A.** "Electrical synapses in the mammalian brain." *Annu Rev Neurosci*, 2004.
3. **Söhl, G. et al.** "Expression and functions of neuronal gap junctions." *Nat Rev Neurosci*, 2005.
4. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
