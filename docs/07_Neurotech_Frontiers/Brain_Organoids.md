# 脑类器官 (Brain Organoids)

> *Brain organoid 是 iPSC/ESC 自组织形成的 3D 微型脑组织(mm 级,含分层皮层结构、多种 neuron + glia)。Lancaster 2013 首个 cerebral organoid。用于发育、疾病建模(小头症、ASD、ZIKV)、药筛、organoid intelligence。引发 sentience 伦理争论(见 [Neuroscience Ethics](../00_Foundations/Neuroscience_Ethics.md))。*
>
> **难度**:Advanced
> **前置知识**:[Neurodevelopment](../00_Foundations/Neurodevelopment.md)、干细胞基础

---

## 1. 是什么

- iPSC / ESC → 神经诱导 → 3D 自组织培养
- 形成:神经上皮 → 分层皮层样结构 + 多 neuron 类型 + glia
- 大小:~ 1-4 mm(扩散限制 → 核心坏死)
- 不是"迷你完整脑":无血管、无感觉输入、组织无序

---

## 2. 类型

| 类型 | 模拟 |
|---|---|
| Cerebral organoid | 皮层(未定向) |
| Region-specific | 前脑 / 中脑(DA)/ 海马 / 视杯 |
| **Assembloid** | 多脑区融合(如皮层+丘脑,研究环路) |
| Vascularized | + 内皮(改善核心存活) |
| Sliced (ALI-CO) | 切片 → 改善氧供 + 轴突外伸 |

---

## 3. 关键应用

- **发育**:人特异皮层扩张机制(vs 小鼠)
- **疾病建模**:
  - 小头症(CDK5RAP2)— Lancaster 原始发现
  - ZIKV → 小头症机制(2016 快速应用)
  - ASD、SCZ、Rett(MECP2)、Timothy syndrome
- **药筛 / 毒理**:神经发育毒性
- **移植**:类器官植入动物 → 整合(Pasca 2022 大鼠皮层整合 + 影响行为)

---

## 4. PyTorch — 自组织极化(玩具)

```python
import torch

def organoid_self_org(N=50, T=100, diffusion=0.1):
    """Toy reaction-diffusion: morphogen gradient -> regional identity."""
    field = torch.zeros(N)
    field[0] = 1.0                       # morphogen source (e.g., SHH)
    for _ in range(T):
        lap = torch.zeros(N)
        lap[1:-1] = field[2:] - 2*field[1:-1] + field[:-2]
        field = field + diffusion * lap
        field = field.clamp(0, 1)
    # Threshold -> regional fate (gradient -> patterning)
    fate = (field > 0.5).long()
    return field, fate
```

---

## 5. Organoid Intelligence (OI)

- 类器官 + MEA(微电极阵列)→ 读写活动
- "DishBrain"(Kagan 2022):类器官学玩 Pong(争议性宣称)
- "Wetware computing"愿景
- 极早期 + 大量过度解读风险

---

## 6. 与动物模型对比

| | Organoid | 动物模型 |
|---|---|---|
| 人特异 | ✓ | ✗(种属差) |
| 完整环路 | ✗(无输入/输出) | ✓ |
| 行为读出 | ✗ | ✓ |
| 伦理 | 较低(但 sentience 争议) | 较高 |
| 成熟度 | 胎儿早期水平 | 完整 |

---

## 7. 局限

- **无血管** → 核心缺氧坏死(限大小 + 成熟)
- 成熟度仅~胎儿早期(无完整髓鞘 / 成人网络)
- 批次变异大(可重复性)
- 无感觉输入 / 运动输出(无"经验")
- 组织结构不完全有序

---

## 8. 伦理 — Sentience?

- 类器官有协调电活动(振荡 — Trujillo 2019)→ 引发是否"有意识"问
- 共识:当前**极不可能有 sentience**(无感觉/经验/完整结构/规模)
- 但需前瞻治理(嵌合、移植、OI)
- 见 [Neuroscience Ethics](../00_Foundations/Neuroscience_Ethics.md)

---

## 9. 前沿方向

- Vascularized / 灌注 → 更大更成熟
- Assembloid → 环路 + 疾病(神经-肌、皮层-纹状体)
- 移植整合(人神经元在动物内成熟 — Pasca)
- 多组学 + 长培养(年级别)
- 个性化(患者 iPSC → 精准医学)

---

## 10. Common Pitfalls

### 10.1 = 迷你大脑

无血管 / 输入 / 完整结构;是发育片段非"小脑"。

### 10.2 有意识

当前极不可能(规模/结构/经验缺失);勿过度解读。

### 10.3 可重复稳定

批次变异显著;需标准化 + 质控。

### 10.4 成熟如成人脑

仅胎儿早期水平;长培养仍不达成人网络。

### 10.5 DishBrain = 类器官会思考

宣称争议大;早期 + 媒体放大。

---

## 11. Related Concepts

- **同节**:[Calcium Imaging](Calcium_Imaging.md)、[Neuropixels](Neuropixels.md)
- **基础**:[Neurodevelopment](../00_Foundations/Neurodevelopment.md)、[Neuroscience Ethics](../00_Foundations/Neuroscience_Ethics.md)
- **疾病**:[Autism](../08_Neuro_Disorders/Autism.md)、[Schizophrenia](../08_Neuro_Disorders/Schizophrenia.md)

---

## References

1. **Lancaster, M. A. et al.** "Cerebral organoids model human brain development and microcephaly." *Nature*, 2013.
2. **Qian, X. et al.** "Brain-region-specific organoids using mini-bioreactors for modeling ZIKV exposure." *Cell*, 2016.
3. **Pasca, S. P. et al.** "Maturation and circuit integration of transplanted human cortical organoids." *Nature*, 2022.
4. **Trujillo, C. A. et al.** "Complex oscillatory waves emerging from cortical organoids." *Cell Stem Cell*, 2019.
