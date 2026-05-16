# 丘脑皮层系统 (Thalamocortical System)

> *丘脑-皮层环路是感觉中继 + 皮层节律 + 注意门控 + 意识的核心。First-order(感觉中继)vs higher-order(皮层-皮层中转)。网状核(TRN)= GABA "注意闸门"。睡眠纺锤波/慢波/觉醒去同步均源于此环路。双侧损 → 昏迷。是 [Thalamus](../01_Neuroanatomy/Thalamus.md) 的功能/计算视角。*
>
> **难度**:Advanced
> **前置知识**:[Thalamus](../01_Neuroanatomy/Thalamus.md)、[Brain Rhythms](../00_Foundations/Brain_Rhythms.md)

---

## 1. 环路骨架

```
皮层 L6 ──(corticothalamic, 调制)──→ 丘脑中继核
                                        ↓ (L4)
皮层 L4 ←──(thalamocortical, 驱动)── 丘脑
        网状核(TRN, GABA)包绕 → 抑制丘脑中继(门控)
```

- 皮层 → 丘脑反馈 ≫ 丘脑 → 皮层(数量上)→ 丘脑被皮层强烈调制

---

## 2. First-order vs Higher-order(Sherman & Guillery)

| | First-order | Higher-order |
|---|---|---|
| 驱动输入 | 外周(视/听/体感) | 皮层 L5 |
| 例 | LGN、VPL、MGN | Pulvinar、MD |
| 作用 | 感觉中继入皮层 | 皮层区间"中转"通信 |

→ 丘脑不仅"门",还是皮层-皮层**通信中枢**。

---

## 3. 网状核 (TRN) — 注意闸门

- 薄壳 GABA 核,包绕丘脑
- 接收皮层 + 丘脑侧支 → 抑制中继核
- "Attentional searchlight"(Crick 1984):增强被注意通道,抑制其余
- TRN 失调 → 注意/感觉门控缺陷(精神分裂假说)

---

## 4. 两种放电模式

- **Tonic**(觉醒):线性中继(忠实传感觉)
- **Burst**(睡眠/低唤醒):T 型 Ca²⁺ 通道 → 簇放电(检测新异/唤醒触发)
- 模式切换由膜电位(调质 + TRN)决定 → 状态依赖中继

---

## 5. PyTorch — 丘脑皮层节律(简化)

```python
import torch

def thalamocortical_loop(T=1000, dt=0.5, trn_inhib=1.2):
    """Cortex (E) ↔ thalamus relay (R) ↔ TRN (I) → spindle-like rhythm."""
    E, R, I = 0.1, 0.1, 0.0
    out = []
    for _ in range(T):
        E += dt/10 * (-E + torch.sigmoid(torch.tensor(R - 0.3)))
        R += dt/10 * (-R + torch.sigmoid(torch.tensor(E - trn_inhib*I)))
        I += dt/10 * (-I + torch.sigmoid(torch.tensor(E + R - 0.5)))
        out.append((float(E), float(R), float(I)))
    return out   # E-R-I loop generates oscillation (spindle/alpha-like)
```

---

## 6. 节律生成

- **Sleep spindles**(11-15 Hz):TRN 起搏 + 中继核 + 皮层(NREM 记忆巩固,见 [Sleep_Wake](Sleep_Wake.md))
- **Slow oscillation**(< 1 Hz):皮层主导,丘脑参与
- **Alpha**(8-13 Hz):丘脑皮层(枕,抑制/注意)
- **Absence seizure**(3 Hz spike-wave):丘脑皮层环路病理同步(见 [Epilepsy](../08_Neuro_Disorders/Epilepsy.md))

---

## 7. 注意门控

- 注意 = 选择性增强相关丘脑通道(pulvinar + TRN)
- Pulvinar:皮层区间注意协调(McAlonan、Saalmann)
- 与 normalization / attention 模型(见 [Normalization Models](../05_Computational_Neuroscience/Normalization_Models.md))

---

## 8. 意识

- 双侧丘脑(尤板内核 + higher-order)损 → 昏迷/植物
- 丘脑皮层环路 = 意识 NCC 候选(Crick-Koch;Llinás 40 Hz)
- DBS 中央丘脑 → 部分意识恢复(微意识态)
- 与 [Consciousness](../04_Cognitive_Neuroscience/Consciousness.md)、[Arousal_System](Arousal_System.md)

---

## 9. 与 AI

- 丘脑 = "中央路由/门控" ↔ attention routing、gating、mixture-of-experts router 类比
- Higher-order 丘脑中转 ↔ 模块间通信瓶颈(类 global workspace)
- TRN searchlight ↔ top-down attention mask
- 但生物丘脑远复杂于简单 router

---

## 10. Common Pitfalls

### 10.1 丘脑 = 被动中继

被皮层强反馈调制 + 门控 + higher-order 通信;非被动。

### 10.2 仅感觉中继

Higher-order 核做皮层-皮层中转;参注意/意识。

### 10.3 Burst = 病理

Burst 是正常睡眠/新异检测模式(非仅癫痫)。

### 10.4 TRN 无关紧要

TRN 是注意/门控/纺锤波关键(失调 → 精神病/注意)。

### 10.5 节律 = 副产品

Spindle 等有功能(记忆巩固);非 epiphenomenon。

---

## 11. Related Concepts

- **同节**:[Arousal_System](Arousal_System.md)、[Sleep_Wake](Sleep_Wake.md)、[Visual_System](Visual_System.md)
- **解剖**:[Thalamus](../01_Neuroanatomy/Thalamus.md)、[Cortex](../01_Neuroanatomy/Cortex.md)
- **基础**:[Brain Rhythms](../00_Foundations/Brain_Rhythms.md)
- **认知**:[Attention](../04_Cognitive_Neuroscience/Attention.md)、[Consciousness](../04_Cognitive_Neuroscience/Consciousness.md)
- **疾病**:[Epilepsy](../08_Neuro_Disorders/Epilepsy.md)

---

## References

1. **Sherman, S. M. & Guillery, R. W.** *Exploring the Thalamus and Its Role in Cortical Function*. 2nd ed., 2006.
2. **Crick, F.** "Function of the thalamic reticular complex: the searchlight hypothesis." *PNAS*, 1984.
3. **Saalmann, Y. B. & Kastner, S.** "Cognitive and perceptual functions of the visual thalamus." *Neuron*, 2011.
4. **Steriade, M. et al.** "Thalamocortical oscillations in the sleeping and aroused brain." *Science*, 1993.
