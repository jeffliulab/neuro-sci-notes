# 小脑 (Cerebellum) — "小大脑",运动控制 + 学习

> *小脑虽小 (脑体积 10%),但含 80% 神经元 (~ 70 billion)。功能远超 "运动协调" — 现代研究表明它参与运动学习、认知、情绪。Marr-Albus (1969) 理论是计算神经科学早期里程碑。*
>
> **难度**:Intermediate
> **前置知识**:[神经元](../02_Cellular_Molecular/Neuron.md)、[Cortex](Cortex.md)

---

## 1. 解剖

小脑位于 brain 后下方,3 部分:
- **Vermis** (中)
- **Hemispheres** (左右)
- **Flocculus** (前庭相关)

3 层皮层 (vs cortex 6 层):
- **Molecular layer**: parallel fibers + interneurons
- **Purkinje layer**: 一行 Purkinje cell bodies
- **Granular layer**: granule cells (最多)

---

## 2. 主要细胞类型

### 2.1 Purkinje Cell

- **唯一 output** 神经元
- 平面扇形 dendritic tree (200k+ synapses)
- GABAergic (抑制)
- 投射到 deep cerebellar nuclei

### 2.2 Granule Cells

- 数量最大 (~ 50 billion in 人)
- 接收 mossy fiber
- Axons → parallel fibers,穿过 molecular layer

### 2.3 Climbing Fibers

- 来自下橄榄核 (inferior olive)
- 1 climbing fiber wrap 1 Purkinje (强 1:1)
- 每次激活产生 complex spike

### 2.4 Mossy Fibers

- 来自 spinal cord, pontine nuclei, vestibular
- 在 granule cell 上 mossy 形 synapse

---

## 3. Marr-Albus 学习理论

```
Mossy fibers → Granule → Parallel fibers (~ 100k 输入 / Purkinje)
                                    ↓
Climbing fiber (error / teacher signal) → Purkinje
                                    ↓
Long-term modify parallel fiber → Purkinje synapse (LTD)
```

理论:
- Mossy fiber 携带 "状态" 信息
- Climbing fiber 是 "error signal"
- LTD 在 parallel fiber → Purkinje 上学习 = supervised learning

Marr 1969 + Albus 1971 互独立发表。现代证实许多预测。

---

## 4. 与 perceptron 关系

小脑 = 巨型 perceptron:
- Input: parallel fibers (100k)
- Output: Purkinje
- Learning: gradient descent via climbing fiber error

→ Cerebellum 是生物 supervised learning 经典案例。

---

## 5. 功能

### 5.1 Motor Control (经典)

- 时序、流畅性、协调
- Cerebellar lesion → ataxia (运动失调)
- 学新动作快(打字, 钢琴)

### 5.2 Motor Learning

- Vestibulo-ocular reflex (VOR) 适应:戴 prism 后眼动调整
- 经典 cerebellar plasticity 测试

### 5.3 Cognitive

- 近 20 年:cerebellum 参与语言、思维、情绪
- "Cerebellar Cognitive Affective Syndrome" (Schmahmann)
- Cerebellar cortex 与 prefrontal cortex closed loops

---

## 6. 病理

- **Stroke (cerebellar)**: ataxia + dysarthria
- **Friedreich's ataxia**: 遗传退行
- **Ataxia-telangiectasia**: 基因病
- **Alcohol cerebellar degeneration**: 长期酗酒
- **Autism**: cerebellar abnormalities

---

## 7. 与 AI

### 7.1 Perceptron / SVM

小脑机制启发 supervised learning 早期模型。

### 7.2 Spiking neural network

Granule + Purkinje 是 SNN 经典对象。

### 7.3 Predictive control

Cerebellum 内置 forward model — 与 MPC 异曲同工。

---

## 8. PyTorch — Cerebellum-Perceptron

```python
import torch
import torch.nn as nn

class CerebellumPerceptron(nn.Module):
    """Simplified cerebellar circuit as perceptron."""
    def __init__(self, n_parallel=100000):
        super().__init__()
        self.parallel_to_purkinje = nn.Linear(n_parallel, 1, bias=False)
        # Sparse activation
        self.granule_sparsity = 0.05
    
    def forward(self, mossy_input):
        # Granule cells: sparse coding
        granule = (mossy_input > torch.quantile(mossy_input, 1 - self.granule_sparsity)).float()
        # Purkinje output
        purkinje = self.parallel_to_purkinje(granule)
        return -purkinje  # inhibitory output
    
    def learn(self, mossy_input, climbing_signal, lr=1e-4):
        """Climbing fiber as error signal (LTD)."""
        granule = (mossy_input > torch.quantile(mossy_input, 1 - self.granule_sparsity)).float()
        # LTD: depress synapses that were active during error
        with torch.no_grad():
            self.parallel_to_purkinje.weight -= lr * climbing_signal * granule
```

---

## 9. 数字

- 体积:脑的 10%
- Neurons: ~ 70 billion (80% 的全脑)
- Purkinje cells: ~ 15 million
- Granule cells: ~ 50 billion
- Synapses: 100 trillion 级

---

## 10. Common Pitfalls

### 10.1 "只是运动" 错觉

近代认识到 cerebellum 也认知 / 情绪。

### 10.2 与 cortex 简单类比错

3 vs 6 层,wiring 大不同。

### 10.3 Marr-Albus 完整性

LTD 不是唯一可塑机制 (LTP 也存在);climbing fiber 也不只是 error。

### 10.4 损伤定位

Cerebellar lesions 影响范围与 cortex 不同 (compensate 较强)。

### 10.5 Granule cell 难记录

如此小 + 多,实验生理学非常 challenging。

---

## 11. Related Concepts

- **同节**:[Cortex](Cortex.md)、[Hippocampus Anatomy](Hippocampus_Anatomy.md)、[Basal Ganglia](Basal_Ganglia.md)
- **学习**:[LTP/LTD](../02_Cellular_Molecular/LTP_LTD.md)
- **AI 对比**:Perceptron / SVM 启发 — https://jeffliulab.github.io/ai-notes/01_AI/02_Machine_Learning/

---

## References

1. **Marr, D.** "A theory of cerebellar cortex." *J Physiol*, 1969.
2. **Albus, J. S.** "A theory of cerebellar function." *Math Biosci*, 1971.
3. **Schmahmann, J. D.** "Cerebellar cognitive affective syndrome." *Brain*, 1998.
4. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
