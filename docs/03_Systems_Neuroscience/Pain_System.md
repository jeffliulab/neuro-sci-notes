# 疼痛系统 (Pain System)

> *疼痛 = 感觉 + 情绪 + 认知的多维体验,非单纯"伤害信号"。Nociceptor → 脊髓背角(gate)→ 上行(外侧:感觉辨别;内侧:情绪)→ 皮层"pain matrix"。下行调制(PAG-RVM)+ 内源阿片。慢性痛是中枢敏化(神经可塑病),非组织损伤持续。*
>
> **难度**:Intermediate
> **前置知识**:[Somatosensory](Somatosensory.md)、[Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.md)

---

## 1. 痛 ≠ 伤害感受

- **Nociception**:伤害刺激的神经检测(无意识)
- **Pain**:主观多维体验(感觉 + 情绪 + 认知)
- 可分离:先天无痛症(SCN9A)有 nociceptor 却无痛;痛无伤(纤维肌痛)

---

## 2. 伤害感受器 + 纤维

| 纤维 | 髓鞘 | 速度 | 痛质 |
|---|---|---|---|
| **Aδ** | 薄髓 | 5-30 m/s | 第一痛(快、锐、定位) |
| **C** | 无髓 | 0.5-2 m/s | 第二痛(慢、钝、弥散、灼) |

Nociceptor:游离神经末梢,TRPV1(热/辣椒素)、TRPM8(冷)、ASIC(酸)、机械门控。

---

## 3. 通路

```
Nociceptor → DRG → 脊髓背角(换元 + gate)
   ↓ 交叉
Spinothalamic tract(上行)
   ├→ 外侧系统:VPL → S1/S2(感觉辨别:在哪、多强)
   └→ 内侧系统:内侧丘脑 → ACC/insula(情绪:多难受)
"Pain matrix"(非单一痛中枢)
```

---

## 4. Gate Control(Melzack & Wall 1965)

- 脊髓背角"闸门":Aβ(触觉)激活抑制性中间神经元 → 关闸 → 抑痛
- 解释揉痛处减痛、TENS、针灸部分机制
- 后扩展为 **neuromatrix theory**(痛是脑生成的输出)

---

## 5. 下行调制

- **PAG → RVM → 脊髓**:内源镇痛(阿片、5-HT、NE)
- 安慰剂镇痛 = 内源阿片(纳洛酮可逆)
- 应激镇痛、注意调制
- 也可下行**易化**(慢性痛中失衡)

---

## 6. PyTorch — Gate Control

```python
import torch

def gate_control(c_fiber, a_beta, descending_inhib=0.0):
    """A-beta (touch) + descending close the gate on C-fiber pain."""
    inhibitory_interneuron = torch.sigmoid(a_beta + descending_inhib)
    pain_output = torch.relu(c_fiber - 1.5 * inhibitory_interneuron)
    return pain_output   # rubbing (↑a_beta) or descending → less pain
```

---

## 7. 中枢敏化 + 慢性痛

- **Wind-up**:C 纤维重复 → 背角 NMDA → 渐强(短期)
- **中枢敏化**:长期 → 痛阈↓、感受野扩、allodynia(轻触即痛)、hyperalgesia
- 慢性痛 = 神经系统**可塑性疾病**(常无持续组织损伤)
- 类 LTP 机制(见 [LTP_LTD](../02_Cellular_Molecular/LTP_LTD.md))

---

## 8. 慢性痛类型

| 类型 | 机制 |
|---|---|
| Nociceptive | 持续组织损伤(炎症) |
| Neuropathic | 神经损伤(灼、电击样) |
| Nociplastic | 中枢敏化(纤维肌痛;无明确损伤) |
| Mixed | 多机制 |

---

## 9. 治疗

- **WHO 阶梯**:NSAID → 弱阿片 → 强阿片(+ 辅助)
- **Neuropathic**:gabapentinoid、TCA/SNRI(非阿片优先)
- **阿片危机**:慢性非癌痛长期阿片弊大(耐受/痛敏/成瘾,见 [Addiction](../08_Neuro_Disorders/Addiction.md))
- **非药物**:CBT、运动、针灸、神经调控(SCS、DBS)
- **新靶**:Nav1.7/1.8、CGRP(偏头痛)、NGF 抗体

---

## 10. Common Pitfalls

### 10.1 痛 ∝ 组织损伤

弱相关;痛是脑输出(neuromatrix),慢性痛常无损伤。

### 10.2 单一"痛中枢"

是分布 pain matrix(感觉 + 情绪 + 认知)。

### 10.3 慢性痛 = 急性痛延长

是中枢敏化(可塑病),机制不同。

### 10.4 阿片是慢性痛首选

慢性非癌痛长期阿片弊大;非阿片 + 多模式优先。

### 10.5 安慰剂 = "假的"

真实内源阿片机制(纳洛酮可逆),神经可测。

---

## 11. Related Concepts

- **同节**:[Somatosensory](Somatosensory.md)、[Reward_System](Reward_System.md)
- **细胞**:[Neurotransmitters](../02_Cellular_Molecular/Neurotransmitters.md)、[Ion Channels](../02_Cellular_Molecular/Ion_Channels.md)(TRPV1/Nav)、[LTP_LTD](../02_Cellular_Molecular/LTP_LTD.md)
- **疾病**:[Addiction](../08_Neuro_Disorders/Addiction.md)(阿片)、[Migraine](../08_Neuro_Disorders/Migraine.md)

---

## References

1. **Melzack, R. & Wall, P. D.** "Pain mechanisms: a new theory." *Science*, 1965.
2. **Basbaum, A. I. et al.** "Cellular and molecular mechanisms of pain." *Cell*, 2009.
3. **Woolf, C. J.** "Central sensitization: implications for the diagnosis and treatment of pain." *Pain*, 2011.
4. **Tracey, I. & Mantyh, P. W.** "The cerebral signature for pain perception and its modulation." *Neuron*, 2007.
