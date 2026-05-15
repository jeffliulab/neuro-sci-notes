# 脑节律 (Brain Rhythms & Oscillations)

> *神经振荡是大量 neuron 同步活动产生的节律(delta 到 gamma)。Buzsáki 提出振荡是 brain 的"语法"。功能:temporal coordination、binding、memory(theta-gamma coupling)、attention(alpha)。Cross-frequency coupling、phase coding 是前沿。AI:振荡 ↔ 同步/门控机制启发。*
>
> **难度**:Intermediate
> **前置知识**:[EEG](../07_Neurotech_Frontiers/EEG.md)、[Neural Coding](Neural_Coding.md)

---

## 1. 频段一览

| 节律 | Hz | 主要状态 |
|---|---|---|
| Delta | 0.5-4 | 深睡眠 |
| Theta | 4-8 | 海马、导航、记忆、REM |
| Alpha | 8-13 | 放松、闭眼、抑制 |
| Beta | 13-30 | 运动准备、active |
| Gamma | 30-100+ | binding、attention、local 处理 |
| Ripples | 80-200 | 海马 SWR(记忆 replay) |

---

## 2. 振荡如何产生

- **E-I loop**: 兴奋-抑制反馈 → 振荡(PING / ING 模型)
- Interneuron(尤 PV+)是节律 pacemaker
- Thalamocortical loop → spindle、alpha
- Pacemaker neuron(Ih 电流)

---

## 3. 功能假说

### 3.1 Temporal binding

- Gamma 同步把分散特征"绑"为一个 object(Singer)

### 3.2 Communication through coherence (Fries)

- 两区 gamma 相位对齐 → 有效通信
- 相位错位 → 信息被门控掉

### 3.3 Theta-gamma coupling

- 一个 theta 周期内嵌 ~ 7 个 gamma → working memory items(Lisman & Idiart)

### 3.4 Phase coding

- Spike 相对 theta 相位编码位置(hippocampal phase precession)

---

## 4. 海马 SWR (Sharp-Wave Ripple)

- 80-200 Hz 短爆发
- 静息 / 慢波睡眠期
- 压缩 replay 经验序列 → memory consolidation
- 见 [Hippocampus Memory](../03_Systems_Neuroscience/Hippocampus_Memory.md)

---

## 5. PyTorch — Theta-Gamma Coupling 生成

```python
import numpy as np

def theta_gamma_signal(T=2.0, fs=1000):
    """Phase-amplitude coupling: gamma amplitude modulated by theta phase."""
    t = np.arange(0, T, 1/fs)
    theta = np.sin(2*np.pi*6*t)              # 6 Hz theta
    gamma_amp = 0.5 * (1 + theta)            # theta modulates gamma amplitude
    gamma = gamma_amp * np.sin(2*np.pi*40*t) # 40 Hz gamma
    return t, theta + gamma
```

→ Modulation Index (Tort 2010) 量化 coupling 强度。

---

## 6. Cross-Frequency Coupling

- **Phase-amplitude (PAC)**: 慢波相位调制快波幅(最常见)
- **Phase-phase**
- **Amplitude-amplitude**
- PAC 是 multi-scale 协调的标志

---

## 7. 振荡 + 认知

| 节律 | 认知关联 |
|---|---|
| Frontal theta | 认知控制、错误监测 |
| Posterior alpha | 注意抑制(忽略无关) |
| Beta | 维持现状(motor / cognitive set) |
| Gamma | 局部处理、attention |
| SWR | 记忆 consolidation + planning |

---

## 8. 病理

- **Parkinson**: 病理性 beta 过强(STN)→ DBS 抑制
- **Epilepsy**: 异常超同步
- **Schizophrenia**: gamma + spindle 异常(PV interneuron)
- **Alzheimer**: gamma 减弱 → 40 Hz 光/声刺激实验(GENUS)
- **Depression**: frontal alpha 不对称

---

## 9. AI 类比

- **Gating / attention** ↔ communication through coherence
- **Oscillation in RNN**:某些 RNN 自发振荡
- **Binding problem**:AI 也面临(slot attention、object-centric)
- 但主流 ANN 无显式振荡机制

---

## 10. Common Pitfalls

### 10.1 振荡 = epiphenomenon

争议:有人认为副产品;但 causal 实验(optogenetic)显示功能性。

### 10.2 频段边界绝对

频段是连续谱的人为划分;个体差异大。

### 10.3 Gamma = 意识

关联 attention/binding,但非充分;NCC 仍 open。

### 10.4 更高频 = 更高级处理

不同频率不同功能,无层级。

### 10.5 EEG 节律 = 单 neuron 节律

EEG 是群体同步;单 neuron 可不规则。

---

## 11. Related Concepts

- **同节**:[Neural Coding](Neural_Coding.md)、[Levels of Analysis](Levels_of_Analysis.md)
- **前沿**:[EEG](../07_Neurotech_Frontiers/EEG.md)、[DBS](../07_Neurotech_Frontiers/DBS.md)
- **系统**:[Hippocampus Memory](../03_Systems_Neuroscience/Hippocampus_Memory.md)、[Sleep_Wake](../03_Systems_Neuroscience/Sleep_Wake.md)

---

## References

1. **Buzsáki, G.** *Rhythms of the Brain*. Oxford, 2006.
2. **Fries, P.** "Rhythms for cognition: communication through coherence." *Neuron*, 2015.
3. **Lisman, J. E. & Jensen, O.** "The theta-gamma neural code." *Neuron*, 2013.
4. **Tort, A. B. L. et al.** "Measuring phase-amplitude coupling between neuronal oscillations." *J Neurophysiol*, 2010.
