# 时间知觉 (Time Perception)

> *脑无单一"时钟"。多尺度时间机制:毫秒(小脑/网络状态)、秒-分(纹状体多巴胺,SBF 模型)、昼夜(SCN)、interval timing(scalar timing,Weber 律)。时间知觉受注意、情绪、多巴胺强烈调制("时间飞逝/度日如年")。是 RL、运动、语言节律的底层。*
>
> **难度**:Intermediate
> **前置知识**:[Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.md)、[Cerebellum](../01_Neuroanatomy/Cerebellum.md)

---

## 1. 多尺度时间

| 尺度 | 机制 | 区域 |
|---|---|---|
| 微秒-ms | 听觉双耳时差 | 脑干 |
| 数百 ms | 网络状态 / 小脑 | 小脑、皮层 |
| 秒-分(interval) | 多巴胺 + 纹状体振荡 | BG、SMA、PFC |
| 昼夜 | 分子钟(SCN) | 下丘脑(见 [Sleep_Wake](../03_Systems_Neuroscience/Sleep_Wake.md)) |

→ 无单一中央时钟(distributed)。

---

## 2. Scalar Timing (SET)

- Gibbon 行为模型:pacemaker → accumulator → 比较
- **Scalar property**:计时误差 ∝ 时长(Weber 律,变异系数恒定)
- 经典 internal clock 隐喻(虽机制非字面起搏器)

---

## 3. Striatal Beat-Frequency (SBF)

- 皮层振荡器群(不同频率)
- 纹状体 detect 振荡相位**重合模式** → 编码时长
- 多巴胺调 pacemaker 速率 → 解释 DA 药改变时间感
- 主流 interval timing 神经模型(Matell & Meck)

---

## 4. PyTorch — SBF 重合检测

```python
import torch

def striatal_beat_frequency(t, freqs, target_T):
    """Cortical oscillators; striatum learns coincidence at target_T."""
    osc = torch.stack([torch.cos(2*torch.pi*f*t) for f in freqs])  # (F, T)
    # Striatal MSN: weighted detector tuned to phase pattern at target_T
    pattern_at_T = torch.cos(2*torch.pi*freqs*target_T)
    readout = (osc * pattern_at_T.unsqueeze(1)).sum(0)
    return readout   # peaks when oscillator phases match those at target_T
```

---

## 5. 主观时间扭曲

- **注意**:计时 ↔ 主任务竞争资源 → 注意分散 → 低估时长
- **情绪**:威胁/恐惧 → 时间"变慢"(高唤醒 → 时钟加速)
- **多巴胺**:↑ DA(兴奋剂)→ 高估;↓(PD)→ 低估
- **年龄**:成年后主观时间"加快"(比例 / 新异性假说)
- Oddball effect:新异刺激"时间膨胀"

---

## 6. 节律 vs 时长

- **Beat-based**(节拍,音乐):BG 依赖
- **Duration-based**(单间隔):小脑 + BG
- 运动时间(motor timing)vs 感知时间 部分分离
- 与语言节律、音乐(见 [Auditory_System](../03_Systems_Neuroscience/Auditory_System.md))

---

## 7. 病理

- **Parkinson**:DA↓ → interval timing 受损(低估 / 变异↑)
- **小脑损伤**:ms 级运动时序差
- **Schizophrenia**:时间知觉异常(与 agency 缺陷关联)
- **ADHD**:时间感知 + delay aversion(见 [ADHD](../08_Neuro_Disorders/ADHD.md))
- **抑郁**:主观时间"变慢"

---

## 8. 与 RL / AI

- 时间折扣(temporal discounting)依赖时间知觉
- TD learning 需时间信用分配(eligibility trace,见 [Reinforcement Learning Brain](../05_Computational_Neuroscience/Reinforcement_Learning_Brain.md))
- RNN 自发产生 timing(ramping / 群体动力学,见 [Neural Population Dynamics](../05_Computational_Neuroscience/Neural_Population_Dynamics.md))
- 序列模型位置/时间编码 ↔ 神经计时

---

## 9. 测量范式

- 时长辨别 / 二分(bisection)
- 时长复制(reproduction)
- 节拍同步敲击(tapping)
- 口头估计
- 各范式分离不同机制(motor vs perceptual)

---

## 10. Common Pitfalls

### 10.1 脑有单一时钟

多尺度分布机制;无中央钟。

### 10.2 SBF 字面起搏器

是振荡重合模型;"pacemaker"是计算抽象。

### 10.3 主观 = 客观时间

强受注意/情绪/DA 调制(系统性扭曲)。

### 10.4 节律 = 时长同机制

beat-based vs duration-based 部分分离。

### 10.5 时间知觉与 RL 无关

时间折扣 + 信用分配核心依赖计时。

---

## 11. Related Concepts

- **同节**:[Decision_Making](Decision_Making.md)、[Attention](Attention.md)、[Working Memory](Working_Memory.md)
- **解剖**:[Basal Ganglia](../01_Neuroanatomy/Basal_Ganglia.md)、[Cerebellum](../01_Neuroanatomy/Cerebellum.md)
- **计算**:[Reinforcement Learning Brain](../05_Computational_Neuroscience/Reinforcement_Learning_Brain.md)
- **系统**:[Sleep_Wake](../03_Systems_Neuroscience/Sleep_Wake.md)

---

## References

1. **Buhusi, C. V. & Meck, W. H.** "What makes us tick? Functional and neural mechanisms of interval timing." *Nat Rev Neurosci*, 2005.
2. **Gibbon, J. et al.** "Toward a neurobiology of temporal cognition." *Curr Opin Neurobiol*, 1997.
3. **Matell, M. S. & Meck, W. H.** "Cortico-striatal circuits and interval timing (SBF)." *Cogn Brain Res*, 2004.
4. **Eagleman, D. M.** "Human time perception and its illusions." *Curr Opin Neurobiol*, 2008.
