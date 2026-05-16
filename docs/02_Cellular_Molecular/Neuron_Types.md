# 神经元类型 (Neuron Types & Cell Taxonomy)

> *神经元非单一:形态(锥体/星形/Purkinje)× 神经化学(谷氨酸能/GABA能)× 电生理(regular/fast-spiking)× 转录组(单细胞 RNA-seq)多维分类。兴奋(~80%)vs 抑制中间神经元(~20%,PV/SST/VIP 三大类)。BICCN 细胞普查 → 数千类型。是理解回路 + AI"神经元≠perceptron"的基础。*
>
> **难度**:Intermediate
> **前置知识**:[Neuron](Neuron.md)、[Dendrites](Dendrites.md)

---

## 1. 多维分类

| 维度 | 例 |
|---|---|
| **形态** | 锥体、星形(stellate)、Purkinje、篮状、granule |
| **神经化学** | 谷氨酸能(兴奋)、GABA能(抑制)、胆碱能、单胺能 |
| **电生理** | regular-spiking、fast-spiking、bursting、accommodating |
| **投射** | 投射神经元(长程)vs 局部中间神经元 |
| **转录组** | scRNA-seq 标记基因聚类(BICCN) |

→ 现代:多模态(Patch-seq)整合分型。

---

## 2. 兴奋 vs 抑制

- **兴奋(~80%)**:谷氨酸能,多为锥体(皮层)/ granule
- **抑制(~20%)**:GABA能中间神经元,**多样性极高**
- E/I 平衡是回路稳定核心(失衡 → 癫痫/自闭/SCZ)

---

## 3. 三大中间神经元类(皮层)

| 类型 | 标记 | 靶 + 功能 |
|---|---|---|
| **PV**(parvalbumin) | basket/chandelier | 胞体/AIS;快抑制、gamma 振荡 |
| **SST**(somatostatin) | Martinotti | 树突;调输入整合 |
| **VIP** | — | 抑 SST → **去抑制**(gating,见 [Neural Circuits](../00_Foundations/Neural_Circuits.md)) |

→ "Canonical" 抑制微环路。

---

## 4. 标志性投射神经元

- **皮层锥体**:L5(皮层下投射)、L2/3(皮层内)— 见 [Cortex](../01_Neuroanatomy/Cortex.md)
- **Purkinje**:小脑唯一输出,GABA,巨大树突(见 [Cerebellum](../01_Neuroanatomy/Cerebellum.md))
- **海马 CA1/CA3 锥体、DG granule**
- **纹状体 MSN**(GABA,见 [Basal_Ganglia](../01_Neuroanatomy/Basal_Ganglia.md))
- **多巴胺(SNc/VTA)、5-HT(中缝)、运动神经元(α/γ)**

---

## 5. PyTorch — 电生理"指纹"分类

```python
import torch

def classify_firing_type(spike_train, dt=1.0):
    """Crude e-phys typing from ISI statistics (RS vs FS vs bursting)."""
    isi = torch.diff(torch.nonzero(spike_train).squeeze().float()) * dt
    cv = isi.std() / (isi.mean() + 1e-6)        # ISI coefficient of variation
    rate = spike_train.sum() / (len(spike_train) * dt) * 1000
    if rate > 50 and cv < 0.4:  return "fast-spiking (PV-like)"
    if cv > 1.0:                return "bursting"
    return "regular-spiking (pyramidal-like)"
```

---

## 6. 单细胞转录组革命

- scRNA-seq:按基因表达无监督聚类 → 数千"transcriptomic types"
- **BICCN/BRAIN Initiative Cell Census**:小鼠/人皮层细胞图谱
- **Patch-seq**:电 + 形态 + 转录组同细胞(见 [Patch_Clamp](../07_Neurotech_Frontiers/Patch_Clamp.md))
- 争议:类型是离散还是连续谱?

---

## 7. 类型 = 功能特异

- 不同类型 → 不同回路角色(PV gamma vs SST 树突 gating)
- 疾病细胞特异:PV 中间神经元 → SCZ(见 [Schizophrenia](../08_Neuro_Disorders/Schizophrenia.md));DA → PD;运动神经元 → ALS
- 细胞型特异工具(Cre-line)= 现代因果实验基础(见 [Optogenetics_Advanced](../07_Neurotech_Frontiers/Optogenetics_Advanced.md))

---

## 8. 与 AI

- "Neuron = perceptron" 极度简化:生物有数千类型 × dendritic 计算(见 [Dendrites](Dendrites.md))
- 兴奋/抑制 + 去抑制 ↔ gating / 归一化(见 [Normalization Models](../05_Computational_Neuroscience/Normalization_Models.md))
- 细胞类型多样性 = 计算多样性来源(ANN 单元同质)

---

## 9. 演化保守

- 主要类型(锥体/PV/SST/VIP)跨哺乳保守
- 转录组定义类型部分跨物种(也有人特异扩展)
- 见 [Comparative Neuroscience](../00_Foundations/Comparative_Neuroscience.md)

---

## 10. Common Pitfalls

### 10.1 神经元同质

数千类型 × 多维;功能高度特异。

### 10.2 抑制神经元单一

≥ 三大类(PV/SST/VIP)+ 多亚型,功能各异。

### 10.3 形态决定一切

需多模态(形态 + 电 + 转录组)联合定型。

### 10.4 类型离散明确

部分连续谱;离散 vs 连续仍争论。

### 10.5 = perceptron

忽略类型多样 + dendritic + E/I/去抑制结构。

---

## 11. Related Concepts

- **同节**:[Neuron](Neuron.md)、[Dendrites](Dendrites.md)、[Synapse](Synapse.md)
- **解剖**:[Cortex](../01_Neuroanatomy/Cortex.md)、[Cerebellum](../01_Neuroanatomy/Cerebellum.md)、[Basal_Ganglia](../01_Neuroanatomy/Basal_Ganglia.md)
- **基础**:[Neural Circuits](../00_Foundations/Neural_Circuits.md)、[Comparative Neuroscience](../00_Foundations/Comparative_Neuroscience.md)
- **前沿**:[Patch_Clamp](../07_Neurotech_Frontiers/Patch_Clamp.md)

---

## References

1. **Zeng, H. & Sanes, J. R.** "Neuronal cell-type classification: challenges, opportunities and the path forward." *Nat Rev Neurosci*, 2017.
2. **Tasic, B. et al.** "Shared and distinct transcriptomic cell types across neocortical areas." *Nature*, 2018.
3. **Tremblay, R. et al.** "GABAergic interneurons in the neocortex: from cellular properties to circuits." *Neuron*, 2016.
4. **BICCN** "A multimodal cell census and atlas of the mammalian primary motor cortex." *Nature*, 2021.
