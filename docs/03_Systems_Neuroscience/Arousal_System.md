# 唤醒系统 (Arousal & Neuromodulatory Systems)

> *唤醒/觉醒由脑干-基底前脑弥散调质系统控制:NE(蓝斑)、5-HT(中缝)、DA(VTA/SNc)、ACh(基底前脑/脑桥)、组胺(结节乳头)、orexin(下丘脑)。ARAS(上行网状激活系统)→ 丘脑/皮层。这些系统调"增益/状态"非"内容",影响注意、睡眠、情绪、学习。*
>
> **难度**:Intermediate
> **前置知识**:[Brainstem](../01_Neuroanatomy/Brainstem.md)、[Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.md)

---

## 1. 主要调质核团

| 系统 | 核团 | 主功能 |
|---|---|---|
| **NE** | 蓝斑(LC) | 警觉、惊讶、explore、应激 |
| **5-HT** | 中缝核 | 情绪、耐心、时间折扣 |
| **DA** | VTA/SNc | 奖赏预测误差(见 [Reward_System](Reward_System.md)) |
| **ACh** | 基底前脑/PPT-LDT | 注意、皮层激活、REM |
| **Histamine** | 结节乳头核(TMN) | 觉醒(抗组胺致困) |
| **Orexin/hypocretin** | 外侧下丘脑 | 觉醒稳定(缺 → 发作性睡病) |

---

## 2. ARAS(上行网状激活系统)

```
脑干网状结构 + 调质核团
   ↓ 两路
丘脑(非特异核)→ 皮层(同步↔去同步)
基底前盖 → 皮层(直接)
   → 觉醒 / 意识水平
```

ARAS 损 → 昏迷(见 [Brainstem](../01_Neuroanatomy/Brainstem.md)、[Thalamus](../01_Neuroanatomy/Thalamus.md))。

---

## 3. 调"状态"非"内容"

- 弥散投射(一核 → 全皮层)→ 调全局**增益/状态**
- 类比:调音量/对比度,非传递信息内容
- 影响:信噪比、可塑性、注意、explore-exploit、睡眠阶段

---

## 4. NE — Adaptive Gain (Aston-Jones & Cohen)

- **Tonic 高**:分心/explore;**Tonic 低**:困
- **Phasic**:任务相关 → 聚焦/exploit
- 倒 U(Yerkes-Dodson):中等唤醒最优表现
- LC 与决策不确定性 / 网络重置

---

## 5. PyTorch — 神经增益调制(倒 U)

```python
import torch

def arousal_gain(signal, arousal_level):
    """Neuromodulator sets gain; Yerkes-Dodson inverted-U on performance."""
    gain = 1.0 + 2.0 * torch.sigmoid(torch.tensor(arousal_level))
    output = torch.tanh(gain * signal)            # NE-like gain control
    performance = torch.exp(-((arousal_level - 1.0) ** 2) / 0.5)  # inverted-U
    return output, performance
```

---

## 6. 睡眠-觉醒切换

- **Flip-flop 模型**(Saper):觉醒核(orexin/组胺/LC...)↔ 睡眠核(VLPO,GABA)互抑 → 双稳快切换
- Orexin = 稳定器(缺 → 发作性睡病的状态不稳)
- 与 [Sleep_Wake](Sleep_Wake.md)、[Circadian_System](Circadian_System.md) 交互

---

## 7. 与注意 / 学习

- ACh:增皮层信噪比、标记"该学"(不确定性)
- NE:惊讶/新异 → 网络重置 + 可塑性窗
- DA:奖赏 → 三因子可塑(见 [Synaptic Plasticity Models](../05_Computational_Neuroscience/Synaptic_Plasticity_Models.md))
- 调质 = "何时学/学多少"的门控

---

## 8. 临床 + 药理

- **发作性睡病**:orexin 神经元丧失(自身免疫)
- **昏迷/植物状态**:ARAS/丘脑损
- **抗组胺**致困(H1 中枢)
- **兴奋剂**(咖啡因=腺苷拮抗;modafinil)增觉醒
- **谵妄**:ACh↓ + 应激;**ADHD**:NE/DA(见 [ADHD](../08_Neuro_Disorders/ADHD.md))
- 麻醉作用于这些环路

---

## 9. 与 AI

- 调质 ≈ 全局超参(learning rate、温度、explore-exploit)
- NE adaptive gain ↔ 自适应 gain / attention temperature
- ACh 不确定性 ↔ 学习率调度
- "Neuromodulated"网络(meta-learning;Doya 框架)

---

## 10. Common Pitfalls

### 10.1 调质传递信息内容

调全局状态/增益,非点对点内容(弥散投射)。

### 10.2 唤醒越高越好

倒 U(Yerkes-Dodson);过高损表现。

### 10.3 一调质一功能

多功能 + 状态/任务依赖(NE 既警觉又应激)。

### 10.4 觉醒 = 意识

觉醒是意识的**水平/能量**维度(vs 内容);可解离(植物状态:觉醒无意识)。

### 10.5 睡眠-觉醒是渐变

Flip-flop:双稳态快切换(非平滑滑变)。

---

## 11. Related Concepts

- **同节**:[Sleep_Wake](Sleep_Wake.md)、[Circadian_System](Circadian_System.md)、[Reward_System](Reward_System.md)
- **解剖**:[Brainstem](../01_Neuroanatomy/Brainstem.md)、[Thalamus](../01_Neuroanatomy/Thalamus.md)
- **认知**:[Attention](../04_Cognitive_Neuroscience/Attention.md)、[Consciousness](../04_Cognitive_Neuroscience/Consciousness.md)
- **计算**:[Synaptic Plasticity Models](../05_Computational_Neuroscience/Synaptic_Plasticity_Models.md)

---

## References

1. **Aston-Jones, G. & Cohen, J. D.** "An integrative theory of locus coeruleus-norepinephrine function: adaptive gain." *Annu Rev Neurosci*, 2005.
2. **Saper, C. B. et al.** "Sleep state switching." *Neuron*, 2010.
3. **Doya, K.** "Metalearning and neuromodulation." *Neural Netw*, 2002.
4. **Lee, S.-H. & Dan, Y.** "Neuromodulation of brain states." *Neuron*, 2012.
