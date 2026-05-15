# 额颞叶痴呆 (Frontotemporal Dementia)

> *FTD 是 < 65 岁痴呆第二常见(仅次早发 AD)。额叶/颞叶选择性退行 → 行为变异型(bvFTD:人格/社交剧变)或语言变异型(PPA)。病理:tau / TDP-43 / FUS 包涵体。与 ALS 谱系重叠(C9orf72)。易误诊为精神病。无疾病修饰治疗。*
>
> **难度**:Intermediate
> **前置知识**:[Alzheimer](Alzheimer.md)、[Social_Cognition](../04_Cognitive_Neuroscience/Social_Cognition.md)

---

## 1. 临床亚型

| 亚型 | 核心 |
|---|---|
| **bvFTD**(行为变异) | 人格改变、去抑制、冷漠、缺共情、强迫、饮食改变 |
| **svPPA**(语义性) | 词义/物体知识丧失(颞极) |
| **nfvPPA**(非流利) | 语法/言语产生障碍(左额岛) |
| **FTD-ALS** | + 运动神经元病 |

---

## 2. 与 AD 区别

| | FTD | Alzheimer |
|---|---|---|
| 起病 | 多 < 65 岁 | 多 > 65 岁 |
| 首发 | 行为/语言/人格 | 情景记忆 |
| 早期记忆 | 相对保留 | 早损 |
| 病理 | tau/TDP-43/FUS | amyloid + tau |
| 萎缩 | 额/颞 | 海马/颞顶 |

---

## 3. 病理分子

- **FTLD-tau**(~ 45%):Pick body、PSP、CBD 谱系
- **FTLD-TDP-43**(~ 50%):与 ALS 共享(C9orf72、GRN)
- **FTLD-FUS**(~ 5%)
- 蛋白错折叠 + 聚集 + prion 样扩散(见 [Prion Diseases](Prion_Diseases.md))

---

## 4. 遗传

- ~ 30-40% 家族性(痴呆中遗传比例高)
- **C9orf72** 六核苷酸重复扩增:FTD + ALS 共同最常见基因
- **GRN**(progranulin)、**MAPT**(tau)
- C9orf72 连接 FTD-ALS 谱系

---

## 5. 受累环路

- **bvFTD**:前额叶(orbitofrontal、ACC、vmPFC)+ 前岛 + 前颞
- 影响 social cognition、共情、决策、抑制(见 [Social_Cognition](../04_Cognitive_Neuroscience/Social_Cognition.md))
- Von Economo neurons(前岛/ACC)选择性易损
- "Social brain" 退行 → 行为剧变

---

## 6. PyTorch — 抑制控制丧失模拟

```python
import torch

def bvftd_disinhibition(impulse, pfc_inhibition, T=50):
    """vmPFC/OFC degeneration -> loss of behavioral inhibition."""
    behavior = []
    for t in range(T):
        # Healthy: PFC suppresses inappropriate impulses
        action = torch.relu(torch.tensor(impulse - pfc_inhibition))
        behavior.append(action.item())
    return behavior

# bvFTD: pfc_inhibition -> 0 -> impulses become unfiltered actions
```

---

## 7. 诊断难点

- 早期常误诊为**精神病 / 抑郁 / 双相 / 中年危机**
- 行为剧变但记忆/认知测试早期可正常
- 需结合影像(额颞萎缩)、行为史、有时基因
- 平均确诊延迟长

---

## 8. 治疗

- **无疾病修饰治疗**
- SSRI:减去抑制 / 强迫 / 饮食行为(对症)
- Antipsychotics:谨慎(易锥体外系副作用)
- **胆碱酯酶抑制剂无效甚至加重**(与 AD 不同 — 关键区别!)
- 照护者支持、行为管理
- GRN / C9orf72 基因疗法临床试验中

---

## 9. 社会 / 伦理

- 起病早 → 工作家庭冲击大
- 早期"道德/行为"改变易被误解为品德问题
- 司法:去抑制行为的责任能力争议
- 照护负担极重

---

## 10. Common Pitfalls

### 10.1 痴呆 = 记忆差

FTD 早期记忆常保留;首发是行为/语言。

### 10.2 = Alzheimer

病理、年龄、首发、治疗均不同;胆碱酯酶抑制剂 FTD 无效/有害。

### 10.3 行为变化 = 精神病

bvFTD 常被误诊;需神经退行鉴别。

### 10.4 老年病

FTD 多 45-65 岁起病(早发痴呆主因)。

### 10.5 与 ALS 无关

C9orf72 连接 FTD-ALS 同一谱系。

---

## 11. Related Concepts

- **同节**:[Alzheimer](Alzheimer.md)、[ALS](ALS.md)、[Parkinson](Parkinson.md)
- **认知**:[Social_Cognition](../04_Cognitive_Neuroscience/Social_Cognition.md)、[Decision_Making](../04_Cognitive_Neuroscience/Decision_Making.md)
- **解剖**:[Cortex](../01_Neuroanatomy/Cortex.md)(前额叶)

---

## References

1. **Bang, J., Spina, S., Miller, B. L.** "Frontotemporal dementia." *Lancet*, 2015.
2. **DeJesus-Hernandez, M. et al.** "Expanded GGGGCC hexanucleotide repeat in C9ORF72 causes FTD and ALS." *Neuron*, 2011.
3. **Seeley, W. W. et al.** "Early frontotemporal dementia targets neurons unique to apes and humans (von Economo)." *Ann Neurol*, 2006.
4. **Rascovsky, K. et al.** "Sensitivity of revised diagnostic criteria for bvFTD." *Brain*, 2011.
