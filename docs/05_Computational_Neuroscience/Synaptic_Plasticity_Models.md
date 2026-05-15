# 突触可塑性模型 (Synaptic Plasticity Models)

> *从 Hebb (1949) 到 STDP、BCM、三因子规则。可塑性 rule 是 brain learning 的数学核心。问题:纯 Hebbian 不稳定 → 需 normalization (Oja)、滑动阈值 (BCM)、neuromodulator gating (三因子)。这些 local rule 是 backprop 的 biologically-plausible 替代,也启发 neuromorphic 学习。*
>
> **难度**:Advanced
> **前置知识**:[LTP_LTD](../02_Cellular_Molecular/LTP_LTD.md)、[Neuroplasticity](../00_Foundations/Neuroplasticity.md)

---

## 1. Hebbian 与其问题

$$\Delta w = \eta \, x \, y$$

- 共激活 → 增强
- 问题:**正反馈 → 无界增长**(w → ∞)
- 需稳定机制

---

## 2. Oja's Rule (normalization)

$$\Delta w = \eta \, y (x - y \, w)$$

- Hebbian + 隐式归一化
- 收敛到输入协方差主特征向量(PCA!)
- 权重 norm 自然有界

---

## 3. BCM Rule (滑动阈值)

$$\Delta w = \eta \, x \, y (y - \theta_M), \quad \theta_M = \langle y^2 \rangle$$

- 阈 $\theta_M$ 随平均活动滑动
- $y > \theta_M$ → LTP;$y < \theta_M$ → LTD
- 稳定 + 解释 ocular dominance plasticity
- 与 metaplasticity 对应

---

## 4. STDP (Spike-Timing-Dependent)

- pre 先于 post(~ ms)→ LTP
- post 先于 pre → LTD
- 时间不对称窗(指数)
- 因果性学习;见 [LTP_LTD](../02_Cellular_Molecular/LTP_LTD.md)、[SNN](Spiking_Neural_Networks.md)

$$\Delta w = \begin{cases} A_+ e^{-\Delta t/\tau_+} & \Delta t > 0 \\ -A_- e^{\Delta t/\tau_-} & \Delta t < 0 \end{cases}$$

---

## 5. 三因子规则 (Three-Factor)

$$\Delta w = \eta \, \underbrace{x \, y}_{\text{Hebbian}} \cdot \underbrace{M}_{\text{neuromodulator}}$$

- 第三因子 $M$:dopamine / ACh / 奖励信号
- 解决 credit assignment(何时该学)
- 突触 eligibility trace + 延迟 reward → reward-modulated STDP
- 连接 RL(见 [Reinforcement Learning Brain](Reinforcement_Learning_Brain.md))

---

## 6. PyTorch — BCM Rule

```python
import torch

def bcm_update(w, x, lr=0.01, tau_theta=100):
    """BCM with sliding threshold."""
    theta = bcm_update.theta if hasattr(bcm_update, 'theta') else 0.0
    y = (w * x).sum()
    dw = lr * x * y * (y - theta)
    # Sliding threshold ~ running average of y^2
    theta = theta + (y**2 - theta) / tau_theta
    bcm_update.theta = theta
    return w + dw
```

---

## 7. Homeostatic Plasticity

- **Synaptic scaling**:全局缩放维持目标 firing rate(Turrigiano)
- 慢(小时-天)、乘法性
- 防 Hebbian runaway,保稳定
- 与 fast Hebbian 互补(快学 + 慢稳)

---

## 8. 生物 plausible 学习

| 规则 | 解决 |
|---|---|
| Oja | 稳定 + PCA |
| BCM | 阈值 metaplasticity |
| STDP | 时序因果 |
| Three-factor | credit assignment / RL |
| Homeostatic | 全局稳定 |
| Feedback alignment | backprop 的 bio 替代 |
| Predictive coding | 局部误差 → 近似 backprop |

---

## 9. 与 Backprop

- Backprop 非 bio plausible:weight transport、全局误差、双向
- 替代:feedback alignment、target prop、predictive coding、equilibrium prop
- 这些用 **local** rule 近似梯度
- 是 neuromorphic / on-chip learning 关键

---

## 10. Common Pitfalls

### 10.1 Hebbian 即够

纯 Hebbian 发散;需稳定机制。

### 10.2 STDP 普适

STDP 窗 form 依 synapse / 区域不同;非单一曲线。

### 10.3 一规则解释全部

不同 synapse / 任务用不同 + 组合规则。

### 10.4 Plasticity = LTP/LTD only

含 intrinsic、structural、homeostatic 多型。

### 10.5 Three-factor = backprop

仍是 local + 全局标量;非完整梯度。

---

## 11. Related Concepts

- **同节**:[Spiking Neural Networks](Spiking_Neural_Networks.md)、[Hopfield Networks](Hopfield_Networks.md)、[Reinforcement Learning Brain](Reinforcement_Learning_Brain.md)
- **细胞**:[LTP_LTD](../02_Cellular_Molecular/LTP_LTD.md)、[Synapse](../02_Cellular_Molecular/Synapse.md)
- **基础**:[Neuroplasticity](../00_Foundations/Neuroplasticity.md)
- **AI**: bio-plausible learning、neuromorphic

---

## References

1. **Bienenstock, E. L., Cooper, L. N., Munro, P. W.** "Theory for the development of neuron selectivity (BCM)." *J Neurosci*, 1982.
2. **Oja, E.** "A simplified neuron model as a principal component analyzer." *J Math Biol*, 1982.
3. **Frémaux, N. & Gerstner, W.** "Neuromodulated spike-timing-dependent plasticity and theory of three-factor learning rules." *Front Neural Circuits*, 2016.
4. **Turrigiano, G. G.** "The self-tuning neuron: synaptic scaling of excitatory synapses." *Cell*, 2008.
