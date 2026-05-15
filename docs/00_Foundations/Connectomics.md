# 连接组学 (Connectomics)

> *Connectome = 神经系统完整 wiring diagram。从 C. elegans 302 neuron (White 1986) 到 Drosophila 全脑 (FlyWire 2024, ~140k neuron) 到 mouse cortex 1 mm³ (MICrONS 2025)。EM + AI segmentation 是关键技术。争议:connectome 是否足以理解 function(Seung "I am my connectome" vs 批评)。*
>
> **难度**:Intermediate
> **前置知识**:[Research Methods](Research_Methods.md)、[Neuron Doctrine](Neuron_Doctrine.md)

---

## 1. 定义

- **Connectome**: 所有 neuron + 所有 synapse 的完整图
- 类比:基因组之于遗传 → connectome 之于 wiring
- 多尺度:
  - Micro:单 synapse(EM)
  - Meso:cell-type 群体
  - Macro:脑区间(DTI tractography)

---

## 2. 里程碑

| 年 | 系统 | 规模 |
|---|---|---|
| 1986 | C. elegans (White) | 302 neuron, ~7000 synapse |
| 2013 | Mouse retina | 部分 |
| 2018 | Drosophila larva | 部分 |
| 2024 | Drosophila 成虫全脑 (FlyWire) | ~140,000 neuron, ~5000万 synapse |
| 2025 | Mouse visual cortex (MICrONS) | 1 mm³, ~10万 neuron |
| 未来 | Mouse 全脑 / 人 | 仍遥远 |

---

## 3. 技术 pipeline

```
组织固定 + 染色 (heavy metal)
   ↓
Ultra-thin sectioning (~ 30 nm) 或 block-face
   ↓
Electron microscopy (nm 分辨)
   ↓ TB-PB 级数据
AI segmentation (3D U-Net, flood-filling)
   ↓
Synapse detection + proofreading
   ↓
Connectome graph
```

---

## 4. EM 类型

- **ssTEM**: serial section TEM
- **SBEM**: serial block-face EM
- **FIB-SEM**: focused ion beam(高 z 分辨)
- **ATUM**: 自动收集切片
- 数据量:mouse 全脑 ~ exabyte 级(挑战)

---

## 5. AI 在 connectomics

- **3D segmentation**: flood-filling networks (Januszewski 2018)
- **Synapse detection**: CNN
- **Proofreading**: 人 + AI 协作(FlyWire 众包)
- 没有 deep learning → connectomics 不可行(人工标注太慢)

---

## 6. PyTorch — Connectome 图分析

```python
import torch

def graph_metrics(adjacency):
    """Basic connectome graph metrics."""
    A = adjacency
    degree = A.sum(dim=1)              # 出度
    in_degree = A.sum(dim=0)           # 入度
    # Reciprocity: fraction of bidirectional edges
    recip = ((A > 0) & (A.t() > 0)).float().sum() / (A > 0).float().sum()
    # Hub: high-degree nodes
    hubs = torch.topk(degree, k=5).indices
    return degree, recip, hubs
```

---

## 7. 发现 — C. elegans

- 302 neuron 全图已知 ~ 40 年
- 但仍**不能完全预测行为**!
- 原因:connectome 缺 neuromodulator (extrasynaptic)、突触强度、动态
- 教训:wiring ≠ function

---

## 8. Connectome ≠ Function 争议

| 支持(Seung) | 批评 |
|---|---|
| Wiring 决定 computation | 缺 synaptic weight |
| 必要基础 | 缺 neuromodulation |
| 类比基因组 | 缺 dynamics + plasticity |
| 静态蓝图有价值 | C. elegans 反例 |

→ 共识:connectome 必要但不充分。

---

## 9. Macro Connectome (Human)

- **DTI tractography**: 白质纤维束(非单 synapse)
- **Human Connectome Project** (HCP): 健康 + 疾病
- 分辨率限于 mm(非 neuron 级)
- "Connectopathy":精神疾病作为 connectivity 异常

---

## 10. AI 类比

- ANN 的 weight matrix = artificial connectome
- 但 ANN 完全可见 + 可控,生物 connectome 难测
- Mechanistic interpretability ↔ "reverse-engineering connectome of LLM"

---

## 11. Common Pitfalls

### 11.1 Connectome → 理解 brain

C. elegans 反例:有图 40 年仍不全懂。

### 11.2 EM 图含 weight

EM 仅形态(突触大小约略);强度需生理。

### 11.3 DTI = neuron connectome

DTI 是 mm 级纤维束,非单 synapse。

### 11.4 静态足够

缺 plasticity + neuromodulation + dynamics。

### 11.5 人脑 connectome 即将完成

数据量(exabyte)+ 个体差异 → 仍极遥远。

---

## 12. Related Concepts

- **同节**:[Research Methods](Research_Methods.md)、[Neuron Doctrine](Neuron_Doctrine.md)、[Levels of Analysis](Levels_of_Analysis.md)
- **计算**:[Hopfield Networks](../05_Computational_Neuroscience/Hopfield_Networks.md)
- **前沿**:[Calcium Imaging](../07_Neurotech_Frontiers/Calcium_Imaging.md)

---

## References

1. **White, J. G. et al.** "The structure of the nervous system of the nematode C. elegans." *Phil Trans R Soc B*, 1986.
2. **Seung, S.** *Connectome*. HMH, 2012.
3. **Januszewski, M. et al.** "High-precision automated reconstruction of neurons with flood-filling networks." *Nat Methods*, 2018.
4. **Dorkenwald, S. et al.** "Neuronal wiring diagram of an adult brain (FlyWire)." *Nature*, 2024.
