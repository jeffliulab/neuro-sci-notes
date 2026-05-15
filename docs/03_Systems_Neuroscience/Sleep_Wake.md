# 睡眠 / 唤醒 (Sleep / Wake) 系统

> *睡眠是 brain 必需 active 过程,不是 passive 关机。NREM (4 阶段) + REM 循环,各有 distinct 神经基础。Memory consolidation, glymphatic 清理,免疫 regulation 等关键功能。失眠 / 异常 与多疾病相关。*
>
> **难度**:Intermediate
> **前置知识**:[Brainstem](../01_Neuroanatomy/Brainstem.md)、[神经递质](../02_Cellular_Molecular/Neurotransmitters.md)

---

## 1. 睡眠阶段

```
Wake → N1 → N2 → N3 (deep, slow-wave) → REM → wake or back
        (~ 90 min cycle, 4-6 cycles / night)
```

### 1.1 NREM (Non-REM)

- **N1**: 过渡期, 5%
- **N2**: spindle + K-complex, 45%
- **N3**: slow-wave sleep (SWS), delta wave, 25%
- 体温下降, HR / BP 降

### 1.2 REM (Rapid Eye Movement)

- Dreams 主发生地
- Brain activity 接近清醒
- 肌肉麻痹 (atonia, prevent acting out dreams)
- 25% of total sleep

---

## 2. EEG 特征

| 阶段 | EEG | freq | amplitude |
|---|---|---|---|
| Wake | beta, alpha | 8-30 Hz | low |
| N1 | theta | 4-8 Hz | low |
| N2 | spindles + K-complex | 12-14 Hz spindle | medium |
| N3 | delta | 0.5-4 Hz | very high |
| REM | beta + theta | similar to wake | low |

---

## 3. 神经调控

**唤醒** (ascending arousal system):
- **ACh** (basal forebrain, brainstem PPT)
- **NE** (locus coeruleus)
- **5-HT** (raphe)
- **DA** (VTA)
- **Histamine** (TMN, hypothalamus)
- **Orexin** (hypocretin, hypothalamus) — flip-flop switch

**睡眠**:
- **VLPO** (ventrolateral preoptic nucleus, hypothalamus) → GABA → 抑制 arousal nuclei

→ Flip-flop switch model (Saper 2001)。

---

## 4. 主观与客观

- **Subjective**: tired feeling
- **Objective**: 
  - PSG (polysomnography): EEG + EOG (眼) + EMG (肌)
  - Actigraphy (wrist activity)
  - Sleep diary

---

## 5. 睡眠功能

### 5.1 Memory Consolidation

- SWS (N3): declarative memory 巩固 (hippocampal replay)
- REM: procedural / emotional memory
- 学习后 sleep ≠ no sleep (memory 测试 50%+ 提升)

### 5.2 Glymphatic Clearance

- 睡眠时 astrocyte aquaporin 4 → CSF 冲走 β-amyloid
- 睡眠不足 → amyloid 累积 → Alzheimer 风险

### 5.3 Synaptic Homeostasis

- Tononi:清醒时 LTP 累积 → 睡眠 LTD 平衡
- 维持网络效率

### 5.4 其他

- 免疫 regulation
- Metabolic restoration
- Hormone (growth hormone, cortisol)

---

## 6. 睡眠剥夺

- 24 hr 不睡 → 认知如 BAC 0.10 (酒驾)
- 持续 → 微 sleep (involuntary blink-second sleep)
- 长期 → 心血管, 免疫, 认知 decline
- Fatal familial insomnia (基因病, 完全失眠致死)

---

## 7. Circadian Rhythm

- ~ 24 hr cycle, 由 SCN (suprachiasmatic nucleus) 主导
- Light reset (melatonin from pineal)
- 个体差异:morning lark vs night owl
- Jet lag, shift work 干扰

---

## 8. 睡眠 disorders

- **失眠 (Insomnia)**: 入睡 / 维持困难
- **OSA (Obstructive Sleep Apnea)**: 喉部塌陷, snoring + apnea, 心血管 risk
- **Narcolepsy**: orexin 缺 → 白天 sudden sleep + cataplexy
- **REM Behavior Disorder**: REM atonia 失 → 演 dreams (踢, 跑) — 早期 Parkinson 信号
- **Restless legs syndrome**

---

## 9. PyTorch — Sleep Stage Classifier (toy)

```python
import torch
import torch.nn as nn

class SleepClassifier(nn.Module):
    """5-class sleep stage classifier from 30s EEG epoch."""
    def __init__(self, in_features=128, n_stages=5):
        super().__init__()
        # CNN for spectral features
        self.cnn = nn.Sequential(
            nn.Conv1d(1, 32, 5), nn.ReLU(),
            nn.MaxPool1d(2),
            nn.Conv1d(32, 64, 5), nn.ReLU(),
            nn.AdaptiveAvgPool1d(16),
        )
        # LSTM for temporal context
        self.lstm = nn.LSTM(64, 64, batch_first=True)
        self.fc = nn.Linear(64, n_stages)
    
    def forward(self, eeg_30s):
        feat = self.cnn(eeg_30s)  # (B, 64, 16)
        seq = feat.transpose(1, 2)
        out, _ = self.lstm(seq)
        return self.fc(out[:, -1])
```

---

## 10. AI 应用

- **Apple Watch / Fitbit sleep tracking**: actigraphy + heart rate
- **AASM scoring auto**: DL classify EEG → AASM stages
- **CPAP**: Auto-adjust 治 OSA
- **Sleep coaching apps**: cognitive behavioral therapy for insomnia (CBT-I)

---

## 11. Common Pitfalls

### 11.1 Sleep ≠ Passive

Active brain process。

### 11.2 8 hr ≠ universal

成年需 7-9 hr;青少年 8-10;老年 7-8。

### 11.3 Coffee / 酒影响 sleep architecture

酒 → REM 抑制;咖啡 → SWS 抑制 (long-acting)。

### 11.4 Naps

> 30 min nap → sleep inertia;ideal 10-20 min。

### 11.5 Sleep tracker accuracy

Consumer wearable 误差 大;PSG 是 gold standard。

---

## 12. Related Concepts

- **同节**:[Hippocampus + Memory](Hippocampus_Memory.md)、[Reward System](Reward_System.md)
- **解剖**:[Brainstem](../01_Neuroanatomy/Brainstem.md)
- **疾病**:[Alzheimer](../08_Neuro_Disorders/Alzheimer.md)、[Depression](../08_Neuro_Disorders/Depression.md)

---

## References

1. **Saper, C. B. et al.** "The sleep switch: hypothalamic control of sleep and wakefulness." *Trends Neurosci*, 2001.
2. **Walker, M.** *Why We Sleep*. 2017.
3. **Diekelmann, S. & Born, J.** "The memory function of sleep." *Nat Rev Neurosci*, 2010.
4. **Xie, L. et al.** "Sleep drives metabolite clearance from the adult brain (glymphatic)." *Science*, 2013.
