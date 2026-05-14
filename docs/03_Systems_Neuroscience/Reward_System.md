# 奖赏系统 — VTA / NAcc / DA / RL 神经基础

> *大脑奖赏系统 (reward circuit) 由 VTA → NAcc 投射 + 多个 cortical 调制构成。中心 NT 是 dopamine。Schultz 1997 RPE 发现把 DA 与 reinforcement learning 直接对应。是 motivation、addiction、decision 的核心。*
>
> **难度**:Intermediate
> **前置知识**:[Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.md)、[神经递质](../02_Cellular_Molecular/Neurotransmitters.md)

---

## 1. 核心 circuit

```
VTA (Ventral Tegmental Area) ─DA→ NAcc (Nucleus Accumbens) → 行为
  │                            │
  └─DA→ PFC (规划) ───────────────┐
                                  ↓
                              Amygdala (情绪)
                              Hippocampus (记忆)
                                  ↓
                       OFC (Orbitofrontal) 整合 value
```

---

## 2. DA neurons in VTA

- ~ 25k DA neurons in 人 VTA
- 投射广,影响 NAcc / PFC / amygdala / 海马
- 自发 firing ~ 5 Hz (tonic)
- Reward 触发 phasic burst

---

## 3. RPE (Reward Prediction Error)

Schultz 1997:VTA DA neurons 编码:

$$\delta = r + \gamma V(s') - V(s)$$

- 预期 reward → no DA change (already learned)
- Unexpected reward → DA burst (positive RPE)
- Expected reward 没来 → DA dip (negative RPE)

→ 与 TD-learning 数学一致。

---

## 4. 与 Actor-Critic RL

| 大脑 | RL |
|---|---|
| Striatum (medium spiny neuron) | Critic + Actor |
| VTA DA | TD error δ |
| PFC | Policy / value 高层 |
| Hippocampus | State representation |
| Amygdala | Emotional value |

---

## 5. NAcc (Nucleus Accumbens)

- Ventral striatum 一部分
- "Pleasure center"
- DA → motivation / wanting
- Opioids → liking
- Hub of addiction

---

## 6. 不同维度

DA 不只 reward:
- **Salience**: 重要事件 (好 / 坏)
- **Novelty**: 新刺激
- **Effort**: 估计成本
- **Risk**: 不确定性
- **Motor**: SNc → BG → 运动

---

## 7. Addiction 机制

```
Drug → 直接刺 DA 释放 (cocaine 阻 reuptake; opioid → VTA)
     → NAcc 极高 DA
     → 学到 strong association: drug = max reward
     → Compulsive seeking
     → 长期: DA receptor down-regulation → tolerance
```

→ Reward hijacked。

---

## 8. Affect 与 Choice

- **OFC (Orbitofrontal cortex)**: 计算 value
- **vmPFC**: 选择间比较
- **Anterior insula**: risk / loss aversion
- **dlPFC**: 抑制 impulsive choice

→ 决策 = 多脑区竞争 / 整合。

---

## 9. PyTorch — TD Learning ↔ DA

```python
import torch

class DASignal:
    """DA 编码 TD error."""
    def __init__(self, gamma=0.95, lr=0.1):
        self.values = {}
        self.gamma = gamma
        self.lr = lr
    
    def step(self, state, reward, next_state):
        v_s = self.values.get(state, 0)
        v_next = self.values.get(next_state, 0)
        # TD error = DA signal
        delta = reward + self.gamma * v_next - v_s
        # Update
        self.values[state] = v_s + self.lr * delta
        return delta  # = DA burst / dip
```

---

## 10. 病理

- **Addiction**: cocaine, opioid, alcohol hijack DA
- **Depression**: 减 reward sensitivity (anhedonia)
- **Schizophrenia (positive)**: DA 过量 (假说)
- **Parkinson**: SNc DA 退化 → 运动 + apathy
- **ADHD**: DA / NE 调节失调

---

## 11. 治疗 / 药物

- **SSRI**: 间接影响 reward
- **Bupropion**: DA + NE reuptake inhibitor
- **Naltrexone**: opioid antagonist (addiction)
- **L-DOPA**: Parkinson
- **DBS NAcc**: 实验治疗严重 OCD / depression

---

## 12. Common Pitfalls

### 12.1 "DA = pleasure" 简化

DA 主要 motivation / wanting,not pleasure 本身 (opioid 给 liking)。

### 12.2 RPE 不是唯一

DA 多 dimensions (salience, novelty)。

### 12.3 NAcc ≠ 唯一 reward area

Multiple regions (OFC, insula, amygdala)。

### 12.4 Addiction ≠ 弱意志

是 brain circuit 改变,非纯心理。

### 12.5 Anhedonia 难量化

主观 — pleasure 难客观测。

---

## 13. Related Concepts

- **同节**:[Motor System](Motor_System.md)、[海马 + 记忆](Hippocampus_Memory.md)
- **解剖**:[Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.md)
- **AI 对比**:[Reinforcement Learning](https://jeffliulab.github.io/ai-notes/04_Reinforcement_Learning/)

---

## References

1. **Schultz, W.** "A neural substrate of prediction and reward." *Science*, 1997.
2. **Berridge, K. C. & Robinson, T. E.** "What is the role of dopamine in reward?" *Brain Res Rev*, 1998.
3. **Sutton, R. S. & Barto, A. G.** *Reinforcement Learning: An Introduction*. MIT, 2018.
4. **Volkow, N. D. et al.** "Addiction: pull of pleasure overrides break of self-control." *Nature Rev Drug Discov*, 2017.
