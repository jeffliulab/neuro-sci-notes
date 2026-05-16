# 自我与能动感 (Self & Sense of Agency)

> *"自我"非单一:含 minimal self(身体所有感 + 能动感)与 narrative self(自传叙事)。Sense of agency = "是我做的"。Comparator model(预测 vs 反馈)解释 agency + 为何不能自己挠痒。DMN + insula + TPJ 主体。Schizophrenia(被动体验)、橡胶手错觉是关键证据。*
>
> **难度**:Intermediate
> **前置知识**:[Social_Cognition](Social_Cognition.md)、[Consciousness](Consciousness.md)

---

## 1. 自我的层级

| 层 | 内容 | 区域 |
|---|---|---|
| **Minimal self** | 身体所有感 + 能动感(此刻) | insula、TPJ、SMA |
| **Narrative self** | 自传、身份、时间延续 | mPFC、PCC/precuneus(DMN) |

---

## 2. 两成分(minimal self)

- **Ownership(所有感)**:"这是我的身体"
- **Agency(能动感)**:"这个动作是我发起的"
- 可分离:橡胶手错觉(ownership 错置)vs 被动运动(无 agency 有 ownership)

---

## 3. Comparator Model (Agency)

```
意图 → 运动指令
   ├→ Efference copy → Forward model → 预测感觉结果
   │                                      ↓ 比较
   └→ 实际动作 → 真实感觉反馈 ──────────→ 匹配?
匹配 → 强 agency + 感觉衰减(sensory attenuation)
失配 → 弱 agency / 归因外部
```

→ 解释为何**不能自己挠痒**(预测 → 衰减自生刺激)。

---

## 4. PyTorch — Comparator + 感觉衰减

```python
import torch

def sense_of_agency(motor_cmd, actual_feedback, forward_model_gain=0.9):
    """Predicted == actual -> high agency + attenuated sensation."""
    predicted = forward_model_gain * motor_cmd          # efference copy
    mismatch = (actual_feedback - predicted).abs()
    agency = torch.exp(-3.0 * mismatch)                 # high if match
    perceived = actual_feedback - 0.7 * predicted        # self-gen attenuated
    return agency, perceived
```

---

## 5. 橡胶手错觉 (RHI)

- 同步刷真手(藏)+ 假手(可见)→ 假手"属于我"
- 多感觉整合(视-触)覆盖本体感
- 证明 body ownership 是**推断**(可被操纵)
- 全身错觉(out-of-body)类似(TPJ)

---

## 6. 神经基础

- **Insula(前)**:interoception → 身体自我 + agency
- **TPJ**:自我-他人区分、视角、agency 失配检测
- **SMA / pre-SMA**:意图 + efference
- **mPFC / PCC(DMN)**:narrative self、self-referential 加工
- **Angular gyrus**:agency 失配

---

## 7. 病理

- **Schizophrenia**:被动体验、思维插入 = agency 缺陷(comparator 失调,弱 attenuation → 自生归因外部)
- **Anarchic/alien hand**:动作无 agency 感(SMA/胼胝体)
- **Depersonalization**:自我疏离感
- **Anosognosia**:否认缺陷(右顶叶)
- **Somatoparaphrenia**:否认肢体所有

---

## 8. Default Mode Network

- DMN(mPFC、PCC、angular)= self-referential + 自传记忆 + 心智漫游
- "静息态"激活;任务时抑制
- narrative self 的核心(见 [Consciousness](Consciousness.md))
- 与 [Memory_Systems](Memory_Systems.md) episodic 重叠

---

## 9. 与 AI

- Forward model / efference copy ↔ 机器人 self-model、predictive control
- "Sense of agency" ↔ agent 的 credit assignment / 意图建模
- LLM 无统一自我 / 身体(persona 是 pattern,非 minimal self)
- 自我建模 = 具身智能开放问题(见 [Embodied_Cognition](Embodied_Cognition.md))

---

## 10. Common Pitfalls

### 10.1 自我是单一实体

至少 minimal vs narrative 多成分(可分离损伤)。

### 10.2 身体所有感是直接给定

是多感觉**推断**(橡胶手可操纵)。

### 10.3 Agency = 意图

需 comparator 匹配;意图存在仍可失 agency(alien hand)。

### 10.4 感觉衰减无关紧要

是 agency 标志;失调 → schizophrenia 被动体验。

### 10.5 DMN = "无所事事"

是 self/记忆/模拟的活跃网络,非空闲。

---

## 11. Related Concepts

- **同节**:[Consciousness](Consciousness.md)、[Social_Cognition](Social_Cognition.md)、[Embodied_Cognition](Embodied_Cognition.md)、[Memory_Systems](Memory_Systems.md)
- **系统**:[Motor System](../03_Systems_Neuroscience/Motor_System.md)
- **疾病**:[Schizophrenia](../08_Neuro_Disorders/Schizophrenia.md)
- **AI**: self-model、predictive control

---

## References

1. **Gallagher, S.** "Philosophical conceptions of the self: implications for cognitive science." *Trends Cogn Sci*, 2000.
2. **Blakemore, S.-J. et al.** "Why can't you tickle yourself?" *NeuroReport*, 2000.
3. **Botvinick, M. & Cohen, J.** "Rubber hands 'feel' touch that eyes see." *Nature*, 1998.
4. **Frith, C. D. et al.** "Abnormalities in the awareness and control of action." *Phil Trans R Soc B*, 2000.
