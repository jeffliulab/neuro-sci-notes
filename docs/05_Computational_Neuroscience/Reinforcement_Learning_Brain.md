# 大脑中的强化学习 (Reinforcement Learning in the Brain)

> *RL 理论与 dopamine 系统的对应是 computational neuroscience 最成功故事之一。Schultz 1997:VTA/SNc dopamine 神经元编码 reward prediction error (RPE),正是 TD learning 的 δ。Basal ganglia 实现 actor-critic。Sutton-Barto RL ↔ brain 双向启发,催生 deep RL。*
>
> **难度**:Advanced
> **前置知识**:[Reward System](../03_Systems_Neuroscience/Reward_System.md)、[Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.md)、MDP 基础

---

## 1. RL 框架

- Agent ↔ environment:state $s$、action $a$、reward $r$
- 目标:最大化累积 reward $\sum \gamma^t r_t$
- Value function $V(s)$、$Q(s,a)$
- TD error:$\delta_t = r_t + \gamma V(s_{t+1}) - V(s_t)$

---

## 2. Dopamine = RPE (Schultz 1997)

经典实验(猴 + juice reward):
- **未学会**:reward 到 → DA burst
- **学会后**:cue 到 → DA burst,reward 到 → 无反应(已预测)
- **省略 reward**:预期时间 → DA dip(负 RPE)

→ 完美匹配 TD error $\delta$。

---

## 3. TD Learning

$$V(s_t) \leftarrow V(s_t) + \alpha \, \delta_t$$
$$\delta_t = r_t + \gamma V(s_{t+1}) - V(s_t)$$

- DA 浓度 ∝ $\delta_t$
- DA 调制 cortico-striatal 突触可塑(三因子规则)

---

## 4. Basal Ganglia = Actor-Critic

- **Critic**: ventral striatum (NAcc) 估 $V(s)$ → 产生 RPE
- **Actor**: dorsal striatum 选 action
- DA RPE 同时训 critic + actor
- 直接/间接通路 ↔ Go/NoGo(见 [Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.md))

---

## 5. Model-free vs Model-based

| | Model-free | Model-based |
|---|---|---|
| 机制 | TD / habit | 规划 / tree search |
| 脑 | dorsolateral striatum | PFC + dorsomedial striatum |
| 特点 | 快、僵 | 慢、灵活 |
| 行为 | habit | goal-directed |

人脑两系统并存(Daw 2011 two-step task)。

---

## 6. PyTorch — TD Learning (DA model)

```python
import numpy as np

def td_learning(rewards, n_states, alpha=0.1, gamma=0.9, episodes=100):
    V = np.zeros(n_states)
    da_signal = []
    for _ in range(episodes):
        for s in range(n_states - 1):
            r = rewards[s]
            delta = r + gamma * V[s+1] - V[s]   # = dopamine RPE
            V[s] += alpha * delta
            da_signal.append(delta)
    return V, da_signal
```

---

## 7. 超越经典 RPE

- **Distributional RL** (Dabney 2020 *Nature*):DA 神经元 heterogeneous → 编码 reward 分布(非仅 mean)!
  - 与 deep RL distributional (C51, QR-DQN) 互证
- **Tonic DA**:平均 reward rate / vigor
- **Risk / uncertainty**:DA 也编码

---

## 8. Serotonin + 其他

- **5-HT**:可能编码 punishment / 时间折扣 / patience
- **ACh**:不确定性 / 学习率
- **NE**:explore-exploit、惊讶
- RL 不只 DA — 多 neuromodulator 协作

---

## 9. AI ↔ Brain 双向

| RL 概念 | 脑对应 |
|---|---|
| TD error | DA RPE |
| Actor-critic | BG dorsal/ventral |
| Distributional RL | DA neuron heterogeneity |
| Eligibility trace | 突触 tag |
| Exploration | NE / tonic DA |
| Replay (DQN) | hippocampal replay |

deep RL(DQN、AlphaGo)直接受 brain RL 启发,又反哺 neuroscience。

---

## 10. Common Pitfalls

### 10.1 DA = 快乐

DA 是 RPE / wanting,非 hedonic pleasure(liking 是 opioid)。

### 10.2 DA 仅编码 mean reward

Distributional RL:编码整个分布。

### 10.3 脑只 model-free

Model-based(PFC 规划)并存。

### 10.4 RPE 解释一切学习

仅 reward learning;监督 / 无监督学习另有机制。

### 10.5 一个 δ 全脑

RPE 有区域特异(不同 striatum 子区不同)。

---

## 11. Related Concepts

- **同节**:[Bayesian Brain](Bayesian_Brain.md)、[Predictive Coding](Predictive_Coding.md)
- **系统**:[Reward System](../03_Systems_Neuroscience/Reward_System.md)
- **解剖**:[Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.md)
- **AI**: RL — https://jeffliulab.github.io/ai-notes/04_Reinforcement_Learning/

---

## References

1. **Schultz, W., Dayan, P., Montague, P. R.** "A neural substrate of prediction and reward." *Science*, 1997.
2. **Sutton, R. S. & Barto, A. G.** *Reinforcement Learning: An Introduction*. 2nd ed., 2018.
3. **Dabney, W. et al.** "A distributional code for value in dopamine-based reinforcement learning." *Nature*, 2020.
4. **Daw, N. D. et al.** "Model-based influences on humans' choices and striatal prediction errors." *Neuron*, 2011.
