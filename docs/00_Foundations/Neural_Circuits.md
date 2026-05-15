# 神经环路 (Neural Circuits & Motifs)

> *单 neuron 不计算行为;circuit 才是功能单元。少数 motif 反复出现:feedforward、feedback、lateral inhibition、recurrent、winner-take-all、CPG (central pattern generator)。理解 motif 是连接 single-cell 与 systems 的桥。许多 motif 直接启发 ANN 架构。*
>
> **难度**:Intermediate
> **前置知识**:[Neuron](../02_Cellular_Molecular/Neuron.md)、[Synapse](../02_Cellular_Molecular/Synapse.md)

---

## 1. 为何 circuit 而非 neuron

- 单 neuron 仅 input-output 单元
- 功能涌现于**连接模式**
- 类比:晶体管 vs 电路;ANN unit vs 架构

---

## 2. 核心 motif

| Motif | 功能 |
|---|---|
| **Feedforward excitation** | 信号传递、放大 |
| **Feedforward inhibition** | 时间窗锐化 |
| **Feedback / recurrent excitation** | 持续活动、记忆 |
| **Feedback inhibition** | 稳定、增益控制 |
| **Lateral inhibition** | 对比增强、WTA |
| **Disinhibition** | gating(抑制抑制者) |
| **Convergence / divergence** | 整合 / 广播 |

---

## 3. Lateral Inhibition

- 邻近 neuron 互相抑制
- → 对比增强(Mach bands)
- Retina horizontal cell、V1 surround
- ANN 对应:softmax、normalization

---

## 4. Recurrent Excitation → Attractor

- 自激环 → 持续活动(无 input 也维持)
- Working memory(DLPFC persistent activity)
- Decision making(累积证据)
- 见 [Hopfield Networks](../05_Computational_Neuroscience/Hopfield_Networks.md)

---

## 5. Feedback Inhibition (Gain Control)

- Pyramidal → interneuron → 抑制 pyramidal
- 防止 runaway excitation(否则 → epilepsy)
- E/I balance 关键
- 失衡 → autism / schizophrenia 假说

---

## 6. Disinhibition (Gating)

- VIP interneuron 抑制 SST interneuron → 解放 pyramidal
- 注意 / 学习的门控机制
- "抑制的抑制 = 兴奋"

---

## 7. Central Pattern Generator (CPG)

- 自主产生节律输出(无需节律输入)
- 行走、呼吸、咀嚼、游泳
- 经典:lamprey 游泳 CPG、龙虾胃神经节(STG)
- 半中心模型(half-center oscillator)

---

## 8. PyTorch — Winner-Take-All Circuit

```python
import torch

def winner_take_all(x, n_iter=10, inhib=1.2, self_exc=1.0):
    """Recurrent lateral inhibition → WTA (one winner)."""
    y = x.clone()
    for _ in range(n_iter):
        total = y.sum()
        y = torch.relu(self_exc * y - inhib * (total - y) + 0.1 * x)
    return y  # converges to single active unit

print(winner_take_all(torch.tensor([0.3, 0.9, 0.5, 0.2])))
```

---

## 9. Canonical Cortical Microcircuit

- 6 层皮层有重复 motif(Douglas & Martin)
- L4 接收 thalamic input → L2/3 → L5/6 output
- Recurrent + feedforward + feedback 组合
- 假说:cortex 用同一 microcircuit "重复"于全脑

---

## 10. AI 对应

| Circuit motif | ANN |
|---|---|
| Feedforward | MLP / CNN |
| Recurrent excitation | RNN / attractor |
| Lateral inhibition | softmax / LayerNorm |
| Feedback inhibition | gating / normalization |
| Disinhibition | attention gating |
| Canonical microcircuit | Transformer block(类比) |

---

## 11. Common Pitfalls

### 11.1 单 neuron 决定行为

极少(除 command neuron);通常 circuit-level。

### 11.2 Motif 列表完备

是抽象;真实 circuit 混合 + heterogeneous。

### 11.3 Recurrent = 不稳定

需 inhibition 平衡;balanced network 稳定。

### 11.4 Cortex 一种 microcircuit

canonical 是简化;区域 + 物种有变异。

### 11.5 CPG 需感觉反馈

CPG 离体仍产节律;反馈仅 modulate。

---

## 12. Related Concepts

- **同节**:[Neural Coding](Neural_Coding.md)、[Levels of Analysis](Levels_of_Analysis.md)、[Connectomics](Connectomics.md)
- **细胞**:[Synapse](../02_Cellular_Molecular/Synapse.md)、[Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.md)
- **计算**:[Hopfield Networks](../05_Computational_Neuroscience/Hopfield_Networks.md)、[SNN](../05_Computational_Neuroscience/Spiking_Neural_Networks.md)

---

## References

1. **Douglas, R. J. & Martin, K. A. C.** "Neuronal circuits of the neocortex." *Annu Rev Neurosci*, 2004.
2. **Marder, E. & Bucher, D.** "Central pattern generators and the control of rhythmic movements." *Curr Biol*, 2001.
3. **Shepherd, G. M.** *The Synaptic Organization of the Brain*. 5th ed., 2004.
4. **Luo, L.** *Principles of Neurobiology*. 2nd ed., 2020.
