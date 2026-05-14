# 基底神经节 (Basal Ganglia) — 动作选择与强化学习

> *基底神经节是 cortex 下方的一群核团:caudate, putamen, globus pallidus, substantia nigra, subthalamic nucleus。负责**动作选择**:cortex 提议动作 → BG 决定执行哪个。Dopamine 是 BG 的核心调制 — Parkinson DA 退化导致运动困难。*
>
> **难度**:Intermediate
> **前置知识**:[Cortex](Cortex.md)、[神经递质](../02_Cellular_Molecular/Neurotransmitters.md)

---

## 1. 主要核团

- **Striatum (纹状体)**: caudate + putamen — BG 主输入
- **Globus Pallidus** (内/外, GPi/GPe): 主输出
- **Substantia Nigra** (黑质):
  - SNc: DA 合成 — 投射到 striatum
  - SNr: 类 GPi 的输出
- **Subthalamic Nucleus (STN)**: 兴奋性,影响 GPi

---

## 2. Direct vs Indirect Pathway

经典模型:

```
Cortex → Striatum
            ↓
         (D1)            (D2)
            ↓               ↓
        Direct          Indirect
            ↓               ↓
          GPi/SNr     GPe → STN → GPi
            ↓                  ↓
        Thalamus (less inhibition → more cortex activity)
```

- **Direct (Go)**: 通过 D1 → 抑制 GPi → thalamus 失抑制 → 运动 promote
- **Indirect (No-Go)**: 通过 D2 → 长链 → GPi 更激活 → thalamus 抑制 → 运动抑制

DA 通过 D1 (兴奋) 和 D2 (抑制) 平衡 Go vs No-Go。

---

## 3. 与强化学习

### 3.1 RPE (Reward Prediction Error)

Wolfram Schultz 1997: DA neurons 编码 reward prediction error。
- Expected reward → no DA change
- Unexpected reward → DA burst
- Expected reward missing → DA dip

→ DA = "temporal difference error" signal,匹配 RL theory。

### 3.2 与 Actor-Critic

- **Striatum** = critic (评估 value)
- **DA** = TD error (训练信号)
- **Cortex → BG → thalamus → cortex** = actor (选择动作)

直接对应 actor-critic RL 框架。

---

## 4. 病理

### 4.1 Parkinson's Disease

- SNc DA 神经元退化 (80%+ 死亡才出症状)
- 运动启动困难、tremor、僵直
- 治疗:
  - L-DOPA (DA precursor)
  - DBS (Deep Brain Stimulation) on STN

### 4.2 Huntington's Disease

- Striatum medium spiny neuron 退化
- 不自主舞蹈动作 (chorea)
- Genetic (CAG repeat in HTT gene)

### 4.3 Tourette Syndrome

- BG 异常 → tics
- DA 调节失调

### 4.4 OCD

- Cortico-striato-thalamic loop 过度激活

---

## 5. 与认知

近年发现 BG 不只是 motor:
- 习惯形成 (habit) — putamen
- 程序性记忆 (procedural memory)
- 决策、社交行为
- 与 cortex multiple parallel loops

---

## 6. PyTorch — Actor-Critic 类比

```python
import torch
import torch.nn as nn

class BasalGangliaActorCritic(nn.Module):
    """BG-inspired actor-critic."""
    def __init__(self, state_dim, action_dim):
        super().__init__()
        # Striatum = critic + actor
        self.critic = nn.Linear(state_dim, 1)  # value
        self.actor = nn.Linear(state_dim, action_dim)
        self.action_dim = action_dim
    
    def forward(self, state):
        value = self.critic(state)
        logits = self.actor(state)  # action preferences
        return torch.softmax(logits, -1), value
    
    def td_error(self, r, v_now, v_next, gamma=0.99):
        """DA = TD error."""
        return r + gamma * v_next - v_now
```

---

## 7. 历史

- **1817** — Parkinson 描述疾病
- **1957** — Carlsson 发现 dopamine
- **1980s** — Direct / Indirect pathway model (Albin, DeLong)
- **1997** — Schultz DA = RPE
- **2000s** — DBS for Parkinson 临床
- **2010s** — Cortico-BG closed loops 多回路 model
- **2020s** — Optogenetics 验证 direct/indirect 功能

---

## 8. Common Pitfalls

### 8.1 Direct/Indirect 过度简化

二分法 oversimplified;现代发现 hybrid SPN + 复杂调节。

### 8.2 DA 不只 RPE

DA 也参与 motivation、novelty、salience。

### 8.3 BG ≠ 单一功能

Multiple parallel loops — motor / cognitive / limbic。

### 8.4 Parkinson 不止 SNc

也涉 cortex、autonomic system、其他 nuclei。

### 8.5 Striatum 主细胞

Medium spiny neurons (MSN) 95%,其余 interneurons。

---

## 9. Related Concepts

- **同节**:[Cortex](Cortex.md)、[Cerebellum](Cerebellum.md)、[Hippocampus](Hippocampus_Anatomy.md)
- **神经递质**:[Dopamine](../02_Cellular_Molecular/Neurotransmitters.md)
- **AI 对比**:Actor-Critic RL — https://jeffliulab.github.io/ai-notes/04_Reinforcement_Learning/

---

## References

1. **Schultz, W.** "A neural substrate of prediction and reward." *Science*, 1997.
2. **Albin, R. L. et al.** "The functional anatomy of basal ganglia disorders." *Trends Neurosci*, 1989.
3. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
4. **Bromberg-Martin, E. S. et al.** "Dopamine in motivational control." *Neuron*, 2010.
