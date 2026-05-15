# 树突 (Dendrites)

> *Dendrites 是 neuron 信号输入器,非 passive cable。具 dendritic spike、NMDA spike、Ca²⁺ plateau、active conductance。Pyramidal neuron 一个 dendrite 数千 synapse。Spine 是 plasticity 关键位置。"Dendritic computation" 表明 single neuron 可执行复杂 computation,挑战 point-neuron 假设。*
>
> **难度**:Advanced
> **前置知识**:[Neuron](Neuron.md)、[Synapse](Synapse.md)、[Action Potential](Action_Potential.md)

---

## 1. 形态分类

```
皮层 pyramidal:
        apical dendrite ──┐
                          ├── tuft branches
        soma  ──┐         │
                ├── basal dendrites
        axon  ──┘
```

- **Pyramidal cell**: apical + basal,数千 spine
- **Stellate cell**: 多 radial branch
- **Purkinje cell**: 极复杂分支(~ 200,000 spine)
- **Granule cell**: 简

---

## 2. Passive Cable Theory (Rall 1960)

- Cable equation:$\tau \partial V / \partial t = \lambda^2 \partial^2 V / \partial x^2 - V$
- **Length constant** $\lambda$: 信号衰减距
- **Time constant** $\tau = R_m C_m$
- 远端 input attenuate(典型 10×)

---

## 3. Active Dendritic Channels

| 通道 | 位置 | 作用 |
|---|---|---|
| Nav | 末端 | dendritic spike |
| Cav (L,N,T) | 全 | Ca²⁺ plateau |
| NMDA | spine | NMDA spike |
| HCN (Ih) | 距 soma 远 | normalize 输入 |
| Kv (A-type) | apical | shunt back-prop AP |

→ Dendrite 不是 passive,有 active computation。

---

## 4. 三种 Dendritic Spikes

### 4.1 Na+ spike

- 类似 axonal AP
- Fast、local

### 4.2 NMDA spike

- 局部 dendrite 段 cluster input 触发
- Ca²⁺ + Na+ 流入
- 持 ~ 50 ms
- 关键 for plasticity

### 4.3 Ca²⁺ plateau

- Apical tuft 处
- ~ 100 ms 长 plateau
- 与 burst firing 关联(Larkum 2013)

---

## 5. Dendritic Spines

- 0.5-2 μm 突起,~ 95% excitatory synapse
- Spine head + neck
- **LTP** → spine 增大
- **LTD** → spine 缩小
- Spine 数量代表 learning capacity
- Schizophrenia、Fragile X → spine 异常

---

## 6. Coincidence Detection

- Pyramidal:Apical tuft + basal 同时 active → strong output
- Larkum's "BAC firing"
- Linked to perception(cortical column)

---

## 7. PyTorch — Multi-Compartment Toy

```python
import torch

class MultiCompartmentNeuron(torch.nn.Module):
    """3 compartments: distal dendrite, proximal dendrite, soma."""
    def __init__(self):
        super().__init__()
        # Coupling between compartments
        self.g_distal_proximal = 0.5
        self.g_proximal_soma = 0.8
    
    def forward(self, V_distal_input, V_proximal_input, dt=0.1, T=100):
        V_d, V_p, V_s = -70.0, -70.0, -70.0
        spikes = []
        for t in range(T):
            # Distal updates with input
            V_d = V_d + dt * (-V_d - 70 + V_distal_input - self.g_distal_proximal * (V_d - V_p))
            V_p = V_p + dt * (-V_p - 70 + V_proximal_input - self.g_proximal_soma * (V_p - V_s) + self.g_distal_proximal * (V_d - V_p))
            V_s = V_s + dt * (-V_s - 70 + self.g_proximal_soma * (V_p - V_s))
            if V_s > -55:
                spikes.append(t)
                V_s = -70
        return spikes
```

---

## 8. Single Neuron as Deep Network (Beniaguev 2021)

- 用 deep CNN 拟合单 pyramidal 神经元 → 需要 5-8 层 CNN
- 暗示:single biological neuron 已等同 mini-network
- 挑战 "neuron = perceptron" 简化

---

## 9. Synaptic Integration Modes

- **Linear**: distant input,passive
- **Sublinear**: nearby input,shunt
- **Supralinear**: dendritic spike,boost
- Real neuron 三者结合

---

## 10. AI Connection

- **Capsule Networks (Hinton)**: 与 dendritic compartment 类似
- **Drinov & Mel 2003**: dendrite ~ 2-layer NN
- 模拟 dendrite 可让 ANN 更 efficient

---

## 11. Common Pitfalls

### 11.1 Dendrite = Passive Cable

错;active channels 显著改变 integration。

### 11.2 NMDA = Glutamate receptor

NMDA 是 glutamate receptor,但 dendrite-level NMDA spike 涉 cluster cluster activation,非 single synapse。

### 11.3 Spine = ImmutableElevation

错;spine 在 minute 时间尺度形成 / 消除。

### 11.4 Point Neuron 模型够

ANN ReLU 完全忽略 dendritic computation。

### 11.5 Pyramidal 唯一

Cerebellum Purkinje、basket 细胞 dendritic 同样复杂。

---

## 12. Related Concepts

- **同节**:[Neuron](Neuron.md)、[Synapse](Synapse.md)、[Action Potential](Action_Potential.md)、[LTP_LTD](LTP_LTD.md)
- **解剖**:[Cortex](../01_Neuroanatomy/Cortex.md)
- **计算**:[Hopfield Networks](../05_Computational_Neuroscience/Hopfield_Networks.md)

---

## References

1. **Rall, W.** "Theory of physiological properties of dendrites." *Ann NY Acad Sci*, 1962.
2. **London, M. & Häusser, M.** "Dendritic computation." *Annu Rev Neurosci*, 2005.
3. **Larkum, M. E.** "A cellular mechanism for cortical associations." *Trends Neurosci*, 2013.
4. **Beniaguev, D., Segev, I., London, M.** "Single cortical neurons as deep artificial neural networks." *Neuron*, 2021.
