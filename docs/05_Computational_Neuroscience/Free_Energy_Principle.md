# 自由能原理 (Free Energy Principle)

> *Karl Friston 的 Free Energy Principle (FEP):任何自维持系统通过最小化 variational free energy(感觉惊讶的上界)来抵抗熵增。统一感知(perceptual inference)、学习、行动(active inference)。极具雄心但也极受争议(不可证伪批评)。是 predictive coding 的更一般框架。*
>
> **难度**:Advanced
> **前置知识**:[Predictive Coding](Predictive_Coding.md)、[Bayesian Brain](Bayesian_Brain.md)、变分推断

---

## 1. 核心主张

- 生物体须维持在有限状态(homeostasis)→ 抵抗熵增
- 不能直接最小化 surprise(需知道所有可能感觉)
- 但可最小化其上界:**variational free energy** $F$
- 感知 + 学习 + 行动 都是最小化 $F$

---

## 2. 数学

Surprise(负 log evidence):$-\ln p(o)$

Variational free energy:
$$F = \underbrace{D_{KL}[q(s) \| p(s|o)]}_{\geq 0} - \ln p(o)$$

$$F = \underbrace{\mathbb{E}_q[-\ln p(o|s)]}_{\text{accuracy}} + \underbrace{D_{KL}[q(s)\|p(s)]}_{\text{complexity}}$$

- $q(s)$:大脑的 recognition density(近似后验)
- 最小化 $F$ ≈ 变分贝叶斯推断
- $F \geq -\ln p(o)$(surprise 上界)

---

## 3. 三种最小化途径

| 途径 | 改变 | 对应 |
|---|---|---|
| Perception | $q(s)$ | 推断隐藏状态 |
| Learning | 参数 | 慢权重更新 |
| Action | 感觉本身 | active inference |

---

## 4. Active Inference

- 不止被动推断 → **行动改变感觉**使其符合预测
- 取代经典 RL 的 reward:用 prior preference(期望观测)
- "我预期看到食物 → 行动实现该预测"
- Reward = prior preference 的 log

---

## 5. 与 Predictive Coding 关系

- Predictive coding 是 FEP 在层级高斯模型下的特例
- Prediction error = free energy 的梯度
- FEP 更一般(含 action、learning)

---

## 6. PyTorch — 变分自由能最小化

```python
import torch

def free_energy(obs, q_mean, q_logvar, prior_mean=0.0, prior_var=1.0,
                likelihood_var=0.1):
    """F = accuracy + complexity (single latent demo)."""
    q_var = q_logvar.exp()
    # Accuracy: expected neg log likelihood (predict obs from q_mean)
    accuracy = ((obs - q_mean) ** 2).mean() / (2 * likelihood_var)
    # Complexity: KL[q || prior]
    kl = 0.5 * ((q_var + (q_mean - prior_mean)**2) / prior_var
                - 1 - q_logvar + torch.log(torch.tensor(prior_var)))
    return accuracy + kl.mean()
```

→ 与 VAE 的 ELBO 数学同形(F = -ELBO)。

---

## 7. 与 VAE / ELBO

- Variational free energy = 负 ELBO(VAE 训练目标)
- 大脑 ≈ 实时变分自编码器(假说)
- 但 brain 用 local message passing(非 backprop)

---

## 8. Markov Blanket

- 系统与环境的统计边界(internal vs external states 通过 sensory + active states 交互)
- FEP:任何持存系统有 Markov blanket
- 从细胞到大脑到生态的尺度不变描述(争议点)

---

## 9. 批评

- **不可证伪**:批评者称太一般 → 无法被实验否定(Colombo & Wright)
- **数学 ≠ 机制**:能 fit 任何行为
- **Reward 重述**:active inference 是否真优于 RL?
- 支持者:统一框架价值 + 已有具体可测预测(PC 实验)

---

## 10. Common Pitfalls

### 10.1 FEP = Predictive Coding

PC 是 FEP 特例;FEP 含 action + learning。

### 10.2 Free energy = 热力学自由能

是 variational free energy(信息论),不同概念(借用术语)。

### 10.3 已被证实

仍争议;部分预测(PC)有支持,整体框架不可证伪批评未解。

### 10.4 取代 RL

Active inference 与 RL 数学可互译;非简单取代。

### 10.5 minimize surprise = 求安逸(dark room problem)

Prior preference + exploration(epistemic value)解释为何不躲暗室。

---

## 11. Related Concepts

- **同节**:[Predictive Coding](Predictive_Coding.md)、[Bayesian Brain](Bayesian_Brain.md)
- **认知**:[Consciousness](../04_Cognitive_Neuroscience/Consciousness.md)
- **AI**: VAE / ELBO、active inference agents

---

## References

1. **Friston, K.** "The free-energy principle: a unified brain theory?" *Nat Rev Neurosci*, 2010.
2. **Friston, K. et al.** "Active inference and learning." *Neurosci Biobehav Rev*, 2016.
3. **Buckley, C. L. et al.** "The free energy principle for action and perception: A mathematical review." *J Math Psychol*, 2017.
4. **Colombo, M. & Wright, C.** "First principles in the life sciences: the free-energy principle, organisms, and machines." *Synthese*, 2021.
