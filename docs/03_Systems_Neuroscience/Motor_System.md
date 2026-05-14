# 运动系统 — M1, Premotor, BG, 小脑协同

> *运动系统从 cortex M1 → 脊髓 → 肌肉,但远不止简单 motor command。Premotor / SMA 规划、BG 选择、小脑校准、PPC 集成感觉。本篇覆盖各组件 + population coding + AI 对比。*
>
> **难度**:Intermediate
> **前置知识**:[Cortex](../01_Neuroanatomy/Cortex.md)、[Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.md)、[Cerebellum](../01_Neuroanatomy/Cerebellum.md)

---

## 1. 系统组件

```
PFC → Premotor / SMA (planning)
       ↓
    M1 (execution)
       ↓
  Brainstem / Spinal cord → motor neurons → muscles
       ↑                       ↑
  Cerebellum (timing)    BG (action selection)
       ↑
  PPC + S1 (sensory feedback)
```

---

## 2. M1 (Primary Motor Cortex)

### 2.1 Motor Homunculus

Penfield 1937:M1 表面有 body map:
- 大区:手 / 脸 (精细控制)
- 小区:躯干 / 腿

### 2.2 Output

Layer 5 pyramidal → corticospinal tract → 脊髓 motor neurons。

### 2.3 Population Vector

Georgopoulos 1986:单个 neuron 偏好方向,population vector 编码 movement direction。
→ N neurons sum, direction = arg max。

启发 BCI:从 M1 神经元 population 解码意图运动。

---

## 3. Premotor / SMA

- **Premotor**: 接收 PPC + visual input, plan reach trajectory
- **SMA**: internally triggered actions, motor sequence
- **dlPFC**: high-level goal

---

## 4. 小脑

参与:
- Timing (smooth movement)
- Motor learning (adaptation)
- Forward model:预测 motor command 的 sensory consequence
- 损 → ataxia

---

## 5. 基底神经节

- Action selection
- 习惯 formation
- Reward-based learning

---

## 6. 反馈控制

```
Motor command → muscle → movement → sensors (proprioception, vision)
                                              ↓
                       Compared to predicted (forward model)
                                              ↓
                       Error → corrective adjustment
```

→ Internal Model Theory (Kawato 1999)。

---

## 7. BCI 应用

从 M1 解码意图运动 → 控制机械臂 / 光标:
- Utah array (96 channels) 1990s
- BrainGate clinical 2006+
- Neuralink 1024 channels 2024

---

## 8. PyTorch — Population Coding

```python
import torch

class M1Population:
    def __init__(self, n_neurons=100, preferred_directions=None):
        self.n = n_neurons
        if preferred_directions is None:
            self.pds = torch.linspace(0, 2 * torch.pi, n_neurons + 1)[:-1]
        else:
            self.pds = preferred_directions
    
    def encode(self, direction, noise=0.1):
        """Cosine tuning + noise."""
        # Each neuron responds based on cos(direction - PD)
        responses = torch.cos(direction - self.pds)
        responses = torch.relu(responses) + noise * torch.randn(self.n)
        return responses
    
    def decode(self, responses):
        """Population vector decoding."""
        x = (responses * torch.cos(self.pds)).sum()
        y = (responses * torch.sin(self.pds)).sum()
        return torch.atan2(y, x)
```

---

## 9. 病理

- **Stroke (M1)**: 对侧肢体瘫痪
- **ALS**: motor neuron 退化
- **Spinal cord injury**: 切断 cortex → muscle 通路
- **Parkinson**: BG 退化 → 启动困难
- **Cerebellar ataxia**: 协调失调

---

## 10. Common Pitfalls

### 10.1 M1 ≠ 全部运动

Subcortical (BG, cerebellum, brainstem) 不可缺。

### 10.2 Population coding 不唯一

Rate, timing, dynamics 都参与。

### 10.3 Reflex vs voluntary

膝跳反射 不经 cortex;voluntary motion 经全套。

### 10.4 Open-loop vs closed-loop

精细动作严依赖 feedback;弹钢琴等 fast 动作部分 open-loop。

### 10.5 BCI 输出难

Hand reach decoding ~ 90%,但 fine finger / speech 困难。

---

## 11. Related Concepts

- **同节**:[Visual System](Visual_System.md)、[海马 + 记忆](Hippocampus_Memory.md)
- **解剖**:[Cortex](../01_Neuroanatomy/Cortex.md)、[Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.md)、[Cerebellum](../01_Neuroanatomy/Cerebellum.md)
- **BCI**:[BCI Foundations](../06_Brain_Computer_Interface/01_Foundations/index.md)

---

## References

1. **Georgopoulos, A. P. et al.** "Neural coding of movement direction." *Science*, 1986.
2. **Penfield, W. & Boldrey, E.** "Somatic motor and sensory representation in the cerebral cortex." *Brain*, 1937.
3. **Kawato, M.** "Internal models for motor control and trajectory planning." *Curr Opin Neurobiol*, 1999.
4. **Kandel, E. R. et al.** *Principles of Neural Science*. 6th ed., 2021.
