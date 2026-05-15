# 漂移扩散模型 (Drift-Diffusion Model)

> *DDM (Ratcliff 1978) 是知觉决策的标准计算模型:证据随机累积到 boundary → 决策。同时拟合 choice + reaction time 分布。神经对应:LIP/FEF ramping activity(Shadlen & Newsome)。是 sequential sampling 家族代表,与 attractor、RL 决策互补。*
>
> **难度**:Advanced
> **前置知识**:[Decision Making](../04_Cognitive_Neuroscience/Decision_Making.md)、随机过程基础

---

## 1. 模型

决策变量 $x$ 随时间累积:
$$dx = v \, dt + \sigma \, dW$$

- $v$:drift rate(证据强度)
- $\sigma dW$:Wiener 噪声
- 两 boundary:$+a$(选 A)、$0$ 或 $-a$(选 B)
- 触界 → 决策 + 用时

---

## 2. 关键参数

| 参数 | 含义 |
|---|---|
| $v$ (drift) | 证据质量 / 难度 |
| $a$ (boundary) | 谨慎度(speed-accuracy) |
| $z$ (start) | 先验偏好 |
| $t_0$ (non-decision) | 编码 + 运动延迟 |

---

## 3. 解释的现象

- **Choice probability**: $P(\text{correct}) = \frac{1}{1+e^{-2va/\sigma^2}}$
- **RT 分布**:右偏(long tail)— DDM 自然预测
- **Speed-accuracy tradeoff**:调 $a$
- **错误 RT**:fast / slow errors

---

## 4. 神经对应

- **LIP / FEF**:ramping firing rate ≈ accumulated evidence(Shadlen & Newsome 2001 random-dot motion)
- 触 threshold firing → saccade
- **SC (superior colliculus)**:触发动作
- Roitman & Shadlen 2002:firing 轨迹 ≈ DDM x(t)

---

## 5. 与 SPRT 最优性

- DDM = 连续版 Sequential Probability Ratio Test
- SPRT:给定 accuracy,最少样本(Wald)
- → DDM 在 2AFC 是统计最优的决策规则
- 大脑近似最优 evidence accumulation

---

## 6. PyTorch — DDM 仿真

```python
import torch

def simulate_ddm(v=0.3, a=1.0, z=0.5, sigma=1.0, dt=0.001, max_t=3.0):
    """Return choice (1/0) and RT."""
    x = z * a
    t = 0.0
    while 0 < x < a and t < max_t:
        x += v * dt + sigma * torch.randn(1).item() * (dt ** 0.5)
        t += dt
    choice = 1 if x >= a else 0
    return choice, t

# Aggregate → choice prob + RT distribution
```

---

## 7. 扩展模型

- **LCA** (Leaky Competing Accumulator, Usher & McClelland):泄漏 + 互抑
- **Race model**:多累加器竞速
- **Collapsing bounds**:boundary 随时间下降(urgency)
- **Attractor model**(Wong-Wang):biophysical 实现 DDM
- 多选项:multi-hypothesis SPRT

---

## 8. 应用

- 知觉决策(motion、对比)
- 价值决策(food choice — vmPFC)
- 记忆检索(认 / 不认)
- 临床:ADHD、aging、抑郁 → drift rate ↓ / boundary 变化
- 拟合工具:HDDM、PyDDM、EZ-diffusion

---

## 9. 与 RL / Attractor 关系

- DDM:abstract sequential sampling
- Wong-Wang attractor:biophysical 机制实现 DDM-like
- RL:value 决定 drift;DDM 决定 how 累积
- 三者互补,非竞争

---

## 10. Common Pitfalls

### 10.1 DDM 仅 2AFC

扩展可多选;但经典是 binary。

### 10.2 Boundary 固定

Collapsing bound / urgency 常更拟合数据。

### 10.3 Ramping = 单 neuron 累积

群体层面;单 neuron 有 stepping 解释争议(Latimer 2015)。

### 10.4 参数唯一可辨识

参数 trade-off;需足够 RT 数据 + 约束。

### 10.5 DDM = 脑确切机制

是 algorithmic-level 描述;biophysical 实现是 attractor。

---

## 11. Related Concepts

- **同节**:[Attractor Networks](Attractor_Networks.md)、[Neural Population Dynamics](Neural_Population_Dynamics.md)、[Reinforcement Learning Brain](Reinforcement_Learning_Brain.md)
- **认知**:[Decision Making](../04_Cognitive_Neuroscience/Decision_Making.md)、[Attention](../04_Cognitive_Neuroscience/Attention.md)
- **AI**: sequential decision、early-exit

---

## References

1. **Ratcliff, R.** "A theory of memory retrieval." *Psychol Rev*, 1978.
2. **Gold, J. I. & Shadlen, M. N.** "The neural basis of decision making." *Annu Rev Neurosci*, 2007.
3. **Roitman, J. D. & Shadlen, M. N.** "Response of neurons in the lateral intraparietal area during a combined visual discrimination reaction time task." *J Neurosci*, 2002.
4. **Bogacz, R. et al.** "The physics of optimal decision making." *Psychol Rev*, 2006.
