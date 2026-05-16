# 执行功能 (Executive Function)

> *Executive function 是高级认知控制的总称:抑制(inhibition)、工作记忆更新(updating)、认知灵活(shifting)— Miyake 三因子。PFC(尤 dlPFC、ACC)主体。是目标导向行为、规划、自我调节的核心。ADHD、FTD、衰老损此功能。是 human vs LLM 的关键差异维度。*
>
> **难度**:Intermediate
> **前置知识**:[Working Memory](Working_Memory.md)、[Cortex](../01_Neuroanatomy/Cortex.md)

---

## 1. Miyake 三因子模型 (2000)

| 成分 | 功能 | 任务 |
|---|---|---|
| **Inhibition** | 抑制优势/无关反应 | Stroop、Go/NoGo、Flanker |
| **Updating** | 工作记忆刷新 | N-back、keep-track |
| **Shifting** | 任务/规则切换 | Task-switching、WCST |

→ "Unity and diversity":三者相关但可分离。

---

## 2. 神经基础

- **dlPFC**:工作记忆、规则维持、shifting
- **vlPFC / IFG**:抑制控制
- **ACC**:冲突监测、错误检测
- **Parietal**:与 PFC 构成 fronto-parietal control network
- **Basal ganglia**:门控(gating)更新

---

## 3. 经典任务

- **Stroop**:命名字色 vs 读字(抑制)
- **Wisconsin Card Sorting (WCST)**:规则推断 + 切换(PFC 损伤经典缺陷)
- **Tower of London/Hanoi**:规划
- **N-back**:更新
- **Trail Making B**:shifting
- **Iowa Gambling**:决策(vmPFC)

---

## 4. PyTorch — 冲突监测 + 抑制(Stroop 类)

```python
import torch

def stroop_conflict(word_signal, color_signal, control_gain=2.0):
    """ACC detects conflict; PFC control biases toward task-relevant."""
    conflict = (word_signal * color_signal).abs()        # co-activation
    # PFC top-down boosts color (task) and suppresses word (prepotent)
    response = control_gain * color_signal - 0.5 * word_signal
    rt = 1.0 + conflict                                  # RT ↑ with conflict
    return torch.sigmoid(response), rt
```

---

## 5. 发展与衰老

- **发展**:EF 随 PFC 成熟到 ~ 25 岁(见 [Neurodevelopment](../00_Foundations/Neurodevelopment.md))
- 早期 EF 预测后期学业 / 健康 / 收入(Moffitt 2011 纵向)
- **衰老**:EF 早衰(尤 shifting / updating);crystallized 保留
- 是认知衰老最敏感域之一

---

## 6. 与工作记忆关系

- WM 是 EF 的核心底物(updating ∝ WM)
- 但 EF ⊃ WM:含 inhibition、shifting、规划
- Central executive(Baddeley)≈ EF(见 [Working Memory](Working_Memory.md))

---

## 7. 病理

- **ADHD**:EF 缺陷核心(尤 inhibition,Barkley)— 见 [ADHD](../08_Neuro_Disorders/ADHD.md)
- **bvFTD**:去抑制 + 计划丧失(见 [Frontotemporal Dementia](../08_Neuro_Disorders/Frontotemporal_Dementia.md))
- **PFC lesion**:dysexecutive syndrome、Phineas Gage
- **Schizophrenia / depression**:EF 受损
- **TBI**:常累 PFC → EF

---

## 8. 测量 + 局限

- "Task impurity problem":每任务混杂非 EF 成分
- 生态效度:实验任务 ≠ 真实生活 EF
- 因子结构随年龄变(童年更"unitary")
- Latent variable 建模(SEM)缓解

---

## 9. AI 类比

| EF 成分 | AI |
|---|---|
| Inhibition | gating、attention 抑制无关 |
| Updating | RNN/记忆写入、Transformer KV |
| Shifting | task/context switching、in-context learning |
| 规划 | tree search、planning module |

LLM 在多步规划 / 抑制干扰仍弱于人 EF(差异维度)。

---

## 10. Common Pitfalls

### 10.1 EF = IQ

相关但分离;EF 预测结果独立于 IQ。

### 10.2 EF = 工作记忆

WM 是子成分;EF 更广(抑制 + 切换 + 规划)。

### 10.3 单一统一能力

Unity + diversity:可分离成分(Miyake)。

### 10.4 任务纯测 EF

Task impurity;需 latent variable 分离。

### 10.5 训练可大幅提升通用 EF

近迁移有,远迁移 / 通用提升证据弱(同 brain training 争议)。

---

## 11. Related Concepts

- **同节**:[Working Memory](Working_Memory.md)、[Decision_Making](Decision_Making.md)、[Metacognition](Metacognition.md)、[Attention](Attention.md)
- **解剖**:[Cortex](../01_Neuroanatomy/Cortex.md)(PFC)
- **疾病**:[ADHD](../08_Neuro_Disorders/ADHD.md)、[Frontotemporal Dementia](../08_Neuro_Disorders/Frontotemporal_Dementia.md)

---

## References

1. **Miyake, A. et al.** "The unity and diversity of executive functions." *Cogn Psychol*, 2000.
2. **Diamond, A.** "Executive functions." *Annu Rev Psychol*, 2013.
3. **Miller, E. K. & Cohen, J. D.** "An integrative theory of prefrontal cortex function." *Annu Rev Neurosci*, 2001.
4. **Moffitt, T. E. et al.** "A gradient of childhood self-control predicts health, wealth, and public safety." *PNAS*, 2011.
