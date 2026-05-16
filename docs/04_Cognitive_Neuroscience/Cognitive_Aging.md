# 认知老化 (Cognitive Aging)

> *正常老化 ≠ 痴呆。Fluid 能力(处理速度、WM、episodic、EF)随龄下降;crystallized(词汇、知识)保留甚至提升。机制:PFC/海马萎缩、白质退化、多巴胺↓、dedifferentiation。Cognitive reserve + 补偿(PASA、HAROLD)解释个体差异。区分正常老化 vs MCI/AD 是临床核心。*
>
> **难度**:Intermediate
> **前置知识**:[Executive Function](Executive_Function.md)、[Memory_Systems](Memory_Systems.md)

---

## 1. 双轨:Fluid vs Crystallized

| | Fluid | Crystallized |
|---|---|---|
| 内容 | 处理速度、WM、推理、episodic、EF | 词汇、常识、专业知识 |
| 轨迹 | 成年后渐降(~20s 起) | 稳定 / 上升至 60s+ |
| 例 | 数字符号、Raven | 词汇测验 |

→ "老化不是全面衰退"(领域分化)。

---

## 2. 最受累的域

- **处理速度**(Salthouse:速度↓ 是多数下降的中介)
- **工作记忆 / EF**(尤 updating、shifting,见 [Executive Function](Executive_Function.md))
- **Episodic memory**(尤 free recall、source memory;recognition 较保)
- 相对保留:semantic、procedural、情绪调节("积极效应")

---

## 3. 神经机制

- **结构**:PFC + 海马萎缩最明显;白质完整性↓(见 [DTI Tractography](../07_Neurotech_Frontiers/DTI_Tractography.md))
- **多巴胺**:D2 受体 ~ 每十年 -10% → 影响 WM/EF/速度
- **Dedifferentiation**:神经表征特异性↓(调谐变宽)
- 默认网络连接↓;突触/树突变化

---

## 4. 补偿模型

| 模型 | 描述 |
|---|---|
| **HAROLD** | 老人双侧 PFC 激活(去偏侧化补偿) |
| **PASA** | 后→前 转移(枕↓ 额↑) |
| **CRUNCH** | 低负荷过度激活,高负荷资源耗尽 |
| **Scaffolding (STAC)** | 补偿性脚手架应对神经退化 |

---

## 5. PyTorch — Dedifferentiation 模拟

```python
import torch

def neural_tuning(stimulus, age_factor=0.0):
    """Aging broadens tuning curves -> less distinct representations."""
    preferred = torch.linspace(0, 1, 10)
    sigma = 0.1 + 0.3 * age_factor          # older -> wider tuning
    resp = torch.exp(-((stimulus - preferred) ** 2) / (2 * sigma ** 2))
    return resp   # high age_factor -> overlapping (dedifferentiated) codes
```

---

## 6. Cognitive Reserve

- 高教育 / 认知活跃 / 双语 / 社交 → 同等病理下功能更好
- "Reserve" 缓冲(非阻止)退化
- 解释为何脑病理与症状解离(有 AD 病理却无症状)
- 强调可改变因素(运动、认知 + 社会参与)

---

## 7. 正常老化 vs 病理

| | 正常老化 | MCI | AD 痴呆 |
|---|---|---|---|
| Episodic | 轻降(recall) | 明显客观损 | 严重 + 进展 |
| 功能 | 保留 | 基本保留 | 受损 |
| 进展 | 缓慢稳定 | 可转化 AD | 进行性 |

鉴别是临床核心(见 [Alzheimer](../08_Neuro_Disorders/Alzheimer.md))。

---

## 8. 保护 / 风险因素

- **保护**:有氧运动(最强证据)、教育、社交、地中海饮食、睡眠、认知参与
- **风险**:高血压/糖尿病(血管)、吸烟、抑郁、孤独、听力丧失(可干预!)
- Lancet 委员会:~ 40% 痴呆风险归于可改变因素

---

## 9. 与 AI

- Dedifferentiation ↔ 表征崩溃 / 容量衰减
- Cognitive reserve ↔ 冗余 / 鲁棒性 / 模型容量
- 老化轨迹建模 ↔ 纵向 ML(疾病进展预测)
- 但 AI 无生物老化;类比限度大

---

## 10. Common Pitfalls

### 10.1 老化 = 全面认知衰退

Crystallized 保留/上升;领域分化。

### 10.2 老化 = 痴呆

正常老化轻、稳、功能保留;痴呆是病理。

### 10.3 脑萎缩 = 一定有症状

Cognitive reserve 解离病理与症状。

### 10.4 补偿激活 = 更好

补偿可有效或无效(CRUNCH:高负荷耗尽)。

### 10.5 老化不可干预

~40% 痴呆风险可改变(运动/听力/血管等)。

---

## 11. Related Concepts

- **同节**:[Executive Function](Executive_Function.md)、[Memory_Systems](Memory_Systems.md)、[Working Memory](Working_Memory.md)
- **疾病**:[Alzheimer](../08_Neuro_Disorders/Alzheimer.md)
- **前沿**:[DTI Tractography](../07_Neurotech_Frontiers/DTI_Tractography.md)
- **基础**:[Neuroplasticity](../00_Foundations/Neuroplasticity.md)

---

## References

1. **Salthouse, T. A.** "The processing-speed theory of adult age differences in cognition." *Psychol Rev*, 1996.
2. **Cabeza, R.** "Hemispheric asymmetry reduction in older adults: the HAROLD model." *Psychol Aging*, 2002.
3. **Park, D. C. & Reuter-Lorenz, P.** "The adaptive brain: aging and neurocognitive scaffolding." *Annu Rev Psychol*, 2009.
4. **Livingston, G. et al.** "Dementia prevention, intervention, and care (Lancet Commission)." *Lancet*, 2020.
