# 决策 (Decision-Making) — 神经科学视角

> *决策是 brain 整合 value + risk + 选择 action 的过程。涉及 OFC、vmPFC、insula、ACC、striatum 多脑区。Drift-diffusion model (DDM) 是经典数学框架。与 RL 紧密相关。*
>
> **难度**:Intermediate
> **前置知识**:[奖赏系统](../03_Systems_Neuroscience/Reward_System.md)、[Cortex](../01_Neuroanatomy/Cortex.md)

---

## 1. 决策类型

- **Perceptual**: "向左 vs 向右" 视觉点
- **Value-based**: A vs B 哪个更好
- **Reward-based**: 选 reward 高的 (与 RL)
- **Social**: 信任 / 公平
- **Strategic**: 长 horizon 计划

---

## 2. 神经基础

```
Sensory input → cortex
                   ↓
            OFC / vmPFC (value computation)
                   ↓
            Insula (risk)
                   ↓
            ACC (conflict monitoring)
                   ↓
            Striatum (action selection)
                   ↓
            M1 (motor execution)
```

---

## 3. Drift-Diffusion Model (DDM)

经典 perceptual decision:

$$\frac{dx}{dt} = v + \sigma \eta(t)$$

- $v$: drift rate (evidence accumulation)
- $\sigma$: noise
- 上 boundary → 选 A
- 下 boundary → 选 B

预测 RT distribution + accuracy。

---

## 4. OFC / vmPFC

- OFC: 计算 value (gluttony Hippy 食物 / 视觉刺激)
- vmPFC: 比较 options (A vs B)
- Damage → 决策异常 (Phineas Gage)

---

## 5. ACC (Anterior Cingulate)

- Conflict monitoring (Stroop test 高 active)
- 错误检测
- Effort 估计

---

## 6. Insula

- 负面情绪 / loss
- Disgust
- Risk aversion

---

## 7. 决策偏差 (Behavioral Economics)

- **Loss aversion**: 损失 hurt > gain 喜悦
- **Sunk cost**: 已付 cost 不该影响 future decision
- **Confirmation bias**: 只看 supportive info
- **Anchoring**: 先 see number 影响后判断

→ Kahneman / Tversky 2002 Nobel Economics。

---

## 8. PyTorch — DDM 模拟

```python
import torch

class DDM:
    def __init__(self, drift=0.3, noise=1.0, threshold=2.0):
        self.drift, self.noise, self.threshold = drift, noise, threshold
    
    def simulate(self, max_steps=10000, dt=0.01):
        x = 0
        for t in range(max_steps):
            dx = self.drift * dt + self.noise * torch.randn(1).item() * (dt ** 0.5)
            x += dx
            if x >= self.threshold:
                return 'A', t * dt
            if x <= -self.threshold:
                return 'B', t * dt
        return 'undecided', max_steps * dt

ddm = DDM()
choice, rt = ddm.simulate()
```

---

## 9. 病理

- **Frontal lobe damage** (Phineas Gage): 性格 + 决策异常
- **OFC damage**: Iowa Gambling Task 失败 (Bechara 1994)
- **Addiction**: 短期 reward > 长期 cost
- **Depression**: indecision + 悲观 bias
- **Pathological gambling**: insula 异常

---

## 10. AI 关联

- LLM decision making: tool use, function calling
- RL with reward shaping
- Inverse RL: 从行为反推 reward function

---

## 11. Common Pitfalls

### 11.1 DDM 简化

只 1 维 evidence;real decision 多维。

### 11.2 vmPFC ≠ "决策中心"

Distributed network。

### 11.3 Behavioral 偏差不是 bug

许多 bias 是 evolved heuristic,某些情境合理。

### 11.4 RT ≠ confidence

短 RT 不一定 confidence 高。

### 11.5 fMRI ≠ causal

fMRI 仅 correlate; lesion / TMS 才证 causal。

---

## 12. Related Concepts

- **同节**:[语言](Language.md)、[意识](Consciousness.md)
- **系统**:[奖赏系统](../03_Systems_Neuroscience/Reward_System.md)
- **AI**: [RL](https://jeffliulab.github.io/ai-notes/04_Reinforcement_Learning/)

---

## References

1. **Ratcliff, R.** "A theory of memory retrieval (DDM)." *Psychol Rev*, 1978.
2. **Bechara, A. et al.** "Insensitivity to future consequences (Iowa Gambling Task)." *Cognition*, 1994.
3. **Kahneman, D.** *Thinking, Fast and Slow*. 2011.
4. **Glimcher, P. W. & Fehr, E.** *Neuroeconomics*. 2nd ed., 2014.
