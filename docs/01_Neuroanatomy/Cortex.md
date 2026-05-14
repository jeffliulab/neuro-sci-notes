# 大脑皮层 (Cerebral Cortex) — 6 层结构与功能分区

> *大脑皮层是 mammalian brain 进化最晚也最厚的部分。人类皮层 2-4 mm 厚,展开面积 ~ 0.25 m²,含 ~ 16 billion neurons。6 层结构 + 50+ 功能区 (Brodmann)。*
>
> **难度**:Introduction-Intermediate
> **前置知识**:[神经元](../02_Cellular_Molecular/Neuron.md)
> **后续阅读**:[Hippocampus 解剖](Hippocampus_Anatomy.md)、[Cerebellum](Cerebellum.md)

---

## 1. 总体结构

- 厚 2-4 mm
- 灰质 (cell bodies),下面是白质 (axons)
- 6 个 cell layers (新皮层 / neocortex);海马旁皮层是 3-5 层 (allocortex)
- 表面有 sulci (沟) 和 gyri (回)

---

## 2. 6 层

```
Layer 1: Molecular - 少 neuron,主 dendrite 头
Layer 2: External granular - small pyramidal + interneurons
Layer 3: External pyramidal - 大 pyramidal,主要输出到其他 cortex
Layer 4: Internal granular - thalamic input 主要接收层
Layer 5: Internal pyramidal - 大 pyramidal,输出到 subcortical (BG/SC)
Layer 6: Multiform - 多形态,反馈到 thalamus
```

### 2.1 Input / Output

- **Thalamic input**: Layer 4 (除 motor cortex)
- **Cortico-cortical**: Layer 2/3
- **Subcortical output**: Layer 5 → BG, brainstem, spinal cord
- **Cortico-thalamic**: Layer 6 → thalamus

---

## 3. Cortical Columns

Mountcastle 1957 发现:
- 同一 column (~ 0.5 mm wide) 的 neuron 编码相似 stimulus
- 是 cortex 的 **functional unit**
- 每个 column ~ 10,000 neurons

---

## 4. 功能分区 (Brodmann Areas)

Korbinian Brodmann 1909 按细胞结构分 52 区。主要功能区:

### 4.1 Sensory

- **V1** (Area 17): 视觉
- **A1** (Area 41/42): 听觉
- **S1** (Area 1/2/3): 体觉

### 4.2 Motor

- **M1** (Area 4): 主运动
- **Premotor** (Area 6): 运动规划
- **SMA** (Supplementary Motor): 内部触发动作

### 4.3 Associative

- **PFC** (Area 9/10/46): 工作记忆、决策、人格
- **Parietal** (Area 5/7): 空间注意、感觉融合
- **Temporal** (Area 20/21): 物体识别、语言

### 4.4 Language

- **Broca** (Area 44/45): 语言产生
- **Wernicke** (Area 22): 语言理解

---

## 5. 4 大脑叶

```
Frontal (额叶): 计划、决策、运动
   |
Parietal (顶叶): 空间、触觉
   |
Temporal (颞叶): 听觉、记忆、识别
   |
Occipital (枕叶): 视觉
```

---

## 6. 半球分工 (Lateralization)

- **左**: 语言 (大多数 right-handed)、逻辑、序列
- **右**: 空间、面孔、整体感知

Split-brain 实验 (Sperry 1960s) 证明两半球能独立工作 — Nobel 1981。

---

## 7. 与 AI 对比

| 维度 | Cortex | CNN/Transformer |
|---|---|---|
| Layer 数 | 6 | 10-100 |
| 内 column | 局部 column | local attention |
| 输入流 | thalamus → L4 | input → layer 1 |
| 输出 | L5 → action | output head |
| 学习 | STDP / 调制 | backprop |
| 神经元 | 16 B | up to 1T params |

---

## 8. 病理

- **Stroke**: 局部 cortex 损 → 失功能 (依赖位置)
- **Frontal lobotomy**: 历史精神病治疗,已禁
- **Cortical atrophy** (Alzheimer): 灰质减少
- **Cortical dysplasia**: 发育异常 → 癫痫

---

## 9. 测量技术

- **MRI/fMRI**: 结构 + 功能
- **DTI**: 白质 tractography
- **MEG/EEG**: 时间分辨率高
- **Calcium imaging**: 细胞分辨率,需 craniotomy
- **Single-unit recording**: 高时间空间分辨,microelectrode

---

## 10. PyTorch — Simplified Cortical Column

```python
import torch, torch.nn as nn

class CorticalColumn(nn.Module):
    """6-layer column with feedforward + feedback."""
    def __init__(self, dim=512):
        super().__init__()
        self.L4 = nn.Linear(dim, dim)  # thalamic input
        self.L2_3 = nn.Linear(dim, dim)  # cortico-cortical
        self.L5 = nn.Linear(dim, dim)  # to subcortical
        self.L6 = nn.Linear(dim, dim)  # cortico-thalamic feedback
    
    def forward(self, thal_input):
        l4 = torch.relu(self.L4(thal_input))
        l2_3 = torch.relu(self.L2_3(l4))
        l5_out = torch.tanh(self.L5(l2_3))
        l6_feedback = self.L6(l2_3)
        return l5_out, l6_feedback  # subcortical out + thalamic feedback
```

---

## 11. Common Pitfalls

### 11.1 Brodmann ≠ 功能边界

Brodmann area 是 cytoarchitectonic 分区,与精细功能边界不完全对应。

### 11.2 Cortical column 实在性

Some species (rat / cat / 猴) column 明显;人类不那么清晰。

### 11.3 Lateralization 过度简化

"右脑创造力" 等是流行误读;实际两半球都参与几乎所有任务。

### 11.4 Cortex ≠ intelligence sole

Subcortical (海马 / BG / 小脑) 同样关键。

### 11.5 Allocortex vs neocortex

海马 / olfactory 是 allocortex,只 3 层 — 与典型 6 层 neocortex 不同。

---

## 12. Related Concepts

- **同节**:[Hippocampus Anatomy](Hippocampus_Anatomy.md)、[Cerebellum](Cerebellum.md)、[Basal Ganglia](Basal_Ganglia.md)
- **细胞**:[神经元](../02_Cellular_Molecular/Neuron.md)
- **AI 对比**:https://jeffliulab.github.io/ai-notes/02_Deep_Learning/03_Computer_Vision/CNN/

---

## References

1. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
2. **Brodmann, K.** *Vergleichende Lokalisationslehre der Großhirnrinde*. 1909.
3. **Mountcastle, V. B.** "Modality and topographic properties of single neurons of cat's somatic sensory cortex." *J Neurophysiol*, 1957.
4. **Sperry, R. W.** "Cerebral Organization and Behavior." *Science*, 1961.
