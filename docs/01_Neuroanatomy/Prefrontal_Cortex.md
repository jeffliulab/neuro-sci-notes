# 前额叶皮层 (Prefrontal Cortex)

> *PFC = 额叶运动/前运动区之前的皮层,人类占比最大。执行功能、工作记忆、决策、社会行为、抑制控制的核心。三大亚区:dlPFC(认知)、vmPFC/OFC(价值情绪)、ACC(冲突监测)。最晚成熟(~25 岁髓鞘化)。Phineas Gage 是其功能的历史案例。*
>
> **难度**:Intermediate
> **前置知识**:[Cortex](Cortex.md)、[Executive Function](../04_Cognitive_Neuroscience/Executive_Function.md)

---

## 1. 定义 + 亚区

| 亚区 | 功能 |
|---|---|
| **dlPFC**(背外侧) | 工作记忆、规则、计划、shifting |
| **vmPFC / OFC**(腹内/眶额) | 价值、情绪调节、社会决策 |
| **ACC**(前扣带) | 冲突监测、错误、动机 |
| **lPFC/vlPFC** | 抑制控制、语言(左 Broca 附近) |
| **frontopolar(BA10)** | 元认知、多任务、关系推理 |

---

## 2. 连接

- 与几乎所有皮层 + 边缘 + BG + 丘脑 MD + 脑干调质广泛互连
- "Hub":整合 + top-down 控制
- 接收所有调质(DA/NE/5-HT/ACh)→ 状态敏感(倒 U,见 [ADHD](../08_Neuro_Disorders/ADHD.md))

---

## 3. 核心功能

- **执行功能**(见 [Executive Function](../04_Cognitive_Neuroscience/Executive_Function.md))
- **工作记忆**:dlPFC persistent activity(见 [Working Memory](../04_Cognitive_Neuroscience/Working_Memory.md))
- **决策/价值**:vmPFC/OFC(见 [Decision_Making](../04_Cognitive_Neuroscience/Decision_Making.md))
- **社会/道德**:vmPFC(Damasio 躯体标记)
- **抑制控制**:vlPFC → 抑优势反应

---

## 4. PyTorch — PFC 持续活动(WM)

```python
import torch

def pfc_persistent_activity(cue, T=100, recurrent_w=1.05, decay=0.05):
    """Recurrent self-excitation maintains WM trace after cue ends."""
    h = 0.0
    trace = []
    for t in range(T):
        inp = cue if t < 10 else 0.0           # cue only early
        h = torch.tanh(torch.tensor(recurrent_w * h + inp - decay))
        trace.append(float(h))
    return trace   # activity persists in delay (attractor, see Working_Memory)
```

---

## 5. Phineas Gage(历史案例)

- 1848 铁棍穿左额(尤 vmPFC)→ 智力/记忆保留,**人格/社会决策剧变**
- 首个 PFC ↔ 人格/社会行为证据
- 现代:vmPFC 损 → Iowa gambling 缺陷(Damasio)

---

## 6. 发育(最晚成熟)

- 突触修剪 + 髓鞘化持续至 ~ 25 岁(后→前最末,见 [Neurodevelopment](../00_Foundations/Neurodevelopment.md))
- 解释青少年风险决策 + 冲动(PFC-边缘失衡)
- 法律/教育含义

---

## 7. 病理

- **bvFTD**:vmPFC/OFC 退行 → 去抑制、人格(见 [Frontotemporal_Dementia](../08_Neuro_Disorders/Frontotemporal_Dementia.md))
- **ADHD**:PFC-纹状体 DA/NE 失调(见 [ADHD](../08_Neuro_Disorders/ADHD.md))
- **Schizophrenia**:dlPFC 低激活(hypofrontality)+ WM 损
- **抑郁**:dlPFC↓ / sgACC↑(DBS 靶,见 [Depression](../08_Neuro_Disorders/Depression.md))
- **TBI**:额叶常累 → 执行障碍

---

## 8. 进化

- 人 PFC 绝对 + 相对扩大(尤 frontopolar)
- 连接复杂度 + 调质支配↑
- 与抽象、规划、社会复杂性关联
- 但"人独有"程度有争议(灵长连续谱)

---

## 9. 与 AI

- PFC = "执行控制器 / 中央调度" ↔ controller、gating、meta-controller 类比
- 持续活动 WM ↔ RNN/working memory(见 [Working Memory](../04_Cognitive_Neuroscience/Working_Memory.md))
- Mixed selectivity(见 [Neural Population Dynamics](../05_Computational_Neuroscience/Neural_Population_Dynamics.md))
- LLM 缺稳健多步规划/抑制 → 类 PFC 功能差距

---

## 10. Common Pitfalls

### 10.1 PFC = "理性中枢"

也主情绪调节/社会/价值(vmPFC);非纯"冷认知"。

### 10.2 一区一功能

亚区分工但高度交互;损伤效应分布。

### 10.3 PFC 童年成熟

最晚(~25 岁);青少年 PFC 未熟。

### 10.4 越大越聪明

连接/调质/微结构 > 体积;EQ 复杂。

### 10.5 Gage 完全"失去人性"

人格改变但保留多数功能;细节被夸大(史料考证)。

---

## 11. Related Concepts

- **同节**:[Cortex](Cortex.md)、[Cingulate_Cortex](Cingulate_Cortex.md)、[Basal_Ganglia](Basal_Ganglia.md)、[Thalamus](Thalamus.md)
- **认知**:[Executive Function](../04_Cognitive_Neuroscience/Executive_Function.md)、[Working Memory](../04_Cognitive_Neuroscience/Working_Memory.md)、[Decision_Making](../04_Cognitive_Neuroscience/Decision_Making.md)
- **疾病**:[Frontotemporal_Dementia](../08_Neuro_Disorders/Frontotemporal_Dementia.md)、[ADHD](../08_Neuro_Disorders/ADHD.md)

---

## References

1. **Miller, E. K. & Cohen, J. D.** "An integrative theory of prefrontal cortex function." *Annu Rev Neurosci*, 2001.
2. **Fuster, J. M.** *The Prefrontal Cortex*. 5th ed., 2015.
3. **Damasio, A. R.** *Descartes' Error*. 1994.
4. **Arnsten, A. F. T.** "Stress signalling pathways that impair prefrontal cortex structure and function." *Nat Rev Neurosci*, 2009.
