# 内感受 (Interoception)

> *Interoception = 对身体内部状态(心跳、呼吸、饥饿、内脏)的感知。岛叶(尤前岛)是核心。是情绪(James-Lange / Damasio 躯体标记)、自我、稳态调节、决策的底层。心跳感知任务是经典测量。与焦虑、抑郁、进食障碍紧密。Friston 主动推断框架的核心。*
>
> **难度**:Intermediate
> **前置知识**:[Autonomic_Nervous_System](Autonomic_Nervous_System.md)、[Emotion](../04_Cognitive_Neuroscience/Emotion.md)

---

## 1. 定义 + 维度

- **Interoception**:感知内部生理状态(vs exteroception 外部)
- 维度(Garfinkel):
  - **Accuracy**:客观准确度(心跳计数)
  - **Sensibility**:自我报告倾向
  - **Awareness**:元认知(信心-准确对应)
- 三者可分离

---

## 2. 通路

```
内脏感受器(机械/化学/伤害)
   ↓ 迷走 / 脊髓 lamina I
NTS(孤束核)/ 臂旁核
   ↓
丘脑 VMpo
   ↓
后岛 → 中岛 → 前岛(AIC)
   ↓
ACC / OFC / 杏仁核(整合 → 情绪/价值)
```

前岛(AIC)= 内感受 + 主观感受整合中枢(Craig)。

---

## 3. 与情绪理论

- **James-Lange**:情绪 = 对身体变化的感知("见熊→逃→怕")
- **Damasio 躯体标记假说**:身体状态信号引导决策(Iowa gambling,vmPFC)
- **Schachter-Singer 二因素**:生理唤醒 + 认知标签
- 前岛 = "情感觉知"枢纽

---

## 4. PyTorch — 躯体标记引导决策

```python
import torch

def somatic_marker_decision(options, somatic_signal):
    """Bodily (interoceptive) signal biases choice before explicit reasoning."""
    # somatic_signal: learned visceral 'gut feeling' per option (vmPFC/insula)
    explicit_value = options.mean(dim=1)
    gut_bias = torch.tanh(somatic_signal)          # anticipatory bodily state
    decision_value = explicit_value + 0.6 * gut_bias
    return decision_value.argmax()
```

---

## 5. 心跳感知任务

- **Heartbeat counting / discrimination**:经典 accuracy 测量
- 高内感受准确 → 更强情绪体验、更佳直觉决策(部分研究)
- 但任务效度争议(可能靠心率知识非真感知)
- 趋势:呼吸 / 胃 / 多通道 + 神经测量

---

## 6. 主动推断视角

- Friston:脑预测内部状态 → 误差最小化经**自主行动**(allostasis)
- 内感受 = 对身体的 generative model(见 [Free Energy Principle](../05_Computational_Neuroscience/Free_Energy_Principle.md))
- 情绪 = 内感受推断结果
- "Allostasis":预测性调节(非被动稳态)

---

## 7. 临床

- **焦虑 / 惊恐**:内感受过敏 + 误读(心跳→"心脏病")
- **抑郁**:内感受钝化
- **进食障碍**:饥饱信号失调(岛叶异常)
- **述情障碍(alexithymia)**:难识别情绪 ~ 内感受缺陷
- **躯体症状障碍**、PTSD
- 内感受训练(干预方向)

---

## 8. 与自我 / 意识

- Minimal self 的身体基础(见 [Self_and_Agency](../04_Cognitive_Neuroscience/Self_and_Agency.md))
- "Embodied"情绪 + 自我(见 [Embodied_Cognition](../04_Cognitive_Neuroscience/Embodied_Cognition.md))
- 意识的躯体根源理论(Damasio core consciousness)

---

## 9. 与 AI

- LLM 无身体 → 无内感受 → "无真情绪"论据之一
- Affective computing:外部生理(HR/GSR)≈ 内感受代理
- Homeostatic RL:内部状态作 reward(Keramati & Gutkin)
- 具身 agent 需 internal-state model(见 [Embodied_Cognition](../04_Cognitive_Neuroscience/Embodied_Cognition.md))

---

## 10. Common Pitfalls

### 10.1 内感受 = 心跳感知

是多通道(呼吸/饥/痛/内脏);心跳只是常用测量。

### 10.2 Accuracy = Awareness

三维度可分离(Garfinkel);高报告 ≠ 高准确。

### 10.3 内感受被动

主动推断 / allostasis:预测性调节。

### 10.4 与情绪无关

James-Lange/Damasio:内感受是情绪核心基础。

### 10.5 心跳计数任务无瑕

可能靠心率知识;效度争议未决。

---

## 11. Related Concepts

- **同节**:[Autonomic_Nervous_System](Autonomic_Nervous_System.md)、[Hypothalamus_Homeostasis](Hypothalamus_Homeostasis.md)、[Pain_System](Pain_System.md)
- **认知**:[Emotion](../04_Cognitive_Neuroscience/Emotion.md)、[Self_and_Agency](../04_Cognitive_Neuroscience/Self_and_Agency.md)、[Decision_Making](../04_Cognitive_Neuroscience/Decision_Making.md)
- **计算**:[Free Energy Principle](../05_Computational_Neuroscience/Free_Energy_Principle.md)

---

## References

1. **Craig, A. D.** "How do you feel? Interoception: the sense of the physiological condition of the body." *Nat Rev Neurosci*, 2002.
2. **Damasio, A. R.** *Descartes' Error*. 1994.
3. **Garfinkel, S. N. et al.** "Knowing your own heart: distinguishing interoceptive accuracy from awareness." *Biol Psychol*, 2015.
4. **Seth, A. K.** "Interoceptive inference, emotion, and the embodied self." *Trends Cogn Sci*, 2013.
